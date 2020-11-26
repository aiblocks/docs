---
title: AiBlocks Network Overview
replacement: https://developers.aiblocks.io/docs/start/introduction/
---
![AiBlocks Ecosystem](assets/developers.png)

Using the AiBlocks network, you can build mobile wallets, banking tools, smart devices that pay for themselves, and just about anything else you can dream up involving payments! Even though AiBlocks is a complex distributed system, working with it doesn’t need to be complicated.

## API: Millennium

**Most applications interact with the AiBlocks network through [Millennium](https://www.aiblocks.io/developers/millennium/reference/),** a RESTful HTTP API server. Millennium gives you a straightforward way to submit transactions, check accounts, and subscribe to events. Because it’s just HTTP, you can communicate with Millennium using your web browser, simple command line tools like cURL, or the AiBlocks SDK for your favorite programming language.

The easiest way to install Millennium is by using the [**aiblocks/quickstart** docker image](https://hub.docker.com/r/aiblocks/quickstart/).

AiBlocks.io maintains [JavaScript](https://github.com/aiblocks/js-aiblocks-sdk), [Java](https://github.com/aiblocks/java-aiblocks-sdk), and [Go](https://github.com/aiblocks/go/tree/master/clients/millennium)-based SDKs for communicating with Millennium. There are also community-maintained SDKs for [Ruby](https://github.com/astroband/ruby-aiblocks-sdk), [Python](https://github.com/AiBlocksCN/py-aiblocks-base), and [C#](https://github.com/elucidsoft/dotnet-aiblocks-sdk).

## Network Backbone: AiBlocks Core

Behind the scenes, every Millennium server connects to **[AiBlocks Core](https://www.aiblocks.io/developers/aiblocks-core/software/admin.html), the backbone of the AiBlocks network.** The AiBlocks Core software does the hard work of validating and agreeing with other instances of Core on the status of every transaction through the [AiBlocks Consensus Protocol](../concepts/scp.md) (SCP). The AiBlocks network itself is a collection of connected AiBlocks Cores run by various individuals and entities around the world. Some instances have a Millennium server you can communicate with, while others exist only to add reliability to the overall network.

The easiest way to install AiBlocks Core is by using the [**aiblocks/quickstart** docker image](https://hub.docker.com/r/aiblocks/quickstart/).

You might want to host your own instance of AiBlocks Core in order to submit transactions without depending on a third party, have more control over who to trust, or simply to help make the AiBlocks network more reliable and robust for others.

## Big Picture: The AiBlocks Network

The AiBlocks network is a worldwide collection of AiBlocks Cores, each maintained by different people and organizations. The distributed nature of the network makes it reliable and safe.

All these AiBlocks Cores—the network of nodes—eventually agree on sets of transactions. Each transaction on the network costs a small fee: 100 stroops (0.00001 <abbr title="Delos">DLO</abbr>). This fee helps prevent bad actors from spamming the network. 

To help you test your tools and applications, AiBlocks.io operates a small test network and Millennium instance. [Get started with the testnet.](../concepts/test-net.md)
 
<div class="sequence-navigation">
  <a class="button button--next" href="create-account.html">Next: Create an Account</a>
</div>
