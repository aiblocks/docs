---
title: Testnet
replacement: https://developers.aiblocks.io/docs/glossary/testnet/
---

The testnet is a small test AiBlocks network, run by the AiBlocks Development
Foundation (SDF), which is open to developers.

SDF runs 3 AiBlocks Core validators on the testnet.

You can connect a node to the testnet by configuring [aiblocks-core](https://github.com/aiblocks/aiblocks-core) to use this
[configuration](https://github.com/aiblocks/aiblocks-core/blob/master/docs/aiblocks-core_testnet.cfg).

There is also a [Millennium instance](https://millennium-testnet.aiblocks.io/) that
can directly interact with the testnet.

## What is the AiBlocks testnet good for?

* [Creating test accounts](../get-started/create-account.md) (with funding thanks to Friendbot).
* Developing applications and exploring tutorials on AiBlocks without the
  potential to lose any valuable [assets](assets.md).
* Testing existing applications against new releases or release candidates of
  [AiBlocks Core](https://github.com/aiblocks/aiblocks-core/releases) and [Millennium](https://github.com/aiblocks/go/releases).
* Performing data analysis on a smaller, non-trivial data set compared to the public network.

## What is the AiBlocks testnet not good for?

* Load and stress testing.
  * If you want to test for performance, a good place to
    start is
    [AiBlocks Core's core performance document](https://github.com/aiblocks/aiblocks-core/blob/master/performance-eval/performance-eval.md#networks-to-test-against).
* High availability test infrastructure - SDF makes no guarantees about the
  availability of the testnet.
* Long term storage of data on the network - [the network is ephemeral, and resets periodically](test-net.md#periodic-reset-of-testnet-data).
* A testing infrastructure that requires more control over the test environment,
  such as:
  * The ability to control the data reset frequency.
  * The need to secure private or sensitive data (before launching on the public network)

Keep in mind that you can always run your own test network for use cases that
don't work well with SDF's testnet.

## Best Practices For Using Testnet

### Surge Pricing on the Testnet

The testnet has a capacity limit of **100 operations per ledger**.  When more than 100 operations are submitted to a given ledger, the network enters surge pricing mode, which uses market dynamics to decide which submissions are included.  It works exactly the same way as [surge pricing on the public network](https://github.com/aiblocks/docs/blob/master/guides/concepts/fees.md#surge-pricing).

If you are having trouble submitting transactions to the testnet, you may need to offer a higher fee.  You can also take the opportunity to develop a [fee strategy](https://github.com/aiblocks/docs/blob/master/guides/concepts/fees.md#fee-stats-and-fee-strategy), which may prove useful when you move your project into production.

### Periodic Reset of Testnet Data
In order to preserve a good experience for developers, the SDF testnet is
periodically reset to the genesis (initial) ledger. Resets declutter the network, remove
spam, minimize the time required to catch up to the latest ledger, and help
maintain the system over time.

A reset clears all ledger entries (such as accounts, trustlines, offers,
etc), transactions, and historical data from both AiBlocks Core and
Millennium, which is why developers should not rely on the persistence of any accounts or on the state of any balances when using testnet.

After a reset, you will need to take a few steps to re-join and re-synch to the testnet.  Those steps are outlined [here](https://github.com/aiblocks/packages/blob/master/docs/testnet-reset.md#testnet-reset), along with line-by-line instructions for people using core + millennium ubuntu packages.  If you need help with other packages, check [AiBlocks's Stack Exchange](https://aiblocks.stackexchange.com/) for guidance.

SDF will try to make testnet resets as painless as possible, and will announce the exact date at least two weeks in advance on the [AiBlocks Dashboard](http://dashboard.aiblocks.io/), and via several of AiBlocks’s online developer communities.

The testnet resets once per quarter (every three months).  The 2020 dates:

* 1/29/20
* 4/29/20
* 7/29/20
* 10/28/20

The testnet will always restart on the announced reset date at 0900 UTC.  

### Test Data Automation

Since most applications rely on data being present to do anything useful, it is
highly recommended that you have testing infrastructure that can repopulate
testnet with useful data after a reset. Not only will this make testing more
reliable, but it will also help you scale out your testing infrastructure to
a private test network if you choose to do so.

For example, you may want to:
* Generate issuers of assets for testing the development of a wallet.
* Generate orders on the order book (both current and historical) for testing
  the development of a trading client.

As a maintainer of an application, you will want to think about creating a data
set that is representative enough to test your primary use cases, and allow for
robust testing even when testnet is not available.

A script can automate this entire process by [creating an account with
Friendbot](../get-started/create-account.md), and submitting a set of
[transactions](transactions.md) that are predefined as a part of
your testing infrastructure.

For additional questions we recommend heading over to
[AiBlocks's Stack Exchange](https://aiblocks.stackexchange.com/).
