---
title: Create an Account
replacement: https://developers.aiblocks.io/docs/tutorials/create-account/
---

_Before we get started with working with AiBlocks in code, consider going through the following
examples using the [AiBlocks Laboratory](https://www.aiblocks.io/laboratory/). The lab allows you
create accounts, fund accounts on the AiBlocks test network, build transactions, run any operation,
and inspect responses from Millennium via the Endpoint Explorer._

The first thing you’ll need to do anything on the AiBlocks network is an account. Accounts hold all
your money inside AiBlocks and allow you to send and receive payments—in fact, pretty much
everything in AiBlocks is in some way tied to an account.

Every AiBlocks account has a **public key** and a **secret seed**. AiBlocks uses public key
cryptography to ensure that every transaction is secure. The public key is always safe to
share—other people need it to identify your account and verify that you authorized a transaction.
The seed, however, is private information that proves you own your account. You should never share
the seed with anyone. It’s kind of like the combination to a lock—anyone who knows the combination
can open the lock. In the same way, anyone who knows your account’s seed can control your account.

If you’re familiar with public key cryptography, you might be wondering how the seed differs from a
private key. The seed is actually the single secret piece of data that is used to generate both the
public and private key for your account. AiBlocks’s tools use the seed instead of the private key
for convenience: To have full access to an account, you only need to provide a seed instead of both
a public key and a private key.[^1]

Because the seed must be kept secret, the first step in creating an account is creating your own
seed and key—when you finally create the account, you’ll send only the public key to a AiBlocks
server. You can generate the seed and key with the following command:

<code-example name="Generating Keys">

```js
// create a completely new and unique pair of keys
// see more about KeyPair objects: https://aiblocks.github.io/js-aiblocks-sdk/Keypair.html
const pair = AiBlocksSdk.Keypair.random();

pair.secret();
// SAV76USXIJOBMEQXPANUOQM6F5LIOTLPDIDVRJBFFE2MDJXG24TAPUU7
pair.publicKey();
// GCFXHS4GXL6BVUCXBWXGTITROWLVYXQKQLF4YH5O5JT3YZXCYPAFBJZB
```

```java
// create a completely new and unique pair of keys.
// see more about KeyPair objects: https://aiblocks.github.io/java-aiblocks-sdk/org/aiblocks/sdk/KeyPair.html
import org.aiblocks.sdk.KeyPair;
KeyPair pair = KeyPair.random();

System.out.println(new String(pair.getSecretSeed()));
// SAV76USXIJOBMEQXPANUOQM6F5LIOTLPDIDVRJBFFE2MDJXG24TAPUU7
System.out.println(pair.getAccountId());
// GCFXHS4GXL6BVUCXBWXGTITROWLVYXQKQLF4YH5O5JT3YZXCYPAFBJZB
```

```go
package main

import (
	"log"

	"github.com/aiblocks/go/keypair"
)

func main() {
	pair, err := keypair.Random()
	if err != nil {
		log.Fatal(err)
	}

	log.Println(pair.Seed())
	// SAV76USXIJOBMEQXPANUOQM6F5LIOTLPDIDVRJBFFE2MDJXG24TAPUU7
	log.Println(pair.Address())
	// GCFXHS4GXL6BVUCXBWXGTITROWLVYXQKQLF4YH5O5JT3YZXCYPAFBJZB
}
```

```python
# aiblocks-sdk >= 2.0.0 required
# create a completely new and unique pair of keys
# see more about KeyPair objects: https://aiblocks-sdk.readthedocs.io/en/latest/api.html#keypair
from aiblocks_sdk.keypair import Keypair

pair = Keypair.random()
print(f"Secret: {pair.secret}")
# Secret: SCMDRX7A7OVRPAGXLUVRNIYTWBLCS54OV7UH2TF5URSG4B4JQMUADCYU
print(f"Public Key: {pair.public_key}")
# Public Key: GAG7SXULMNWCW6LX42JKZOZRA2JJXQT23LYY32OXA6XECUQG7RZTQJHO
```

</code-example>

Now that you have a seed and public key, you can create an account. In order to prevent people from
making a huge number of unnecessary accounts, each account must have a minimum balance of 1 delo
(delos are the built-in currency of the AiBlocks network).[^2] Since you don’t yet have any delos,
though, you can’t pay for an account. In the real world, you’ll usually pay an exchange that sells
delos in order to create a new account.[^3][^4] On AiBlocks’s test network, however, you can ask
Friendbot, our friendly robot with a very fat wallet, to create an account for you.

To create a test account, send Friendbot the public key you created. It’ll create and fund a new
account using that public key as the account ID.

<code-example name="Creating a test account">

```js
// The SDK does not have tools for creating test accounts, so you'll have to
// make your own HTTP request.

// if you're trying this on Node, install the `node-fetch` library and
// uncomment the next line:
// const fetch = require('node-fetch');

(async function main() {
  try {
    const response = await fetch(
      `https://friendbot.aiblocks.io?addr=${encodeURIComponent(pair.publicKey())}`
    );
    const responseJSON = await response.json();
    console.log("SUCCESS! You have a new account :)\n", responseJSON);
  } catch (e) {
    console.error("ERROR!", e);
  }
})()
```

```java
// The SDK does not have tools for creating test accounts, so you'll have to
// make your own HTTP request.
import java.net.*;
import java.io.*;
import java.util.*;

String friendbotUrl = String.format(
  "https://friendbot.aiblocks.io/?addr=%s",
  pair.getAccountId());
InputStream response = new URL(friendbotUrl).openStream();
String body = new Scanner(response, "UTF-8").useDelimiter("\\A").next();
System.out.println("SUCCESS! You have a new account :)\n" + body);
```

```go
package main

import (
	"net/http"
	"io/ioutil"
	"log"
	"fmt"
)

func main() {
	// pair is the pair that was generated from previous example, or create a pair based on
	// existing keys.
	address := pair.Address()
	resp, err := http.Get("https://friendbot.aiblocks.io/?addr=" + address)
	if err != nil {
		log.Fatal(err)
	}

	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(string(body))
}
```

```python
# The SDK does not have tools for creating test accounts, so you'll have to
# make your own HTTP request.

# if you're trying this on Python, install the `requests` library.
import requests

public_key = "GD4NB2FLQAN5JO7PKPGZJMNBDYQXVSNVC7DEIZMOL5WSNSBLEBUTEF5Q"
response = requests.get(f"https://friendbot.aiblocks.io?addr={public_key}")
if response.status_code == 200:
    print(f"SUCCESS! You have a new account :)\n{response.text}")
else:
    print(f"ERROR! Response: \n{response.text}")
```

</code-example>

Now for the last step: Getting the account’s details and checking its balance. Accounts can carry
multiple balances—one for each type of currency they hold.

<code-example name="Getting account details">

```js
const server = new AiBlocksSdk.Server("https://millennium-testnet.aiblocks.io");

// the JS SDK uses promises for most actions, such as retrieving an account
const account = await server.loadAccount(pair.publicKey());
console.log("Balances for account: " + pair.publicKey());
account.balances.forEach(function(balance) {
  console.log("Type:", balance.asset_type, ", Balance:", balance.balance);
});
```

```java
import org.aiblocks.sdk.Server;
import org.aiblocks.sdk.responses.AccountResponse;

Server server = new Server("https://millennium-testnet.aiblocks.io");
AccountResponse account = server.accounts().account(pair.getAccountId());
System.out.println("Balances for account " + pair.getAccountId());
for (AccountResponse.Balance balance : account.getBalances()) {
  System.out.println(String.format(
    "Type: %s, Code: %s, Balance: %s",
    balance.getAssetType(),
    balance.getAssetCode(),
    balance.getBalance()));
}
```

```go
package main

import (
	"fmt"
	"log"

	"github.com/aiblocks/go/clients/millennium"
)

func main() {
	account, err := millennium.DefaultTestNetClient.LoadAccount(pair.Address())
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("Balances for account:", pair.Address())

	for _, balance := range account.Balances {
		log.Println(balance)
	}
}
```

```python
from aiblocks_sdk.server import Server

server = Server("https://millennium-testnet.aiblocks.io")
public_key = "GD4NB2FLQAN5JO7PKPGZJMNBDYQXVSNVC7DEIZMOL5WSNSBLEBUTEF5Q"
account = server.accounts().account_id(public_key).call()
for balance in account['balances']:
    print(f"Type: {balance['asset_type']}, Balance: {balance['balance']}")
```

</code-example>

Now that you’ve got an account, you can [start sending and receiving payments](transactions.md).

<div class="sequence-navigation">
  <a class="button button--previous" href="index.md">Back: AiBlocks Network Overview</a>
  <a class="button button--next" href="transactions.md">Next: Send and Receive Money</a>
</div>

[^1]: A private key is still used to encrypt data and sign transactions. When you create a `KeyPair` object using a seed, the private key is immediately generated and stored internally.
[^2]: Other features of AiBlocks, like [trust lines](../concepts/assets.md#trustlines), require higher minimum balances. For more on minimum balances, see [fees](../concepts/fees.md#minimum-account-balance)
[^3]: CoinMarketCap maintains a list of exchanges that sell delos at http://coinmarketcap.com/currencies/aiblocks/#markets
[^4]: Once you have an account with a balance of delos, you can also create additional accounts
  via the [Create Account](../concepts/list-of-operations.md#create-account) operation, which
  transfers delos from an existing account to a new account. This is how new accounts are
  created on the public network, often where an exchange is the source account and your account is
  created by them. To do this yourself, follow the steps for [building a
  transaction](transactions.md#building-a-transaction) using a [Create Account
  operation](../concepts/list-of-operations.md#create-account) instead of a [Payment
  operation](../concepts/list-of-operations.md#payment).
