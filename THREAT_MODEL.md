# Channel Jamming Threat Model

This document gathers a consistent threat model for the channel jamming issue grieving the Lightning Network. It aims to
serve as as basis to evaluate the robustness of proposed jamming mitigations.

A channel jamming is a type of denial-of-service of the routing capabilities of a Lightning node. The attacker paralyzes
the channel by withholding the resolution of a forward HTLC by controlling both sender and receiver of the payment path.

There are two major variants of Lightning jammings:
- Slot jamming: per which an attacker locks forwarding capabilities of the target channel by exhausting the limit of in-flight payments.
- Amount jamming: per which an attacker locks a significant portion of the target channel capacity.

## Attacker profiles

"The script kiddie": The script kiddie has a single Lightning node and a liquidity budget in the order of $1000. The script
kiddie strategy is to provoke havoc on the network for fun and no profit.

"The Lightning routing hop/merchant competitor": The Lightning competitor is a small business entity with a few Lightning
nodes from which to launch jamming attacks. The liquidity budget is in the order of $10 000 - $100 000. The Lightning competitor
strategy is to paralyze the payment and routing capabilities of established hops to earn routing fees or capture a merchant
selling traffic by offering substitutable goods/services.

"The payment system competitor": The payment system competitor is an estabblished financial services entity or cryptocurrency
entity of which the economic sustainability is threathened by the Lightning Network. The payment system competitor is a
large business entity with Lightning nodes in the order of 10+ from which to launch jamming attacks. The liquidity budget
is in the order of $100000 - $1 000 000. The payment system competitor is to paralyze the network by targeting high-traffic
links bridging the segement of the Lightning Network.

"The nation-state hacking group": The nation-state hacking group is an entity with hundreds of Lightning Network nodes,
capabilites to combine jamming attacks with low-level on-chain attacks (mempool network congestion, eclipse attacks),
liquidity budget is in the order of $1 000 000+. The nation-state hacking group strategy is to cut out permanently some
Lightning nodes payment capabilities, especially based on geographic or social attributes (e.g a Global South country
payment grid powered by Bitcoin as a legal tender).

## Attacker Targets

Stealing Routing Fees: Lightning routing hops earn off-chain fees to carry on HTLC forward along a payment path. In a
world where the user liquidity demand is nonelastic in function of routing hops liquidity offer, a Lightning routing
hop can be incentivized to steal routing fees by launching jamming attacks.

Capturing Goods/Services Traffic: Lightning merchant nodes earn income to sell liquidity services (e.g submarine
swaps) or goods (e.g gift cards) over the Lightning Network. In a world where the user goods/services demand is
nonelastic in function of the merchant offer, a Lightning merchant can be incentivized to catpure economic traffic
by launching jamming attacks.

Exhausting Client-Side Ressources: Delegated Lightning operations to support mobile clients like watchtowers are
likely to beconme an essential part of LN infrastructure. Watchtowers might require rate-limits or pre-payments
for each channel update. Frequently spamming some channels might exhaust their watchtower credits or bump mobile
clients Lightning usage cost.

Payment Send/Receive Capabilities: Lightning nodes can access payment capabilities over the network with a
single on-chain channel (or hosted ones). A Lightning node payment capabilities robustness scales in function
of the number of opened channels.

## User Profiles
 
Consumer User: This is a standard Lightning user on a mobile trying to use Lightning as a daily
payment system. They don't know how jamming mitigations work and they don't have time to configure them
correctly. They use the default and if Lightning does not work they will go to use Dogecoin.

Professional/Bitcoin hobbyist User: This is a user, either an advanced Bitcoin hobbyist or Lightning Service Provider,
who really wants to use Lightning for its economic advantages. They stay informed about jamming attacks, and 
they're ready to tweak the default values and deploy additional software modules to make their Lightning node
robust.

Large-scale Lightning Business: This is a Lightning entity maintaining significant Lightning services over
the network and operating some of the highest traffic Lightning channels. This entity has an infrastructure
team that can deploy its Lightning/Bitcoin nodes to mitigate against a variety of attacks and risks.
 
## References

- [HTLC Endorsement - Threat Model](https://gist.github.com/carlaKC/be820bb638624253f3ae7b39dbd0e343#threat-model) - Carla Kirk-Cohen
- [Unjamming Lightning: A Systematic Approach](https://eprint.iacr.org/2022/1454.pdf) - Clara Shikhelman and Sergei Tikhomirov
- [Spamming the Lightning Network](https://github.com/t-bast/lightning-docs/blob/master/spam-prevention.md) - Bastien Teinturier
- [Solving the Channel Jamming issue of the Lightning Network](https://jamming-dev.github.io/book/about.html) - Gleb Naumenko and Antoine Riard
