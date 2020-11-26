---
title: Add AiBlocks To Your Exchange
replacement: https://developers.aiblocks.io/docs/tutorials/exchange/
---

This guide describes how to add tokens from the AiBlocks network to your exchange. First, we walk through adding AiBlocks's native asset, delos. Following that, we describe how to add other tokens. This example uses Node.js and the [JS AiBlocks SDK](https://github.com/aiblocks/js-aiblocks-sdk), but it should be easy to adapt to other languages.

There are many ways to architect an exchange. This guide uses the following design:
 - `issuing account`: One AiBlocks account that holds the majority of customer deposits offline.
 - `base account`: One AiBlocks account that holds a small amount of customer deposits online and is used to payout to withdrawal requests.
 - `customerID`: Each user has a customerID, used to correlate incoming deposits with a particular user's account on the exchange.

The two main integration points to AiBlocks for an exchange are:<br>
1) Listening for deposit transactions from the AiBlocks network<br>
2) Submitting withdrawal transactions to the AiBlocks network

## Setup

### Operational
* *(optional)* Set up [AiBlocks Core](https://www.aiblocks.io/developers/aiblocks-core/software/admin.html)
* *(optional)* Set up [Millennium](https://www.aiblocks.io/developers/millennium/reference/index.html)

It's recommended, though not strictly necessary, to run your own instances of AiBlocks Core and Millennium - [this doc](https://www.aiblocks.io/developers/aiblocks-core/software/admin.html#why-run-a-node) lists more benefits. If you choose not to, it's possible to use the AiBlocks.io public-facing Millennium servers. Our test and live networks are listed below: 

```
  test net: {hostname:'millennium-testnet.aiblocks.io', secure:true, port:443};
  live: {hostname:'millennium.aiblocks.io', secure:true, port:443};
```

### Issuing account
An issuing account is typically used to keep the bulk of customer funds secure. An issuing account is a AiBlocks account whose secret keys are not on any device that touches the Internet. Transactions are manually initiated by a human and signed locally on the offline machine—a local install of `js-aiblocks-sdk` creates a `tx_blob` containing the signed transaction. This `tx_blob` can be transported to a machine connected to the Internet via offline methods (e.g., USB or by hand). This design makes the issuing account secret key much harder to compromise.

### Base account
A base account contains a more limited amount of funds than an issuing account. A base account is a AiBlocks account used on a machine that is connected to the Internet. It handles the day-to-day sending and receiving of delos. The limited amount of funds in a base account restricts loss in the event of a security breach.

### Database
- Need to create a table for pending withdrawals, `AiBlocksTransactions`.
- Need to create a table to hold the latest cursor position of the deposit stream, `AiBlocksCursor`.
- Need to add a row to your users table that creates a unique `customerID` for each user.
- Need to populate the customerID row.

```
CREATE TABLE AiBlocksTransactions (UserID INT, Destination varchar(56), DLOAmount INT, state varchar(8));
CREATE TABLE AiBlocksCursor (id INT, cursor varchar(50)); // id - AUTO_INCREMENT field
```

Possible values for `AiBlocksTransactions.state` are "pending", "done", "error".


### Code

Use this code framework to integrate AiBlocks into your exchange. For this guide, we use placeholder functions for reading/writing to the exchange database. Each database library connects differently, so we abstract away those details. The following sections describe each step:


```js
// Config your server
var config = {};
config.baseAccount = "your base account address";
config.baseAccountSecret = "your base account secret key";

// You can use AiBlocks.io's instance of Millennium or your own
config.millennium = 'https://millennium-testnet.aiblocks.io';

// Include the JS AiBlocks SDK
// It provides a client-side interface to Millennium
var AiBlocksSdk = require('aiblocks-sdk');
// uncomment for live network:
// AiBlocksSdk.Network.usePublicNetwork();

// Initialize the AiBlocks SDK with the Millennium instance
// You want to connect to
var server = new AiBlocksSdk.Server(config.millennium);

// Get the latest cursor position
var lastToken = latestFromDB("AiBlocksCursor");

// Listen for payments from where you last stopped
// GET https://millennium-testnet.aiblocks.io/accounts/{config.baseAccount}/payments?cursor={last_token}
let callBuilder = server.payments().forAccount(config.baseAccount);

// If no cursor has been saved yet, don't add cursor parameter
if (lastToken) {
  callBuilder.cursor(lastToken);
}

callBuilder.stream({onmessage: handlePaymentResponse});

// Load the account sequence number from Millennium and return the account
// GET https://millennium-testnet.aiblocks.io/accounts/{config.baseAccount}
server.loadAccount(config.baseAccount)
  .then(function (account) {
    submitPendingTransactions(account);
  })
```

## Listening for deposits
When a user wants to deposit delos in your exchange, instruct them to send DLO to your base account address with the customerID in the memo field of the transaction.

You must listen for payments to the base account and credit any user that sends DLO there. Here's code that listens for these payments:

```js
// Start listening for payments from where you last stopped
var lastToken = latestFromDB("AiBlocksCursor");

// GET https://millennium-testnet.aiblocks.io/accounts/{config.baseAccount}/payments?cursor={last_token}
let callBuilder = server.payments().forAccount(config.baseAccount);

// If no cursor has been saved yet, don't add cursor parameter
if (lastToken) {
  callBuilder.cursor(lastToken);
}

callBuilder.stream({onmessage: handlePaymentResponse});
```


For every payment received by the base account, you must:<br>
 - check the memo field to determine which user sent the deposit.<br>
 - record the cursor in the `AiBlocksCursor` table so you can resume payment processing where you left off.<br>
 - credit the user's account in the DB with the number of DLO they sent to deposit.

So, you pass this function as the `onmessage` option when you stream payments:

```js
function handlePaymentResponse(record) {

  // GET https://millennium-testnet.aiblocks.io/transaction/{id of transaction this payment is part of}
  record.transaction()
    .then(function(txn) {
      var customer = txn.memo;

      // If this isn't a payment to the baseAccount, skip
      if (record.to != config.baseAccount) {
        return;
      }
      if (record.asset_type != 'native') {
         // If you are a DLO exchange and the customer sends
         // you a non-native asset, some options for handling it are
         // 1. Trade the asset to native and credit that amount
         // 2. Send it back to the customer  
      } else {
        // Credit the customer in the memo field
        if (checkExists(customer, "ExchangeUsers")) {
          // Update in an atomic transaction
          db.transaction(function() {
            // Store the amount the customer has paid you in your database
            store([record.amount, customer], "AiBlocksDeposits");
            // Store the cursor in your database
            store(record.paging_token, "AiBlocksCursor");
          });
        } else {
          // If customer cannot be found, you can raise an error,
          // add them to your customers list and credit them,
          // or do anything else appropriate to your needs
          console.log(customer);
        }
      }
    })
    .catch(function(err) {
      // Process error
    });
}
```


## Submitting withdrawals
When a user requests a delo withdrawal from your exchange, you must generate a AiBlocks transaction to send them DLO. See [building transactions](https://www.aiblocks.io/developers/js-aiblocks-base/reference/building-transactions.html) for more information.

The function `handleRequestWithdrawal` will queue up a transaction in the exchange's `AiBlocksTransactions` table whenever a withdrawal is requested.

```js
function handleRequestWithdrawal(userID,amountDelos,destinationAddress) {
  // Update in an atomic transaction
  db.transaction(function() {
    // Read the user's balance from the exchange's database
    var userBalance = getBalance('userID');

    // Check that user has the required delos
    if (amountDelos <= userBalance) {
      // Debit the user's internal delo balance by the amount of delos they are withdrawing
      store([userID, userBalance - amountDelos], "UserBalances");
      // Save the transaction information in the AiBlocksTransactions table
      store([userID, destinationAddress, amountDelos, "pending"], "AiBlocksTransactions");
    } else {
      // If the user doesn't have enough DLO, you can alert them
    }
  });
}
```

Then, you should run `submitPendingTransactions`, which will check `AiBlocksTransactions` for pending transactions and submit them.

```js
AiBlocksSdk.Network.useTestNetwork();
// This is the function that handles submitting a single transaction

function submitTransaction(exchangeAccount, destinationAddress, amountDelos) {
  // Update transaction state to sending so it won't be
  // resubmitted in case of the failure.
  updateRecord('sending', "AiBlocksTransactions");

  // Check to see if the destination address exists
  // GET https://millennium-testnet.aiblocks.io/accounts/{destinationAddress}
  server.loadAccount(destinationAddress)
    // If so, continue by submitting a transaction to the destination
    .then(function(account) {
      var transaction = new AiBlocksSdk.TransactionBuilder(exchangeAccount)
        .addOperation(AiBlocksSdk.Operation.payment({
          destination: destinationAddress,
          asset: AiBlocksSdk.Asset.native(),
          amount: amountDelos
        }))
        // Wait a maximum of three minutes for the transaction
        .setTimeout(180)
        // Sign the transaction
        .build();

      transaction.sign(AiBlocksSdk.Keypair.fromSecret(config.baseAccountSecret));

      // POST https://millennium-testnet.aiblocks.io/transactions
      return server.submitTransaction(transaction);
    })
    //But if the destination doesn't exist...
    .catch(AiBlocksSdk.NotFoundError, function(err) {
      // create the account and fund it
      var transaction = new AiBlocksSdk.TransactionBuilder(exchangeAccount)
        .addOperation(AiBlocksSdk.Operation.createAccount({
          destination: destinationAddress,
          // Creating an account requires funding it with DLO
          startingBalance: amountDelos
        }))
        // Wait a maximum of three minutes for the transaction
        .setTimeout(180)
        .build();

      transaction.sign(AiBlocksSdk.Keypair.fromSecret(config.baseAccountSecret));

      // POST https://millennium-testnet.aiblocks.io/transactions
      return server.submitTransaction(transaction);
    })
    // Submit the transaction created in either case
    .then(function(transactionResult) {
      updateRecord('done', "AiBlocksTransactions");
    })
    .catch(function(err) {
      // Catch errors, most likely with the network or your transaction
      updateRecord('error', "AiBlocksTransactions");
    });
}

// This function handles submitting all pending transactions, and calls the previous one
// This function should be run in the background continuously

function submitPendingTransactions(exchangeAccount) {
  // See what transactions in the db are still pending
  // Update in an atomic transaction
  db.transaction(function() {
    var pendingTransactions = querySQL("SELECT * FROM AiBlocksTransactions WHERE state =`pending`");

    while (pendingTransactions.length > 0) {
      var txn = pendingTransactions.pop();

      // This function is async so it won't block. For simplicity we're using
      // ES7 `await` keyword but you should create a "promise waterfall" so
      // `setTimeout` line below is executed after all transactions are submitted.
      // If you won't do it will be possible to send a transaction twice or more.
      await submitTransaction(exchangeAccount, tx.destinationAddress, tx.amountDelos);
    }

    // Wait 30 seconds and process next batch of transactions.
    setTimeout(function() {
      submitPendingTransactions(sourceAccount);
    }, 30*1000);
  });
}
```

## Going further...
### Federation
The federation protocol allows you to give your users easy addresses—e.g., bob*yourexchange.com—rather than cumbersome raw addresses such as: GCEZWKCA5VLDNRLN3RPRJMRZOX3Z6G5CHCGSNFHEYVXM3XOJMDS674JZ?19327

For more information, check out the [federation guide](./concepts/federation.md).

### Anchor
If you're an exchange, it's easy to become a AiBlocks anchor as well. Anchors are entities people trust to hold their deposits and issue credits into the AiBlocks network. As such, they act a bridge between existing currencies and the AiBlocks network.  Becoming a anchor could potentially expand your business.

To learn more about what it means to be an anchor, see the [anchor guide](./anchor/).

### Accepting Other Tokens 
If you'd like to accept other non-delo tokens follow these instructions. 

First, open a [trustline](https://www.aiblocks.io/developers/guides/concepts/assets.html#trustlines) with the issuing account of the token you'd like to list -- without this you cannot begin to accept the token. 

```js
var someAsset = new AiBlocksSdk.Asset('ASSET_CODE', issuingKeys.publicKey());

transaction.addOperation(AiBlocksSdk.Operation.changeTrust({
        asset: someAsset
}))
```
If the token issuer has `authorization_required` set to true, you will need to wait for the trustline to be authorized before you can begin accepting this token. Read more about [trustline authorization here](https://www.aiblocks.io/developers/guides/concepts/assets.html#controlling-asset-holders).

Then, make a few changes to the example code above:
* In the `handlePaymentResponse` function, we dealt with the case of incoming non-delo assets. Since we are now accepting other tokens, you will need to change this condition; if the user sends us DLO we will either:
	1. Trade delos for the desired token
	2. Send the delos back to the sender

*Note*: the user cannot send us tokens whose issuing account we have not explicitly opened a trustline with.

* In the `withdraw` function, when we add an operation to the transaction, we must specify the details of the token we are sending. For example: 
```js
var someAsset = new AiBlocksSdk.Asset('ASSET_CODE', issuingKeys.publicKey());

transaction.addOperation(AiBlocksSdk.Operation.payment({
        destination: receivingKeys.publicKey(),
        asset: someAsset,
        amount: '10'
      }))
```
* In the `withdraw` function your customer must have opened a trustline with the issuing account of the token they are withdrawing. So you must take the following into consideration:
	* Confirm the user receiving the token has a trustline
	* Parse the [Millennium error](https://www.aiblocks.io/developers/guides/concepts/list-of-operations.html#payment) that will occur after sending a token to an account without a trustline


For more information about tokens check out the [general asset guide](https://www.aiblocks.io/developers/guides/concepts/assets.html) and the [issuing asset guide](https://www.aiblocks.io/developers/guides/issuing-assets.html).
