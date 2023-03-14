# Meeting Transcript - 20/02/23
Transcript Nodes:
- The transcript is AI generated, so may misinterpret the conversation in places. 
- It has been edited for readability, so filler words have been removed.

Speaker 0: My update: I didn't do much on channel jamming, but circuitbreaker is the main thing that I'm looking at. What's happening at the moment is that some of my colleagues are redesigning the UI. I'm not sure if I mentioned that already in a previous meeting, but they're redesigning the UI to make it a bit more user-friendly and also workable on mobile. So if people use Umbrel to configure circuitbreaker, they can also do that on a mobile phone. I did read the mailing list post about the discussion about the binary and the continuous metrics. My feeling, of course this is very subjective, is that there are so many parameters and things that you can do. To me, it feels like it should start out very, very simple, much simpler than the things that I currently discussed. Maybe just the very simplest thing that you can think of that is a step in this direction. But that's just a general comment there. 

Speaker 2: So what do you think is too complicated and should be simplified? 

Speaker 0: Well, there's a model with a few parameters and we're keeping statistics of things. I would think, okay, so if there's multiple metrics, for example, can't you start with one metric? Just do one thing and just see how that works out instead of starting with three things. I have to think about what's happening in L&D with pathfinding, for example. It's a complicated model and the model is made even more complicated. It's very difficult to validate whether it's correct. At some point, there's 20 different interesting, complicated models that people can come up with. It's very difficult to see which one is the best of those. This is my personal preference. Just to start very, very simple, much simpler than what's currently discussed. Maybe not even have two tiers, just consider all pairs equal and just do something with dynamic limits based on their behavior. I think I mentioned this in the last meeting as well, that I was looking for the simplest possible thing. 

Speaker 1: I think that doing something dynamic might be slightly more complicated than having two bins when it comes to calculations. But I do agree with the general notion. 

Speaker 2: I think both my proposal for computing the reputation and {Redacted}'s proposal are actually quite simple. I don't think there is much complicated. 

Speaker 0: To me, it doesn't look like it's the simplest possible thing. I'm reading now about the three parameters, like the S, the maximum time, and there's a revenue that you need to track. 

Speaker 2: Well, you need a way to compute the reputation. You need to have some parameters. 

Speaker 0: I know you need to have some parameters, but there's already three. If you would only, for example, look at revenue and issue or maybe only report revenue, just start with that. I'm missing a little bit like the step-by-step approach because I don't think it's that simple. I don't think it's so simple to really implement this to the point where a user can actually use it and understands what it is. 

Speaker 1: Do you have a suggestion for the simplest thing? 

Speaker 0: I think if you pick one of those three parameters, let's say the time that it takes for HTLC to resolve, just measure that and show that to the user as a first step. And then as a second step, do something to limit the user if that S is not within certain bounds. That is simpler than what is here. You don't need to have the endorsements necessarily, but you can gain some experience with this metric. What does it look like and what do people set it to? So it's not that the endpoint is something that I think doesn't work. In Lightning, we have had so many complicated proposals and they are in draft for a year. So I'm just cautious up front now to see, okay, what's the simplest thing that you can do? That already can make some difference, can make it slightly better, so that you can roll it out to users and they can play with it. But of course, it's all up to you. You are the ones implementing that. That's my whole feeling with reading those suggestions generally. 

Speaker 2: I think if we are talking about the spec, the change is very simple. It's just adding one field, one TLV to add HTLC. The formula, if you want a simple formula, you can have a simple formula. If someone else wants a complicated formula, they can have it. It doesn't need to be in the spec. 

Speaker 0: I do agree that the formula doesn't need to be in the spec. What you're saying about just one message, you need to get it into LND because otherwise the metric is not using it. How do you get it in LND? It takes months. It takes a long, long time to get that, even if it's a simple change, because maybe it's not so simple. I think {Redacted} already looked at that. There needs to be changes to the database to do that, to carry over that extra field properly. I don't want to distract you all too much from your plans, but just wanted to share this. 

Speaker 2: Yes, actually on my side, I've started writing a simulator of a network, creating a dummy network and just simulating some traffic and applying different strategies and see what is the proportion of traffic that's blocked. So I started doing that. I'll share the results when I have them. I'm planning to try the same approach with local reputation, but different ways to compute the local reputation. 

Speaker 0: You said you have something to share about that, like results or code? 

Speaker 2: No, not yet, but I will. Actually, maybe what would be interesting is if you have some ideas of attacks, maybe we can create a shared document and everyone can share their ideas for attacks, because we don't really know what we're trying to protect against. I've seen, for instance, today {Redacted} sent an email about some attacks, if two channels have different fees and how you can use that. But I didn't really understand it as it was explained in the email. Maybe we should make it more formal and define what are the attacks we want to protect against. I think the criterion for what is an attack is does it block honest traffic? If it doesn't block honest traffic, it's fine. 

Speaker 1: So I'm guessing you want to take into account different structures of networks. I think the most basic is the path of Alice, Bob, Carol and Dave. And Alice and Dave are accomplices, they're the attackers and they're trying to ruin the channel. That's the most basic topology. But then you probably want to add in some traffic, which is honest. Yes, actually I wanted to simulate the whole network. 

Speaker 2: So something that we did for a bit is we created a network. 

Speaker 1: So something that we did for a bit is you have the basic structure of Alice, Bob, Carol, Dave. And then you add one more node that represents all of the network. And the four of them are connected to this demi-node. And this demi-node starts to route through them randomly or not randomly, depending on which criteria you want. I think this is already an interesting enough topology and then you can play with the fees and see where things are going. If you have a document in mind or some code that you want to share and get comments, I would be very happy to have a look and maybe suggest some things. 

Speaker 2: Yeah, I'll have something later this week. Yeah, so that's a good idea. Actually, I haven't really looked at this repo, but we could use it to coordinate. 

Speaker 1: For now, it's just used for these meetings. So there isn't much to look at. 

Speaker 2: And actually, if you could share your simple proposal in a created document that describes it. So maybe you can compare it to the other approaches. 

Speaker 0: Oh, I don't really have a simple proposal ready made, but just reading the proposals there, it seems to me that this is not the simplest possible thing. I think with creativity, it should be possible to make it simpler, to strip it down, to just do very small steps at a time. So, yeah, I also don't know what the best way is to do it, but I can imagine that if you only do one metric, you can still add value. But if you want to build it all at once and say, like, this all belongs together, if you do only part of this, it doesn't work. Yeah, that's also possible. But I think it's just easy to underestimate how much work it is to get something like that actually being used, because that's the goal, right? Unless you want to do just academic things and write paper and write simulation. But in the end, you want users to use it, I think. Even if you would just make one step, the small step, knowing that it's not really solving the problem, just to get familiar with, like, okay, what does it mean to add this metric, to show the metric to the user, if you even want that. I guess if you don't show anything, it's not going to work anyway, because then the user has no idea what's going on. So you probably need to give some insights into what this artificial brain is doing. Yeah. 

Speaker 1: And then there's no repercussions to me, because nobody knows where it came from. And they just depend on all the labors. So this is something I wouldn't give up on for the minimal model, whatever it will be. 

Speaker 0: But part of that thing, like if you implement what you're just describing, you also need to track those metrics, right, to upgrade the reputation of the peer or to downgrade the reputation. So, right. 

Speaker 1: You need to do something. So because we're talking, I don't know what's the simplest thing. I think that giving up on the ability of your peer to tell you something about this HTLC is too much of a simplification. So I think there should be an ability for a peer to say this is a great HTLC and then you do whatever you do with it. 

Speaker 0: So you're thinking, like, if you think about simplification, you would rather simplify on the algorithm to track that reputation than on this signal itself. So you rather have like a very simple, let's say you send this endorsement, but then the rule is tenfold and you're out, something like that. That would be like a more logical first step than making complicated metrics without the endorsement. 

Speaker 1: Yeah, exactly. 

Speaker 0: Yeah. Okay. I get that. 

Speaker 1: Does anybody else want to share something? Do we have other ideas, like different ideas? or is reputation and approval, like, the same? 

Speaker 2: Reputation and upfront fees, that's it? Or do we have other proposals? Personally, I think reputation is probably enough. And we probably don't even need upfront fees. We can have upfront fees, maybe it brings some improvements. But does anyone think differently? or what do you think about that? 

Speaker 1: So I'm starting to think that we might not need upfront fees. I'm very cautious about this because I feel like anything with reputation has this tricking around it. So I'm trying to think about ways to convince that this is indeed the case. Maybe the simulations that you're talking about would show that it's waterproof and we're good. Yeah, but I'm not sure. And I think that upfront fee could be a good idea in general. But that's a completely different discussion that doesn't have to do with jamming. 

Speaker 2: And Just, I know that you have been worried about jamming for a long time. Do you feel like we're going to solve it? 

Speaker 0: I don't know. I really don't know. I'm still not convinced of any of the solutions really. So maybe it's just going to be a trusted network after all. I can't say that. I really don't know. It's very disappointing that we're not able to come up with a solution without downsides. That is also simple. I guess it's the DOS problem which is also very difficult to solve in other areas of the internet. Maybe just scale up capacity to deal with it in the end. Bigger channels, more channels so that it's just too expensive to actually bring down a node. Yeah, I'm a little bit pessimistic maybe. 

Speaker 1: Does anybody want to say something about new problems that we might be introducing with this whole reputation system? Let's say it solves jamming magically and completely, but then what is the price we're paying for this? 

Speaker 3: I think from my point of view, with the reputation we don't know. We don't have the data to look and see, okay, with this reputation for a new peer, it's easy to become a trusted peer or not a trusted peer. So this is something that I'm a little bit worried about, about this local reputation. Because I believe that the simulation needs to be done in the real network on a real node. So we have a couple of nodes because we are developing for sure we have some nodes. And running the simulation on these nodes can be helpful. So we report the data somewhere and we see, okay, in this node, this local reputation is this way and we see how we converge at some point. I always don't think that the simulation on a fake network is giving something back. Because I really don't know that this information that we are faking are really happening in the network. or in the network we see something different. But I don't know. 

Speaker 2: The problem is I'm not really sure how we can simulate it in the real network. I mean, I have access to Acinq node. But yeah, I'm planning to implement my algorithm and shadow deploy it. So just to get the metric but not actually block the HTLCs. And we can get metrics. But I would expect that right now the network is not congested. I would expect it will just tell me that nothing was blocked. And we don't gain anything by doing that. 

Speaker 3: Yeah, this is true. But also you are seeing the network in the real environment. So you are seeing the users of, I don't know, mobile wallets that try to probing the network and so on. So you have a real behavior on the network, in the normal network. I see your point that, okay, you cannot simulate an attack or whatever. But I think with the fake, with the real, if we simulate the data on a real network, we can also, after all, we can also simulate in a fake network with the metrics that we have from a real network. We have more data while we are faking an attack or something like that. I don't know if I was able to explain my view on this. But I think if we have some data that say, okay, we know that this is happening on the real network. We can have more data when we are trying to simulate more complex or corner case. 

Speaker 1: Okay. Just to the question in the chat from {Redacted}. In both, reputation was to address two kinds of jamming attacks. One trying to mimic an honest behavior and one not trying to mimic an honest behavior. It doesn't have to do with slots or liquidity. So I at least used to think that reputation wasn't good enough because people can mimic, an attacker can mimic an honest behavior and get away with it. But now I am not sure that this is the case. Maybe an attacker cannot mimic an honest behavior and be able to jam. Either if the jamming is slot based or liquidity based. 

Speaker 2: So I think the distinction before was not between slot jamming and liquidity jamming, but between slow jamming and fast jamming. And the upfront fees was proposed to solve the fast jamming. But my belief is that reputation can solve both. Okay. Okay. So... Yeah. I agree we should list attacks and have a good way to measure the effectiveness of the attacks. And that's what we should be working on now. So I guess that's the actions to do before the next meeting, is to try to get some numbers on the different approaches. Does anyone have something to add? 

Speaker 3: No. I think we can take our time back. 

Speaker 2: Yeah. Okay. So the conclusion is that if you have some idea of attack, open an issue on the GitHub repo. And I guess that's it. 

Speaker 0: See you next time. 

Speaker 2: See you next time. Bye. 
