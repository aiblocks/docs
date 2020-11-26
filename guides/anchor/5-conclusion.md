---
title: Next Steps
sequence:
  previous: 4-compliance-server.md
replacement: https://developers.aiblocks.io/docs/enabling-deposit-and-withdrawal/
---

## Deprecation Notice

This guide explains how to set up a AiBlocks anchor service using a legacy flow outlined in [SEP-0003](https://github.com/aiblocks/aiblocks-protocol/blob/master/ecosystem/sep-0003.md).  For most use cases, we actually recommend the workflow specified in [SEP-0024](https://github.com/aiblocks/aiblocks-protocol/blob/master/ecosystem/sep-0024.md).  We are working on new documentation that explains that workflow, and it will be ready soon.  In the meantime, check the [Basic Anchor Implementation section](https://github.com/aiblocks/aiblocks-protocol/blob/master/ecosystem/sep-0024.md#basic-anchor-implementation) included in SEP-0024.


Congratulations! If you’ve made it to this point, you should now have a working bridge server, federation server, and compliance server, with the ability to make and receive secure, sanctioned payments on the AiBlocks network.

## Testing
Sending and receiving require your servers to interact with someone else's. This can make it difficult to test. For this reason SDF has built a handy testing harness were you can test sending and receiving against our test anchor. It can simulate various failure scenarios so you can be sure all corner cases are handled. Check out the testing tool here [goaiblocks.io](http://goaiblocks.io).

## Moving to Production

As you prepare to move your services into production and support transactions on the public AiBlocks network, you should make sure to carefully check a few things:

- Update the Millennium URL and network passphrase in your bridge server and compliance server configuration files. The bridge server will need a millennium server on the public network (instead of the test network) to send transactions. Both the bridge and compliance servers also have a `network_passphrase` in their configuration. The public network uses a separate passphrase:

    ```toml
    network_passphrase = "Public Global AiBlocks Network ; September 2015"
    ````

- Ensure that your bridge server and the internal port of your compliance server are not publicly accessible. Both of these servers allow for privileged operations that could be very costly if someone can reach them when they should not have access.


## What Next?

While you’ve now learned to handle the core operations of an anchor, there are many more things anchors might want to learn about or should consider:

- [Operate your own node and millennium server on the AiBlocks network](https://aiblocks.io/developers/aiblocks-core/software/admin.html). Doing so makes you less reliant on other providers and makes the whole AiBlocks network stronger.
- Read our guide on [security](../security.md).
- Make offers to [buy and sell assets on the distributed exchange](../concepts/exchange.md).
- Use a [market making bot to automate liquidity provisioning for your asset](https://github.com/lightyeario/kelp).
- Explore [multisignature systems](../concepts/multi-sig.md) to make critical accounts more secure.
- Use [channels](../channels.md) to submit more transactions at a time.
- Talk with other AiBlocks developers in [AiBlocks’s Slack community](http://slack.aiblocks.io/)
- [Contribute](../contributing.md) your own fixes and improvements to AiBlocks software.

<nav class="sequence-navigation">
  <a rel="prev" href="4-compliance-server.md">Back: Compliance Server</a>
</nav>
