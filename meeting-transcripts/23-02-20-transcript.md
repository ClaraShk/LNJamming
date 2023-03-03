# Meeting Transcript - 20/02/23
Transcript Nodes:
- The transcript is AI generated, so may misinterpret the conversation in places. 
- It has been edited for readability, so filler words have been removed.

Speaker 1: So let's get started. The topic we had for today was the local reputation. And I'm wondering, do you all have time to read the write-up I sent? For now, we're focusing on a binary reputation between a node and its neighbors. The reputation is only used to decide whether a forwarded HTLC is accepted or rejected. In each channel we have quotas for HTLCs that we deem "not trusted" - we're neutral about them. Our suggestion would be 50% of liquidity and 50% of the slots. If you have available slots and available liquidity, anything that the neighbor sends to is forwarded. The remaining 50% of liquidity and slots, is saved only for neighbors that: you consider having a good reputation and they tells you that this HTLC came from somebody they consider to have good reputation, so you should forward it. This is the structure of where reputation comes into play. The question is, naturally, how does a neighbor have a high reputation? Here the aim is for a neighbor to pay you more in fees than the damage they can create. I'm just going to throw some numbers around. We're assuming that the maximum time something can hang in Bob's channels is two weeks. So Bob looks at how much he earned in fees in this two weeks? Say, half a Bitcoin from this moment in time looking back two weeks. At the moment, he needs to decide: does he trust Alice or not? So he looks at the past two weeks and says:  OK, I earned half a Bitcoin. Then he asks whether Alice in a longer period of time, for example in the past 20 weeks, gave him HTLCs that were the same half Bitcoin. It can be more or less, but this is the idea that we're proposing. If Alice in the past 20 weeks gave him fees that are the same as he earned in the past two weeks, then he says: OK, you have good reputation so you're allowed the extra liquidity and the extra slots if you need them. This is the sketch. the main idea is when everything is OK, you don't care what your reputation is. Usually not all of the slots and not all of the liquidity are used. If the network is under attack, then this comes into play. So any questions about this? 

Speaker 3, The scheme you're describing is just to gain reputations, to bootstrap your reputation in the network. Is that aiming to cover use case where you might lock up liquidity for two weeks, or I don't know, for doing some money swaps or that kind of things? 

Speaker 1: Let's say, if Bob and Alice agree that he allows some kind of different non-standard behavior, then he can change the way he grades your reputation. Nobody should be locking up liquidity for two weeks without discussing it first. 

Speaker 3: That's the main concern with this scheme that will work well for all standard types of packets, for doing things under two weeks. If you want to do loop out or peer swaps, DLC contracts on top of Lightning, where people might lock up liquidity over a payment path for three weeks, a month, something like this?

Speaker 0: That's already a problem, though. It's not Alice Bob channel you care about. It's how much, if we were Bob, right? So it's how much of their liquidity did Alice use? So if Alice uses her own channel, if you have a peer swap or something going on privately, that's fine. It's when you forward Alice's HTLC, and she uses that liquidity that it's an issue. There's some scope there to allow local lockup, because we're already talking about that for various offline payment scenarios anyway. There's a bit of nuance to how you measure. 

Speaker 3: The concern I have is: let's say you do some kind of multi-hop peer swaps for three weeks, and you cancel out the last minute after three weeks has passed. Now, rating fees are going to be paid for all the hops that are part of the payment path. 

Speaker 0: That's working as planned, I think? In the future, for the Lightning Network will work for long-haul payments you'll do a prepay. You're going to have to. You're going to have to send the successful payment on the side and say this is associated with a future payment, and have people reserve bandwidth and all those things. There has to be a whole other scheme for "I want to hold up your liquidity for three weeks". You're going to pay me. I think that's on the side. Any mitigation to jamming will mitigate jamming. So if you want to start jamming, you're going to have to pay for it - I think in advance. It'll invert the trust - so you'll have to trust me that I will accept your jamming. It's kind of orthogonal to how we detect the normal case, I think. 

Speaker 2: In today's network, a swap takes between three and six blocks to confirm, depending on the size. So it's not normal behavior to hold for two weeks. I think that would be different in a DLC world. Right now, these payments actually don't have very high hold times, except for like an exceptional 1% of swap cases. 

Speaker 1: Are those six, or even two, three blocks under what we're suggesting now considered bad behavior? This is probably not something you should do as a surprise to your neighbor and your neighbor's neighbor. 

Speaker 0: Yes, you just can't handle that kind of distribution because your normal cases is in seconds and now you're talking about an hour. We're probably looking at a factor of 1,000 or so difference. And it's hard to paper over that kind of thing without warning. I think in the long run, the network will head towards a state where you have to make some special arrangement for any kind of, you're waiting for blocks anyway. That's always been the case. 

Speaker 1: An interesting point that came up in conversation earlier today had to do with our proposal only looking at how quick a payment resolved and whether it succeeded or failed. The suggestion came up to that if Alice forwards payments that mostly fail, we also need to not like it and penalize her. I was wondering what you think about this?

Speaker 4: I agree with that, because from my data this happens a lot. There are a lot of channels that are forwarding very bad HTLCs. You can say: in this range of time this node gave me trash. Another thing that I was thinking is also if you are forwarding a lot of trash, you should start to pay for this trash. If in the last cycle, you forward 10 bad payments, right now your fee or upfront fee is more higher than before. I think this could be a good solution for that. I feel that is a little bit tricky, to measure this thing only on what is happening right now and not what is happening in the past. At least is what I am seeing with the data that I have. 

Speaker 1: Do you have some data to point to the distribution of failures that like? 

Speaker 4: I have a scoring of normal node. We are collecting node failure data. I have the API that you can query, but if you want I can do an excel sheet. I can zip the file that you need in a format that you want. 

Speaker 1: What would you consider when we're talking about fees? Bob is just looking the at amount of fees. What would be a normal behavior for Alice? So in time - say 10 seconds -everything needs to resolve? What would be like in success rates?

Speaker 4: I don't have this data because I am measuring every 30 minutes. I take this snapshot of the data and I have the scoring function. The 30 minutes is completely random. I would like to analyze this data and say: OK it's not 30 minutes, but 20 or 10 or two minutes. Right now the failure are very high. This depends also from the experimental feature that you have. For instance, in the last release of Core Lightning we broke the funding channel and now my local failure is very high. 

Speaker 4: I am not great to manage my channel, my node. I think this is a common problem with all the normal nodes that are not very good.

Speaker 0: There's a bootstrap problem as well. I'm not sure how much data you get over time. I am wondering: how a normal node, my node, which like (Redacted) is probably not beautifully run, I wonder how much data it would have for peers and whether we would just choose to grandfather in peers as having good reputation if they've been around for a while. If we're not gathering stats up to a certain point and we give everyone a good reputation to start with? Use time that they've been connected perhaps, or time they've had the channel established as a proxy just to bootstrap and then gather stats. But yeah, I'm not sure how many people would actually earn reputation. 

Speaker 1: I think it's okay for a lot of nodes to not have high reputation with each other. It usually should be like seamless for them. I think you can also start with just mistrusting everyone and then after a while building this up. 

Speaker 3: Even also function of how much demand you get for your slots and chain liquidity in a sense of, okay, if I have to have 100% of demand, so my reputation is going to be really costly to acquire.. On the other side, if I do have like 20% of HTLC traffic demand, it might be easier for my peers to build local reputations and that way I'll grant them more slots and it's like a lot more efficient. I do wonder if you should be more trusting if you're a quieter node. 

Speaker 0: If you've got 10 peers and they're all pretty quiet, do you just pick the most popular one and go: I'm going to trust you anyway? I wonder if your threshold should logically drop if you're going to trust someone, right? 

Speaker 1: In what we're suggesting right now, if all of your peers are pretty quiet, this means that your threshold for trust is also pretty low because you're checking how much fees you got lately. It's not like there's a lot for me to lose. If you paid well, even though you're pretty quiet, then sure, go ahead. So about the time as a measure of reputation, I'm almost slightly worried about this because failure or resolving quickly, this is giving somebody reputation for a repeated experiment with them while behaving, while them just being there. I don't know if this is a good basis for us, but, okay, we've been around for three years. So now we're good?

Speaker 0: I think just to bootstrap, you could probably use it as a proxy? I know I have never been jammed, so I'm going to assume everyone up to this point was good. But as you say, maybe you don't need to bootstrap. Maybe you're happy to have to see this in slowly. 

Speaker 1: If you're a huge node and connected to another huge node, and you're both from companies and things like that.

Speaker 0: I'm always assuming there's a knob you can turn to say: I trust this peer and I trust this peer and turn that on manually. That's a little bit out of scope. 

Speaker 1: Everything is going to happen like outside of the protocol. This is something that is very local and you don't tell anybody anything and you're just doing your thing. Sometimes you tell people I don't have liquidity and you don't even have to explain, but not at all or just, I don't trust you. 

Speaker 1: Any other parameters we would want to consider? 

Speaker 3: I gain reputation of one and then you start to endorse my transactions. But could you not endorse my transactions or my HTLC firewall request, even if I do have a reputation of one because of congestion? 

Speaker 1: If I trust you and you endorsed the transaction, I don't have a lot to gain by not endorsing this transaction going forward because I do want the HTLC to go through because there is a fee for me. If I do trust you, I don't see a reason for me not to endorse.

Speaker 1: I think that the endorsement is there for the neighbors of neighbors, not ruining things for you. If Alice received an HTLC from Zoe and it's forwarded to Bob, but she doesn't know Zoe, she doesn't want this HTLC to ruin her reputation. So she tells Bob: listen, here's an HTLC. It's totally random. You shouldn't think about this as something coming from me. 

Speaker 3: And reputation is binary in this scheme?

Speaker 3: Okay, if I start at zero, I gain one but I don't go immediately to zero, even if the damage I did inflict is less than what I offered to gain my reputation of one. 

Speaker 1: The damage in jamming is very total in its nature. If you jam something, you absolutely lock up the whole channel. You can lose your reputation by just not paying enough fees. This is actually a good point. We didn't elaborate on this on the write-up, we should. But once you go full attack, you're like: okay, I got all this things from you, they didn't pay any fee so your reputation goes down. 

Speaker 3: I'm not going to pay fees because the HTLC is not going through the full payment path or you have an honest case of not paying fees and you might still say: hey, that's okay, you can conserve your reputation of one. 

Speaker 1: That's something that can happen offline, right? Maybe my best friend from high school opens a channel with me, that can happen manually. 

Speaker 3: Sure, sure, just that binary petitions might not map well to the shades of payment flows.

Speaker 1: I like the binary thing because it's simple and I don't see a specific use case or situation that having more packets is significantly helpful with. So if there are these kinds of things, I think it can be and it's a very interesting thing, but no example comes to mind. 

Speaker 2: It's pretty bad for privacy to have continuous reputation. We end up with a step down towards the recipient. 

Speaker 3: Yeah, that's a good point. But you might still have like MPP where you can do four shards of MPP, only three of them gets to the final receivers. As a sender, you might be interested to not paying fees on the three shards, because your payment is not going to get through. But on the other side, it's like it's a downside for watching apps. So I don't know. 

Speaker 2: I think it could be useful. When we first started talking about this in the early calls, (Redacted) brought up the issue of being able to sabotage someone else's reputation through a cheap node. I went back and looked at the old mailing list post, and this is something that was discussed as well back then. If Alice has built up a good reputation with Bob, who is a very low revenue node, just a random Raspberry Pi, and he is connected to Carol, who's a very big node. To what extent can Alice spam Carol through Bob, having cheaply purchased reputation in this one locale, and then send it on to a higher fee reputation environment, and the degree of damage that can be done there? 

Speaker 1: I think in this specific example, if Carol is a big relative node, and Bob is a Raspberry Pi, then Bob will never have a high reputation with Carol, because she's probably gaining so much fees, that if he's hardly sending anything, he will never meet like her threshold for high reputation node. 

Speaker 2: Will that evenly apply though? Say we have Alice and then Bob's a Raspberry Pi, and Carol's a slightly nicer Raspberry Pi, and maybe a better, you know? I'm just not sure that it doesn't have this negative transfer property. 

Speaker 1: So if you have Alice and Carol are pretty much the same, then Alice can inflict damage to Carol through Bob, maybe using Bob's reputation. It can't go deeper, but it can stay in the "mountain range" - so you can like damage up until a certain height. You can start messing up, but you don't get to climb through very low nodes to very high nodes. 

Speaker 0: It's strictly better than the situation now, so it doesn't make that worse, right? 

Speaker 1: I don't know if that's the standard?

Speaker 0: It's a standard, right? 

Speaker 2: Yeah, it's a standard. 

Speaker 0: The perfect is the enemy of the good, right? 

Speaker 1: Yeah. Is there any other kind of thing we want to consider putting into the reputation? Or things that worry us with the scheme? 

Speaker 3: Well, continuous reputation would be like better for all type of management, but yeah, privacy downside. It depends how it's managed. If it's link level, if you have Alice and Bob, and Alice and Bob know their reputation score, and it's continuous, there is no privacy downside. If it does leak in any kind of gossip mechanisms, I can see the privacy concern. 

Speaker 1: I think for doing this not binary, we need like a strong use case. 

Speaker 0: It also makes it more finger-printable when it's not binary. At least there's some leakage here. If you have one trusted node, it's kind of like, oh, I know that probably came from the trusted neighbor of yours, but it's not terrible. I think if you've got one trusted you've got one trusted neighbor, almost by definition, most of your HTLCs are coming from that neighbor, right? They're the high traffic one. It is possible, of course, if you could serve privacy that one of your early payment attempts, you might deliberately not set your trusted bit. You can noise it down because normally you would set the trusted bit on your own payments. You might randomly flip that off in the early probe stage, as a quality of implementation issue. I think that if a new node appears and sets that bit saying, oh, this is high reputation, it's its own payment, that's potentially an issue. Unless the network's under attack, it doesn't matter. So maybe you would use a heuristic, if I don't have any high reputation neighbors, I'm probably not going to set that bit, even on my own payments, maybe, because I'm leaking too much information. 

Speaker 1: When you're a new node and you're sending a transaction, you don't have anything to gain by giving it high reputation because nobody trusts you. So you might as well not. 

Speaker 2: It'll be dropped by your first peer. You show up, you say, hey, trust me, you give it over, and they're like, no. 

Speaker 0: Sure. Nice payment you got there. 

Speaker 2: So in terms of the spec changes, what's quite nice about this is that it probably can live largely outside of the spec. If you're seeing a update_add_htlc field and the bucketing would probably be in the spec, but the actual reputation metric would live outside of the spec or with similar flavor as the routing recommendations? Do what you want. Does that sound sane? 

Speaker 0: Just tell people what to do and they will do it. I think, yes, in theory, like you go mad on whatever wild heuristic you want in practice. Please give us an example and we will copy it. You're thinking about this a lot harder than your average person just implementing it. We have a high level overview: OK, that seems good. The exact numbers, your guess is better than mine. So if you say the factor is 10, I'm like, great, the factor is 10. At least to start with. I'm tempted for you to just go: here's our recommended formula. Here's how you calculate all the things. We will literally implement that. Yes, but yes, you need an odd TLV number.

Speaker 2: I think it's a good case of you can just kind of opportunistically upgrade. It's unlike upfront fees where you need everyone together to set it and understand that it might get dropped along the way and wait for everyone to get there. I guess it wouldn't need a feature bit then, either. 

Speaker 3: No, you don't need a feature bit for this. 

Speaker 2: OK. 

Speaker 0: So in the past, we have so we've always gone on the fence with feature bits. You could make it a feature bit. It's nice to be able to look at the network and go, hey, look, everyone's doing this thing now. Although there are other ways of doing that. If you have to have a channel with someone to really find out. Feature bits are kind of nice for advertising purposes. As far as in the future, you can go: I'm going to require this feature, because I want you to know. So I would be tempted to assign a feature bit simply for that purpose, even though you can send it unsolicited. So which makes your code simpler. You just set it and you don't check whether our peer understands it. They will ignore it because it's odd. So you've got one less thing in your test matrix. You should advertise it simply for the state of like bragging rights. I would tend to say, yes, you should have a feature bit. 

Speaker 2: OK. Yeah. You do a release, you see the line. That's really nice. 

Speaker 0: Right. 

Speaker 2: Cool. So I think that in terms of what we'll do next for this is that I will bang together a spec proposal. Then I will ask (Redacted) to do all the smart things and give us the equations, because I can't do that part. I can only write the code. 

Speaker 3: We might have issues between high performance channels? Do you have issues between, like, performance and change aiming or the way you solve it? 

Speaker 2: I haven't really kept up with the mailing list, but is that going to happen? It seems no brainer to a lot of people. So I imagine if that does happen, it would be only very experimentally. 

Speaker 3: The conversations we had a few weeks ago with inbound fees. Do we have any issues with protocol level due to this? I'm not sure. 

Speaker 1: It feels almost orthogonal. This is it has to do with routing. It's global. 

Speaker 3: Sure. I do remember issues with the way you might solve this. 

Speaker 1: If you'll see the connection in a more specific way, please write and we can jump in. 

Speaker 2: I'd like to talk a bit about upfront fees if people have the extra 20 minutes just because we're here and (Redacted) bothered to wake up. 

Speaker 0: Sure. 

Speaker 2: Cool. So I've been trying to read through old mailing list posts and look at various things that we've thought about in the past so that we don't repeat ourselves. I have been looking into a proof of forwarding scheme, which is pretty simple. You put a secret in the next person's onion and they have to reveal it to you when they lock in the HTLC. Something that comes up, there's two things that come up with this kind of approach. The first one is that we need to telescope fees from the sender, which means that they accumulate along the way. And as we have it now, they don't depend on how far the HTLC gets. So if it goes two hops and there's a legitimate failure, the recipient, the person that legitimately fails, it gets the sum of the remainder of the route fees. Where that can be tricky is that it opens us up to attack where you try and get two nodes that are next to each other along a route. Then the second one always just drops payments. Then this one gets all of the upfront fees, which seems like a pretty serious issue. With a proof of forward scheme that works like that. I've been wondering about where we sit in terms of: we want to have upfront fees, do we want to go to something very simple and not even bother with proof forward? Or do we want to try and do something more complex and kind of go further down this proof of forward route that we've been looking at and address these issues? It seems kind of unclear to me and people sit in different camps about whether we need to solve this now or we need to solve this later. I think that Lightning has been in this loop for like five years now, which is tough. 

Speaker 4: I have a stupid question. I did not understand why we need the upfront fee. We cannot just simply change the semantics of the fee base or our base fee. We have our base fee that is for the pathfinding algorithm in theory always set to zero. And why we cannot use the fee base as upfront fee. 

Speaker 2: We still need some new type of mechanism to provide that fee before the HTLC is locked in. If we just use the base fee, we could say: oh, that's our upfront fee. That's fine, but the HTLC locked in. How do you make sure those funds are attributed along the route? They need to be pushed absolutely trustlessly or they need to be pushed conditional on some secret. That's what we've been looking at. The scheme we have currently does express upfront fees as a portion of our existing fees. We could use that in a way that you don't even have to advertise. We just assume it's 1% of what you've got. Other implementations do set the base fee and use it in pathfinding. So we'd probably just want to add new fields. 

Speaker 1: So I think maybe back to the original question, what's your take on doing something more intricate versus sort of jumping in? 

Speaker 4: I vote for the simple stuff, for a simple solution. But I am not sure. 

Speaker 0: OK, so the problem with telescoping fees is more that you get penalized so much for overpaying that you are really tempted to leak your path length. Say the fee is one sat per hop or something. It's really tempting only to sacrifice three sats so that you can get one sat back. Whereas the schemes where the token you get is provable along the path allow you to mitigate that somewhat. So the simple one being you have to hash. Everyone gets the hash of the preimage. So when Alice forwards to Bob, Bob comes back and goes, here's three preimages. Cool, you're entitled to three sats. That, of course, is trivially correlatable. Then again across the path, which is something we're trying to avoid. So you don't actually use hashing. You use points and you can fob them using some factor. If you use some proof that can be reflected along the path, then you can know and then you can overpay because you can go, cool, here's 100 sats I'm going to give to you upfront as the original originator, knowing that there's no hundredth preimage or hundredth point that you could produce. That would give you back. Then you can go for the scheme where either you just get it immediately or you get it, the next person has your preimage to give to you. That seems to work, although if they get the HTLC and you don't add a round trip, and they know they can't forward it, they could just screw you and not give you the contents that you need to get your fee, although there's no reason for them to do so. Just kind of being annoying. So they can look inside and go, well, actually, I can't afford this anyway, so I'm not going to make anything. So why would I give you your thing? And just, yeah, it was malformed and bounce it back that way. The other thing that this does give you, of course, is the proof of how far or of where the failure occurred, because you know that the only way they could have gotten the third preimage, of course, is if it was unwrapped three times. So you know who failed. So it does have some nice properties. I could not come up with a better accumulator than that whole unwrapping thing. Then you have corner case questions like, do you give the preimage to the person at the end so that they can take all of it and then you can fuzz the actual total amount or not? I think the answer is no. But if you've got PTLCs, then maybe the point itself serves as the final preimage. So if they give that, then they can also take all the upfront fees. I don't know. That was still my preferred scheme for doing upfront payments, because you can push your upfront fees through, which you're no longer incentivized so much to give a hint as to how far you're paying for when you push your upfront fees through. 

Speaker 2: And why would you say you don't want to give the final preimage to the destination and take that off your amount? 

Speaker 0: Okay, so you only want that on success. You don't really want to give them 100 sats for nothing, right? If you've got PTLCs, you can probably reuse that point would turn out to be the one that would give you. It would give them 100 sats. So they'll kind of get it as a payment anyway. So the two are tied. If you can't tie them atomically, because you're using HTLCs, then I don't think you want to. 

Speaker 2: Isn't the person you're paying pretty incentivized to do the right thing and settle that? 

Speaker 0: It depends on how you write the protocol. But yes, you could say you'd never get any of these push fees if it's on success. That's perfectly fair, right? You either take the big fees, or you take the little fees, and you only take the little fees if on failure. Yes, which is good because it avoids leakage, right? So the so this scheme doesn't leak how far you could potentially go, but it does leak how far you did go. If everyone's taking one sat, every hop single and sat and three, four at 100, you get back 97. You're like, Okay, well, I know it failed at three. And then you go, Okay, well, we can increase the granularity a bit. And maybe you get two images in your onion, so we can take it up a little and you overpay. I could not do it with arbitrary granularity. There's probably a really clever scheme. You could use handwave, handwave that allows you to provide how much I earned. And here's the proof that I'm owed this much. Some kind of signature, for example, some kind of partial signature that you would manipulate on the way through. So you know, get correlation. But signature against what I don't know. At that point, my brain exploded. So but but the simple kind of pre image idea seems to work. What granularity each pre image gives you? Handwave, handwave. 

Speaker 2: I don't get granularity meaning one preimage 50 sats or one. 

Speaker 0: Yeah, exactly. Maybe that's a parameter that you set. You say: this is how much you get for each pre image. It's definitionally constrained too. You could only have 1000 pre images because I've only sent you 1000 times that amount. So more than that doesn't get any more. 

Speaker 2: So you start to run out of space. But no - because you only need to put one pre image for each hop in the onion and then they need to send it back? 

Speaker 0: They may need to churn for a while to check. Right. So there's some upper limit. You can't go one Melissa and I'm going to pay 1000 sats this way, because they would have to a million, you know, a million iterations to validate that. 

Speaker 2: Then for I'm just thinking about cell phones, I guess that you'd want your LSP to be able to do this hashing for you in some world. The sender needs to do the original hash chain, right? 

Speaker 0: It's really cheap. I'm thinking 1000 is probably upper bound 1000 satoshis, you don't even notice like people do a lot more than that for the passwords. Even 1000 point, you know, point conversions for not using hashes. I think I still. I think I think this scheme works. Trying to get the N plus one behavior in this is the same problem that you have at the moment, which I think we can do, but we have to think about it really hard. This this was this was as far as I ever got down this road. And it seems to have nice properties. 

Speaker 2: And so when you say N plus one, you mean you can have attackers in a row or the final hop? 

Speaker 0: No. This is the: "I only get paid if you know I'm forwarding to you, you have the secret that will get me paid" issue. So therefore, I only get paid on forwarding success and which is that slightly more incentive compatible version. We want to do that. So there is a theoretical model there where I forward you enough that you can give me the secret, but I don't forward you enough that you can forward the payment. Then you give me the secret back and then I give you the thing: oh, and here's the information to forward. Oh, well, now I can't forward it. So, but that adds an extra round trip. And I definitely don't think it's worth it for this, particularly because with eltoo, we're hoping to eliminate a round trip. So I think it's okay that I send you the HTLC. And you may just fail it immediately and say, I'm sorry, there was no, there was no preimage in this or it will give me an evaluation, whatever, because you're not incentivized to do that all the time. 

Speaker 2: And it's fine if you like lock that HTLC into your commitment, even though you know, you're doomed to fail it because it doesn't have the right preimage?

Speaker 0: That's right.

Speaker 2: Well, I'm gonna get back to reading the mailing list because this is a bit before my time. I think I think that scheme was posted. 

Speaker 0: We just sort of include up front fee things if the full route flags the feature for now. 

Speaker 2: Then one day, hopefully we can turn it on as required. I still think there's a smart way to do it at this point. Everyone needs to know the new feature, and you could try and pair wise do it for half of the route, but it doesn't really seem worth the effort. The question is, what's the incentive for someone to actually turn it on? People will go without this for as long as they can. So you've got this transition, you've got two transitions: One where everyone allows it. And then another one, which people start insisting on it. I'm forseeing some issue opened on LND saying "implement dash dash no up front fee pathfinding". 

Speaker 2: Who knows, but that seems very far in the future. One option is to turn it on in case of attack, right? So nodes would go: cool, I'm actually stressed now, I'm going to start insisting or I'm going to start throwing things away. 

Speaker 0: And then it kind of happens organically, right? We could go for years without that happening. And then suddenly, oh, I'm not. And then they start complaining to wallet authors that none of their stuff works. It's like, yeah, well, it finally happened. And we never implemented that. Oops. 
