---
title: Transaction Fees, Minimum Balances, and Surge Pricing
replacement: https://developers.aiblocks.io/docs/glossary/fees/
---

To prevent ledger spam and maintain the efficiency of the network, AiBlocks requires small [transaction fees](#transaction-fees) and [minimum balances on accounts](#minimum-account-balance).  Transaction fees are also used to prioritize transactions when the network enters [surge pricing mode](#surge-pricing).

## Transaction Fees

AiBlocks [transactions](https://www.aiblocks.io/developers/guides/concepts/transactions.html) can contain anywhere from 1 to a defined limit of 100 [operations](https://www.aiblocks.io/developers/guides/concepts/operations.html).  The fee for a given transaction is equal to the number of operations the transaction contains multiplied by the [base fee](#base-fee) for a given ledger.  

```
Transaction fee = # of operations * base fee
```

AiBlocks deducts the entire fee from the transaction’s [source account](./transactions.md#source-account), regardless of which accounts are involved in each operation or who signed the transaction.

### Base Fee

The base fee for a given ledger is determined dynamically using a version of a [VCG auction](https://en.wikipedia.org/wiki/Vickrey%E2%80%93Clarke%E2%80%93Groves_auction).  When you submit a transaction to the network, you specify the *maximum base fee* you’re willing to pay per operation, but you’re actually charged the *lowest possible fee* based on network activity.   

When network activity is below capacity, you pay the network minimum, which is currently **100 stroops (0.00001 DLO)** per operation. 

### Surge Pricing

When the number of operations submitted to a ledger exceeds network capacity (**currently 1,000 ops/ledger**), the network enters surge pricing mode, which uses market dynamics to decide which submissions are included. Essentially, submissions that offer a higher fee per operation make it onto the ledger first.

If there’s a tie — in other words multiple transactions that offer the same base fee are competing for the same limited space in the ledger — the transactions are (pseudo-randomly) shuffled, and transactions at the top of the heap make the ledger.  The rest of the transactions, the ones that didn’t make the cut, are pushed on to the next ledger, or discarded if they’ve been waiting for too long.  If your transaction is discarded, Millennium will return a [timeout error](https://www.aiblocks.io/developers/millennium/reference/errors/timeout.html).  For more information, see [transaction life cycle](./transactions.md#life-cycle).  

The goal of the transaction pricing specification, which you can read in full [here](https://github.com/aiblocks/aiblocks-protocol/blob/master/core/cap-0005.md), is to maximize network throughput while minimizing transaction fees. 

### Fee Stats and Fee Strategy

The general rule of thumb: choose the highest fee you're willing to pay to ensure your transaction makes the ledger.  Wallet developers may want to offer users a chance to specify their own base fee, though it may make more sense to set a persistent global base fee multiple orders of magnitude above the market rate — 0.1 DLO, for instance — since the average user probably won't care if they’re paying 0.8 cents or 0.00008 cents.

If you keep getting a [timeout error](https://www.aiblocks.io/developers/millennium/reference/errors/timeout.html) when you submit a transaction, you may need to increase your base fee, or wait until network activity abates and re-submit your transaction.  To help inform that decision, you can consult the Millennium [`/fee_stats`](https://www.aiblocks.io/developers/millennium/reference/endpoints/fee-stats.html) endpoint, which provides detailed information about per-operation fee stats for the last five ledgers.  You can find the same information on the fee stats panel of the [dashboard](https://dashboard.aiblocks.io/).  All three of the SDF-maintained SDKs also allow you to poll the `/fee_stats` endpoint: [Go](https://godoc.org/github.com/aiblocks/go/clients/millenniumclient#Client.FeeStats), [Java](https://aiblocks.github.io/java-aiblocks-sdk/), [Javascript](https://aiblocks.github.io/js-aiblocks-sdk/Server.html#feeStats).   

### Fee Pool

The fee pool is the lot of delos collected from [transaction fees](./fees.md#transaction-fee).

Since the disabling of the inflation mechanism fees are retained in the fee pool and are no longer redistributed. 

The inflation mechanism was disabled (see [CAP-0026](https://github.com/aiblocks/aiblocks-protocol/blob/master/core/cap-0026.md)) in protocol version 12, which was approved by validators on 10/28/19. After this date no further inflation payouts were made.    

## Minimum Account Balance

All AiBlocks accounts must maintain a minimum balance of delos. The minimum balance is calculated using the **base reserve,** which is currently **0.5 DLO**:

```
Minimum Balance = (2 + # of entries) * base reserve
```

The absolute minimum balance for an account is 1 DLO, which is equal to `(2 + 0 entries) * 0.5 base reserve`. Each additional entry reserves an additional 0.5 DLO. Entries include:

- Trustlines
- Offers
- Signers
- Data entries

For example, an account with 1 trustline and 2 offers would have a minimum balance of `(2 + 3 entries) * 0.5 base reserve = 2.5 DLO`.

Any transaction that would reduce an account's balance to less than the minimum will be rejected with an `INSUFFICIENT_BALANCE` error.  Likewise, delo selling liabilities that would reduce an account's balance to less than the minimum plus delo selling liabilities will be rejected with an `INSUFFICIENT_BALANCE` error.

The minimum balance is held in reserve, and closing an entry frees up the associated base reserve.  For instance: if you zero-out a non-delo balance and close the associated trustline, the 0.5 DLO base reserve that secured that trustline is added to your available balance. 



## Changes to Transaction Fees and Minimum Balances

Ledger limits, the base reserve, and the minimum base fee can change, but should not do so more than once every several years. For the most part, you can think of them as fixed values. When they are changed, the change works by the same consensus process as any transaction. For details, see [versioning](https://www.aiblocks.io/developers/guides/concepts/versioning.html).

