# Meeting Transcript - 23/01/23
Transcript Nodes:
- The recording for this transcript was started ~5 minutes late, so it misses some introductory remarks.
- The transcript is AI generated, so may misinterpret the conversation in places. 

Speaker 1: Yeah, so I was thinking like with the same discussion comes back in like a lot of our areas and I think as long as it's still the case that LD is the majority of the network and if they have a critical work everything stops and then LALU fixes it and then everybody upgrades and then two days later nothing is happening anymore. It seems that we're still very very early and maybe we don't have to do as if we have like a huge legacy that we need to be super careful about. To me it seems like just introduce it in a disabled state and then sometime later make a judgment about how many nodes have upgraded and then just start using it. To me it feels that it's sufficient in the state that we are now. I know that other people have much different opinions about that and think that we should do everything to make this as simple as possible also from all the nodes. 

Speaker 0: No, I think that's fair for relaying nodes. For every node that runs on the server people tend to upgrade regularly because this is early and there are issues and there are good reasons where you really need to upgrade. but it may not be the case for wallets. I don't know about all wallet vendors but I think that most of them depend on quite old forks of some software so I'm not sure how often they would update. so I think it's okay for the relaying nodes but if the senders don't support it yet we'll have to just accept that senders will sometimes not pay the fee. 

Speaker 1: But they will not pay the fee or are they just going to avoid those nodes? because at some point like is there a way for those senders to force them to not use a node like advertise the required feature bits so that senders will skip that node so that they can still make payments but only through a subset of routing nodes? and that in itself creates an incentive to upgrade as a sender so you can use every node again and pay fees. 

Speaker 4: I think that this is what this is a different type of upgrade to something like inbound fees that might need a sender upgrade because this is a upgrade that senders probably don't want. so there is the kind of chicken egg problem of okay we'll get send it we'll get uh forwarding nodes to upgrade to say we support this and then wait until the majority of nodes on the network are sending signaling that they understand it and then we'll enforce it. but I can very quickly see like a wallet dev opening a feature on LND say have like a dash dash no upfront fee option when I make a payment and so I don't have to deal with this. and then what rational routing node will flip on a required feature bit if it's going to mean they lose traffic because people actively don't want to upgrade to this feature. 

Speaker 1: In case the senders don't want this because I know maybe it doesn't work out like. but at least in theory it could be the case that if you pay upfront fees you have much lower routing fees. but it's probably not going to work like that. but it could be that it's still. it's so favorable for the sender. 

Speaker 0: I mean that's the reason why they just want to avoid paying more fees and they don't want to think through something that complex. they just see fees and they just see that there's an additional fee that was added. so they just want to remove it and it's sad. but at some point that means yeah the relaying nodes have to start enforcing it but you know what has been said to be the first one to enforce it. so then we're kind of in a deadlock. but yeah. 

Speaker 1: I mean there is kind of an incentive to enforce it. as soon as the first nodes run into spamming problems and jamming problems then they will upgrade instantly. Just might take some years until we see that. 

Speaker 0: Yeah so when you start enforcing it you should actively jam all the other nodes to force them to start enforcing it as well. That could work. I know that Jost has some code to jam the network so maybe it's a good way to force people to upgrade. 

Speaker 2: There exists. Yeah. 

Speaker 4: Let me strike that from the transcript. Yeah but I think that the way that we have been thinking about this is pretty much that like. we'll try it and see. so like you put it as an optional feature bit to begin with and then if the sender finds a route and it happens to support all upfront fees or whatever the mechanism is you include. if it doesn't you just keep it simple and you don't include it. and I think an important part of any change we do would also need to loop in wallets and UX people and those sorts of folk. so there's kind of this like feeling of community consensus around a change and then maybe we can get away with deploy it and see. but I just wanted to get an idea of whether other people are comfortable with that kind of approach. 

Speaker 0: Yeah that sounds reasonable. On the Phoenix side we would definitely implement those and not provide the switch to get rid of them if we think it's useful. I think Breeze will do the same. I don't know about other wallets though. 

Speaker 1: I mean Core Lightning already prioritizes certain privacy features in our routing algorithm that aren't cost optimized so we already pay a slight premium. I don't think we've ever had too much intensive internal debate on that subject but so far the additional cost has been very minimal. 

Speaker 0: so I haven't 

Speaker 2: heard any users 

Speaker 1: complain about that specifically at 

Speaker 0: least 

Speaker 4: not very vocally. Cool that's all I had on this. So this is 

Speaker 1: also an idea that's probably going to work but I'm thinking about like negative upfront fees paired with a local reputation thing that prevents somebody from like continuously earning that that that upfront fee like similar to what we do with inbound fees make it negative to create a reason to to use them. Of course upfront fees is much more dangerous but I'm wondering if the danger can be countered with something like a firewall so that if anyone tries this they in the end can't do anything anymore. so they can a few times they can get that negative upfront fee and then they can't do anything anymore. 

Speaker 4: I mean how would you know who's abusing a negative upfront fee? because each payment will have a different hash or you just kind of watch for that type of activity? 

Speaker 1: It would be the same story as with lightning firewalls if you wouldn't have upfront fees. So I put on a firewall and if my peer is supplying that kind of traffic to me then the peer will be locked out and then they will be forced to also install the firewall to look at the app peers and then recursively the firewall installation spread throughout the network. I don't think it's a good idea just a random thought. 

Speaker 4: Yeah I'm not necessarily sold on negative fees. I know it has a bit of interaction with this type of topic but... I'm rather agnostic. but when yeah... 

Speaker 3: I mean do we have like where are the interactions of inbound fees with Jami? I mean because it's still unconditional. so it doesn't seem that much equation. 

Speaker 4: With inbound fees as they're currently proposed by both you and Matt is either iteration introducing the possibility of negative fees? I haven't kept up with the proposals. 

Speaker 0: Why would it interact with upfront fees at all since Jami just cancels those payments. so you can still pay inbound fees. but if you cancel the payment it doesn't matter that there are inbound fees or not right? What's the intersection between inbound fees and upfront fees? I don't see it. 

Speaker 4: I don't know how to mail in this post. I think it's if the fee is allowed to go negative but I guess if it's only a 6-day fee it doesn't matter. 

Speaker 2: Yeah I think like what René was talking about which there's like a whole family of attacks that if you can pull a lot of traffic towards you then you can do various shenanigans and one of them is to drain some upfront fees. So if everything has to go through you no matter where it's going then yeah. 

Speaker 0: Yeah you can prevent that. you use inbound fees, a negatively bound fee to create some spamming traffic that uses up some people that pays upfront fees. 

Speaker 2: But I think like in general like I think with what René said it's all like if you don't keep trying the same route with low fees and hope that it will work you shouldn't be paying too much. It's a bit of like for me once but for me thrice kind of structure. 

Speaker 4: I mean I guess another way of phrasing it is do we think it's a fair statement to say that senders will generally adjust their behavior around peers that are failing their payments a lot. I think that's definitely true of LND and is that true for all the implementations now? I think so. Will that always be true? I also think so but it is sort of an assumption that like yeah that is based on. 

Speaker 2: I just feel it's very natural assumption because even if you're not paying for it if you keep trying routes that fail then you'll keep failing and whatever it is you're trying to do is not working. I feel like it's a fair assumption about routing fees. I agree. So we have like the spec on the thing but you want to take this up? 

Speaker 4: Yeah sure. So I have been working on a spec change and a LND proof of concept for the absolute most basic version of our piece we could possibly have. Thanks for your standard to look at it. It's definitely not something that we would like to deploy today. It's pretty simple. I just want to get an idea of what the minimal change set is. But what Clara and I have realized as we work on it is that we probably don't want to implement the most basic version of upfront fees where you just kind of push the total amount along the route. And that because people have different fee rates and different amounts in the network we are probably going to need to look at something like Christian's proof of forward where you put a secret in the onion and it comes back to your peer. 

Speaker 2: Yes so like the heart of the issue that we previously didn't consider is that the fees can be. the difference can be in orders of magnitude and then it can be that the accumulated upfront fee that goes through you. Say you have a small you take a small fee and then the next person on the route takes a very high fee. then the amount of upfront fee you're getting is higher than your success case fee. Even if your upfront fee is only one percent of your success case fee because of this general difference in fees which which is like I don't know it could be like a thousand like 1k versus 400k or something like that. And then like the incentives don't work in a very blunt way you just earn more like your failure fee is better than your success fee. So we were using some ideas and I don't know if we zeroed in on something. I think the general concept is the prove that you forward it. 

Speaker 4: And that will look something like putting a secret in the onion for the next node. that needs to be revealed. But I think generally the problem is that if you accumulate fees along the route and you stop at maybe a cheap node and the next node is very expensive they're probably going to be incentivized to claim the upfront fee instead of their success case fee. We spoke a little bit about the possibility of banned payments each and along the route but that gets pretty messy pretty quickly. This one is a very smart way of doing that. We thought that would be far too many payments and pretty bad for privacy. But is that really happening that nodes will choose to keep the upfront fee? 

Speaker 1: I can't see. if this is the only thing you do in your life then you will keep it. but if you are examining your reputation by sending back those failures especially in the future where failures are penalized really hard maybe it's no node will want to do that even if the upfront fee is much higher than the success fee. Because they have huge costs to not deliver on the pre-image. The question if you're entering this very soft game of how many times can I steal the upfront fee before somebody notices. 

Speaker 2: Do I do it every... I don't know. I see what you're saying but I am worried about how... 

Speaker 3: I think there is a difference between your own risk policy in a sense of adding a high coverage of your success fee and market force in a sense of keeping upfront fees low because you're in competition on the network. 

Speaker 4: I think the example we landed on, I mean this is very related to my background, is just looking at the loop node specifically. That's a known big sink for payments and people have 400 set base fees, 500 set base fees and it seems like it'll always be worth it for cheap nodes leading up to that second to last hop to take it. The question of how much we can rely on senders to penalize those nodes makes it a bit more fuzzy. We haven't really landed on a solution there. I need to go back and look at Joost's fat errors thing and see how those two could combine because if you can attribute it maybe it's a bit better. That's kind of where we are. 

Speaker 0: Even with attribution the issue is that in a world where lately is actually used there are a lot of senders and the senders don't communicate about how else that happens. You're going to fool one sender by taking the fee, another sender will not know that you're playing the game. If there are many senders and you are collecting payments from many senders you can fail a lot of them and collect upfront fees and not actually be very negatively impacted on your routing route. I think this is an issue and I don't think attributing VLR is going to really help mitigate it entirely. But we could say we're not allowed to put upfront fees to more than this static value in the network or to something more than a certain percent of your relaying fees or something like that. 

Speaker 2: The example was if it's percentage the problem doesn't disappear and if you put a strong bound on let's say everybody has to use the same upfront fee then it's not the same number. It makes sense to because if you expect a very high fee revenue this is just not enough money to make you happy. You're not compensated enough with this upfront fee when you're under attack. I'm still very optimistic about the Lightning Network and failures. 

Speaker 1: I still hope that there's a future where as a note you just have no excuse to ever return a failure. If you return a failure you haven't done your work really. You're connecting with peers that are offline or you are not maintaining your balance or your peer is not maintaining their balance. so you shouldn't have appeared with that note in the first place. And once that's the case then senders can really like one failure and you're out. 

Speaker 3: I'm not optimistic like if you need the mempool to do like your liquidity rebalancing operations like you might not be able to keep your ht slots empty so we might have to keep up with a decent failure rate. Yeah I'm projecting this as the most extreme form I guess but I think it can be a lot stricter than it is today. 

Speaker 1: If you look at LMD's default settings with half-life time it's like it's like a lot of time. It's like a lot of time. It's like a lot of time. It's like a lot of time in these default settings with a half-life time of two hours or something. So failures forgotten after two hours. Like there's not really much. it's not really much of a penalty except for the payment that's being made at that point, so yeah. 

Speaker 0: Well and actually just about the comment I made earlier about senders not sharing information about which note failed the payment that's actually not true once we start having things in payments because trampoline payments will actually aggregate failures across many potential for many potential senders so that that could walk around this issue and help those failures be moistly attributable to faulty nodes and prevent them from being routed to. maybe that will work. 

Speaker 4: Guys, I still think that for a very basic upfront fee we really want your incentive to be afforded even though like you might get in trouble for dropping the payment for the fee. I'm just going to ask it because we couldn't come up with anything. but you know the issue here is that like say I'm a cheap peer and Clara is an expensive peer. I have an incentive and I come before her. I have an incentive to just drop the payment and collect her and my upfront fees because upfront fees does accumulate along the route like a normal HCLC does. Does anyone have any ideas about how you could uniquely deliver payments to each node along the route without spamming the network without like blowing up the number of payments that are needed to produce upfront fees for the entire route? because it seems like it doesn't have a solution but like on the off chance anyone else's different way of thinking to us I thought it's worth mentioning. 

Speaker 1: Yeah so as you said before there must be some kind of a locking mechanism that the fee is paid in parts and they are conditional on some kind of a secret that is obtained later. 

Speaker 4: Yeah so that's what we've been. we have a beautiful whiteboard here but we've been looking at like. each hop gets a secret and then you combine the secrets to claim different amounts. but it becomes becomes complicated when you have many. I mean it's nice now in Lightning there's like four hops in a payment. you know this node is x this node is xy this node is xyz. so we're kind of looking at that now. Where it does get tricky is how you communicate to the forwarding nodes how much they need to settle back in the upfront fee depending on how far the payment got and then what privacy starts to look like when you do that. 

Speaker 3: You might use a type of tree of transactions where the final type leaf is some kind of puzzle of your upfront fees. or I think it was Yoan who like was working on this trying to like have like a single output for all the hlcs. Might be a direction I don't know. 

Speaker 1: Yeah that was the idea. you have a single output for all hlcs and then you can battle a few of them and then you end up with a new output that contains the remaining ones. Yeah I'm not sure if you really need all this because you just said like yeah we look at the proposal and it's too simple like you need to add something to fix this. but maybe it's not necessary to fix it and perhaps initially people will just charge one set just to keep away the worst spammers and nobody gets close to their success fee also not cumulatively so then you can still learn a lot of things. you can spamming is no longer free and then do this like in a second iteration because it seems that upfront fees itself is already full of assumptions. and then another complicated thing on top of that. I'm not sure if it's going to happen then. 

Speaker 0: Yeah the thing is we want to solve spamming for a state of a network. that's not what we have today. Today we don't have a huge flow of payments so you don't really need to protect. you don't really need to collect a high fee to compensate because you don't have real payments coming in. that would have paid your real fees. but we want to find a solution that works when you have a steady state of real payments that would get you the fee and that is much more intense than what we have today. and when that happens just the one set upfront fee for example just wouldn't cut it. it just wouldn't compare well to the data to the flow that you would get for normal payments. but that's why it's hard because the network today is really far from that. but we have to project to a network that is really heavily used if we want to find a good solution spamming. So you think today once that fee would do nothing? No I think it would work today but I don't think it would work at all if tomorrow the network is really used. so and I think we I'm not sure it makes sense to really deploy this today if it doesn't solve at all the issue when the network has real volume. So I do want to say that like in an efficient market it would be very strong if we had to. 

Speaker 2: it would be very strange to see differences like I don't know three orders of magnitude difference in fees. 

Speaker 0: The way you would price your upfront fees would be based on your normal payment fees multiplied by the rate at which you expect to relay normal payments which is really true today. so today that would yield a very low upfront fees. but if you expect that flow to be really high then that means that your upfront fee is going to start to be somewhat meaningful. 

Speaker 2: Yeah but the question is like does it make sense in an again mature efficient market or for the fees of the payment to be like in one node it's one set and then down the route it's a thousand sets. 

Speaker 3: Yeah I think yes I mean market is not going to be perfect. 

Speaker 2: Yeah I know but there's like estimatories. Yeah the price differences just I just want sorry for breaking up. I just want to focus on the very very big difference you were saying. 

Speaker 3: But you might have a huge asymmetry because some people are going to be far better than others as you know like doing rocking stuff and like just a better liquidity, better model, better like provisioning, all that kind of things. 

Speaker 0: Yeah newcomers will have to accept that the newcomers will not be able to have an upfront fee that is as high as established nodes that people already use a lot. so there will have to be an imbalance at least for some time here. so I think yeah we will see a big difference in upfront fees between the small new nodes and heavily used established nodes. 

Speaker 3: But yeah I mean we do have like historical buckets on the decay side. so yes we stop to favor like old people, old channels. 

Speaker 1: Yeah I'm still not so sold on the idea that we need to fix a future problem like. I think just fixing today's problem is also worth something. of course you can argue that yeah maybe it's the wrong solution and then we need to revise again. but at least there's something concrete to solve and maybe in the end it's easier. If you just focus on today's problem let's say you just fixed one set fee always and that's it. I think even that you can learn something from it. so it's not time wasted. and probably the things that you learn from that they we can't foresee them yet currently. and who knows how long it takes before the network to gain to get to the sites where it doesn't work anymore. I think I'm more in favor if I would do this of an incremental approach and really start very simple because this whole proof of forwarding. Yeah I just looking at what the process looked like typically for getting protocol changes in like look at the trampolines and the onion routers and it's yeah it's just difficult right. so the simpler the better. If it does something good today I think it has value. 

Speaker 0: Yeah that's really the more complex solution was really fixing a lot of other important things as well. we'd go with the complex one. but if the complex one is we're not sure it's going to fix more stuff. the simple one is a reasonable first step I agree. 

Speaker 3: Yeah I mean we don't have that much part of like loop and that kind of things. there is not that much long term help payments on the network today. I mean it's just like simple payments that we care about. 

Speaker 4: Yeah I think the difference with loop loop is an example of a node that has very high second to last hop fees. so everyone who routes to the loop node has crazy high fees and then all the people before them have crazy low or default fees. so like that is like I am very forward doing something very simple because the minute you need like proof of forwarding it gets awful. but like right now people who forward to the peers of the loop gateway are probably better positioned just killing that payment. and I imagine that's like a very and I imagine the same would be true of another significant sink on the network like a popular merchant like that refill or someone I don't know what the fees that are saying no to like. but like you know that final hop tends to be very high and then everyone else tends to be pretty default. so the problem of I'm incentivized to drop this payment does exist today which is not. 

Speaker 0: Doesn't it just mean that the last hop should charge low upfront fees for that channel because they know that node before them would have incentive to otherwise just collect the upfront fee? 

Speaker 4: If we're like happy to tell people that I'm happy to tell people that. but I think the problem is like yeah you should have lower fees because maybe people will try and spindle them but then on the other side you're not being compensated for your loss of routing fees if your upfront fee isn't related to what your actual fee is like. I'm out here earning this much but now I need to in the failure case where I'm not earning this much be less because someone else might steal from me because the protocol isn't designed in the best way. 

Speaker 2: I do want to reiterate that is it is very very uncommon that you pay for the same product of different qualities a thousand times more. Like it's very atypical to the extent that it's like I don't know like there's apples for two dollars and there's apples for five dollars but there are no apples for two thousand dollars. Sure but I mean this is just like as a general kind of yeah but like locally. to resolve this. should we tell I don't know. 

Speaker 3: I mean your apple price in New York and your price I don't know in like China are going to be completely different and we might not like. 

Speaker 2: it's not the same product. 

Speaker 4: Yeah but I mean yeah it's true in all the default fee peers which I think is a good idea but that's not productive. 

Speaker 2: Big routing nodes like are there fees closer to the loop peers? What do you mean? What is the routing fees of big routing nodes? 

Speaker 0: Oh yeah we'll definitely put higher than the hubs to add the big nodes like loop and it really pays off. the channels are quickly depleted. so we'd want to yeah so we will always as you explained before have a much higher fee on that hub than the one before. so yeah I think that issue is real but. 

Speaker 1: So Carla do you already have an indication of Lightning Labs appetite for this? No reply. 

Speaker 2: There was some looks good need to look deeper and then a lot of silence. 

Speaker 1: Because maybe the problem again is like yeah there's not really a problem at the moment. so but what what? why would we do it? why would we prioritize like it's? yeah I'm a little bit worried about about that with channel jamming solutions in general. 

Speaker 2: Yeah that's. 

Speaker 1: So maybe we first have to create a problem again right that's my favorite thing so that people want to fix it. 

Speaker 0: Yeah so I tried. 

Speaker 2: it feels like a last. yeah I don't know if we're there yet but. 

Speaker 1: I try to create a problem by adding a queue mode. so hopefully a lot of people are going to use key mode and then as you start stacking up everywhere. 

Speaker 2: Just let me know if you need me to stop the recording at some point and then bring it back and we all pretend. No I'm just joking but yeah. 

Speaker 0: The issue is that we upfront fee solve quick jam but doesn't solve slow jam and I think slow jam is much easier to use to attack the network today. so we still don't have a good solution for slow jamming yet. so. 

Speaker 2: Yeah. so yeah like the upfront fee makes sense only as a part of a two-tier solution. so we can like we didn't do anything like spec wise or things like that when it comes to reputation 

Speaker 0: or 

Speaker 2: we can talk a bit more about this and circuit breaker and all like these kinds of things that should punish people that really do do something wrong and not trying to hide 

Speaker 0: it. Yeah. Yeah. 

Speaker 2: You still want to take this up. Circuit breaker. Yeah. 

Speaker 1: Yeah. So I'm not sure everybody knows that I added a user interface to that. Hopefully people are. It's easier for them to use it. And of course it's a super low tech solution very very conventional. But yeah I just hope that I will receive some feedback on it. These are some issues being open. so some people spent some brain cycles on the on the repo and tried to install it. And maybe I need to now make an effort to package it up with Raspi Blitz or Umbrell or just to make it even more accessible. So 

Speaker 0: I 

Speaker 1: yeah I'm not sure really if this is like how much of a solution it is but I do think that it's interesting because it provides at the very least insight to node operators about how many spend a experience because for L&D it's definitely not easy to get that information. It's really not available anywhere except on the HRC event interface and I checked all the GUI tools that exist currently and none of them is like fully exposing what's happening there. Some of them expose all the local failures so the ones that you do yourself but spam payments. typically you are not the one failing like it's for downstream where the failure is happening. So I just hope if people install this that they get to see like okay. so this is the magnitude of the spam problem that I have and perhaps also a little bit more insight in stuck HRCs although that is easier with existing tools to do. I think some of them they show the number of pending HRCs on the channel for example. so then you have that information and then yeah perhaps they start experimenting like there's one guy C.Otto, a customer, I don't know if you know him, he also installed circuit breaker and then he had a node I think LN router app or something. it's like a some probing service like a big spammer probably because it's probing all the time for money and yeah he could easily like just troll down that traffic either fail back or queue so make him feel the pain and yeah. so I will see like there's nothing concrete to update here. but I also think this is like a very slow process again. but hopefully in the coming months I will learn a little bit more from node operators what this does for them. 

Speaker 2: So I do wonder how like circuit breaker and the kind of reputation schemes we discussed a bit like we don't have anything concrete but like are there like the same pushing in the same direction other like we should choose one of them and go with it or do we need both? 

Speaker 1: I think the pushing in the same direction is just that. circuit break is like only manual whereas the thing that you are proposing is more automatic. where you look at like behaviors you have these endorsements and like. I see that as a more advanced version of circuit breaker. that doesn't require like setting those limits up manually. but to me I can definitely see like a two-step process there. once we know what the router node operators do with circuit breaker maybe even just visualize the endorsements. that could be another thing right. so you don't have any limits yet but you do implement the endorsement part of thing you visualize that so they get a feel for yeah what is really spam and what is shouldn't be spent but turned out to be spam and then it becomes also easier to get people on board to make an intervention there. 

Speaker 2: Cool. 

Speaker 3: I still think like we learn from any solution deployment in the sense of how node operators understand like jamming mitigations and like get numbers of what should be like you know on upfront fees or unconditional fees or like any kind of compensations in a monetary strategy and I'm still not convinced that circuit breaker would work well as a jamming mitigation or like as a pure in a pure monetary strategy. it sounds more like conditional control. 

Speaker 1: Yeah I'm not convinced either. but at least it's like if a node operator has like an immediate problem like they are being spent or jammed and they want to fix it. this seems to be the simplest way just lower a limit regardless of what the consequences are for the network. that actually solves the problem eventually. if everybody does this or not I have no idea really. 

Speaker 2: Like the main thing that worries me with this that your your peer can get you in trouble with another peer. right that's like spamming through you and then like you need to react quicker than like. let's say it's Alice, Bob and Charlie and Alice is trying to ruin Bob in the eyes of Charlie. then Bob needs to react quicker than Charlie. 

Speaker 1: So sort of yeah I don't know because everything is now manually. maybe it's not. I don't know. is this officially English? that soup is not eaten as hot as it is served? I don't know if that is officially. maybe that's the case because those peers know each other. so maybe they say hey I see this happening and no just check out your peers and otherwise I'm just going to hit you with a limit something like that. it seems currently like there's a lot of telegram activity between noting between peers anyway. so but yeah I understand what you say there. if everything is automatic it could be too late. like by the time you fix your problem you're banned by all your peers. yeah but maybe just the threat of that moves people to install it just because they know that if I wouldn't install it like I'm basically an uncontrolled note and it could cost me by my channels. 

Speaker 2: in general keeping track seems like a like really interesting. and 

Speaker 3: there is still like the issue when like you stop to rate emits someone which is bringing you like more interesting htlc like which is bringing you a lot of high paying htlc traffic and you start to rate him it because like a low level of it a low rate of this is just crap. 

Speaker 1: yeah yeah yeah I know it's definitely not perfect. 

Speaker 2: um is there anything else that we have in mind so I can give a quick update on like? 

Speaker 3: uh credentials and all that kind of things. uh like 50.2 yeah I mean like. since last time I just do like a bit throughout architecture and main thing is like trying to dissociate like um. you know like um the people paying the fees from the people like benefit getting benefits like the credential and like trying to say hey you might have a model where the upfront fees or the unconditional fees or the credential is paid by the lsp and the lsp. like just like you know like whitelist all like the traffic for the spoke and like getting like this attraction also on the other side where you might have you know you might have like one entity managing multiple sp or multiple routing ops and we do have this with phantom nodes on the uksi. so yeah um scheme about like if he's being paid by the lsp or like being paid by some party other than the original htls is under that's. 

Speaker 0: that's trivial to do if you use something like trampoline and if you don't i think it's uh it should also be easy to do. 

Speaker 3: yeah yeah it sounds trivial. 

Speaker 2: yeah okay so there's another github. yeah so i'm not familiar with the the other reputations but i think but it looks interesting. but uh would you like to elaborate on this a bit? 

Speaker 3: or um i can't break it right next time. but i'm also trying like to like mitigate against like some damn dots we have in like uh in english about mempool and that kind of thing. so it's not only trying to solve jem. okay 

Speaker 2: thanks i 

Speaker 0: mean what ollie 

Speaker 1: just posted just before. i'm not sure why he was here. 

Speaker 0: um uh this is we have a lsp spec group where we try to send resize like lsp apis and other things. and one goal of this group is kind of to create a marketplace. 

Speaker 1: i mean a decent or at least spec out a decentralized marketplace. 

Speaker 0: and for this we defined what are possible reputation criterias to evaluate what is a good note and what is a bad note. 

Speaker 1: and this is the first start of this. i mean it will continue. 

Speaker 0: it will also hopefully do something about discovery and all these things. 

Speaker 1: so apps like breeze or whoever wants to um connect to this decentralized marketplace have a way a centralized way to do it. maybe i'll adjust if you want to continue here. uh yeah that was a pretty good summary. but yeah we've been following this conversation briefly. uh it's maybe slightly different. what we're trying to achieve work is tackled from a different perspective. but uh i think deep down we are solving uh some of the same problems. 

Speaker 2: so we could make things to uh at least be aware of each other's progress and efforts. so maybe consolidate some of this together. 

Speaker 4: yeah so i hope this lsp spec is this um is this so that you can buy liquidity? because i think that because i think that you know forwarding reputation and and i guess they are the same but like scoring for purchasing liquidity often leans in the direction of starting to probe nodes. 

Speaker 1: i'm just wondering um yeah no yeah so if you look at the link there's like several layers three but i mean we're still very early. 

Speaker 4: okay yeah we'll do. look i haven't seen it before. just that the reputation we'd be looking at is from a local perspective and your experience of what the node is for you compared to the public high level kind of quote unquote global reputation of how long you've been around how legit you seem. um so they may touch a little bit but i think that slightly different flavors of the same thing. but i will read the issue because i haven't seen it before. 

Speaker 2: i think yeah so that's it. i just want to say like there is i think value to discussing what do we consider as good behavior until i think that we're yeah uh be it for jamming lsp or anything else. 

Speaker 1: yeah so as you mentioned when this is definitely something we're taking into account is what is the global data? but we're also interested in how to essentially create. consider local data as you name it or we split it essentially to three sections. one is subjective thing which is network data. essentially then you have relative attributes for example latency or how well do you perceive one node is managed? so you can only comment about your peers for example and then going even deeper to more subjective things. so like you bought liquidity but the peer didn't abide by terms of service that they advertised things like that. so i think at least one category overlaps kind of what you're considering here. but we're trying to think about it more globally because there's several different layers to what you're considering when you're trying to purchase liquidity. yeah i think that the sort of base global stuff that you all are looking at also can be useful as a starting point for local reputation. 

Speaker 4: so when someone just connects to you you know if they're a node with no channels. that has just just been announced 10 minutes ago. it's different to a node that has existed for two years and has bunch of channels. so we'll definitely take a look. yeah great and we have the bi-weekly calls. 

Speaker 1: so if anyone's interested please jump on and tell us how wrong we are. are you chatting over like? do you have like a telegram signal? 

Speaker 2: uh yeah let me just look at the chat and see if we can get a little bit of a background on that like. do you have like a telegram signal? uh yeah let me just link everything this is a good subgroup into us discussing like. do we want to go back and like to having these discussions every two weeks? do you want to get an email reminding you about this about the skulls? should the mailing list get any about the skulls? and and are you happy that we are talking an hour earlier? 

Speaker 0: yeah i am at least. yeah no i think i think we should continue and we should add more topics as uh as soon as uh things. uh we start to decide on some of the things but we should also include more people. we should try to get uh wallet developers joining. we should try to get other people from the implementations joining also and giving their point of view and then we should maybe start taking more pieces of the puzzle and for example discuss the reputation uh solutions as well. but i think it's worth uh keeping that effort going so that we slowly make progress and at least raise the level of awareness and where everyone's head at on these cues. 

Speaker 3: okay you want to do every two weeks or every month i don't know like i liked. 

Speaker 4: every two weeks um but what about every two weeks half an hour with the option to extend to an hour? 

Speaker 3: um yeah i found that really this can be good. 

Speaker 2: yeah okay so maybe we can do this depending on the number of topics i really like the fact that we're and i think we kept it up there's like a strong like whatever it is after an hour we're done. um but maybe we can try and keep it even shorter if there's less to talk about. like. for me personally i don't feel like a big difference between half an hour and an hour because i'm already kind of already sitting already like it's already in my schedule. but things like that. but how do you guys feel about this? 

Speaker 3: but you know people have like like implementation meetings like expectings uh irc car meetings and lots of things. 

Speaker 2: so you would prefer it. 

Speaker 3: do it for half an hour after an hour every two weeks sounds good but maybe like ask too many people like people who are not attending today. 

Speaker 0: yeah yeah same for me half an hour that can be extended to an hour if we have a if we're making progress on something sounds good for a bi-weekly meeting. cool. 

Speaker 2: and how do we feel about? like emails and like. well we have the good thing for the topics. 

Speaker 0: yeah i think it's worth spending a bit the mailing list to tell people that this is happening. 

Speaker 4: okay and we have to add upfront fees to the mailing list. that's a disaster. okay we'll we'll. i'll at least go through the recording and do a summary. maybe cool mailing list. yeah just to raise a bit of yeah razzle dazzle. 

Speaker 2: yeah okay thank you. 

Speaker 0: okay thank you that would be useful. i think that would yield more interest for people to participate in the second one. i think if people feel see that we discuss something for example wallet developers see that we started discussing topics that are that may be important to them that that will prompt them to join for the next meeting and hopefully get the discussion. keep the discussion going. 

Speaker 2: okay that's good. like i i wanted to keep spamming the mailing list. so anything short of everybody telling me to stop. i'm taking good as a green light yeah but just just to keep people thinking about maybe jumping in just so they know this keeps happening and they can join. 

Speaker 0: yeah i think 

Speaker 2: if we could relay 

Speaker 3: the problem 

Speaker 1: of the second to last hop and stealing the upfront fees like that's something 

Speaker 0: that 

Speaker 1: having a few more 

Speaker 0: eyes on 

Speaker 4: the topic to 

Speaker 0: brainstorm would probably be great. 

Speaker 2: on the mailing list yeah sounds great. okay reactions oh cool oh yeah thanks everyone. cool uh see you in two weeks then see you in two weeks. then thanks. 

Speaker 4: i got that uh link just typing in at false.dev in my work computer. i was like okay it. 
