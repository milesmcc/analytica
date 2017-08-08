---
layout: post
date: 2017-3-14
title: "Predicting Market Crashes, Part 1: Theoretical Underpinnings"
date: 2017-3-15
title: "Predicting Market Crashes, Part One: Theoretical Underpinnings"
categories: market-crashes, finance
---

October 29, 1929 is called Black Tuesday for a reason.


On that day, the market lost $14 billion as a panic swept the market—stock traders reacted to their
portfolio’s price crashing by selling them as fast as they could, which only decreased prices
further. Some stocks could not be sold at any price: there was simply nobody foolish enough to buy
them. This precipitous collapse of the stock market led to the Great Depression and resulting
economic catastrophe across America and the world.


It is only natural to, in awe at the power and occasional abruptness of stock market crashes or
crashes of any other good, wonder: what causes stock market crashes? Why do systems that normally
seem stable suddenly veer off-course from their equilibria? This begs the question: can we use a
theory of how markets crash to predict them ahead of time or retrospectively analyze them?


Somewhat surprisingly, given how unpredictable and inscrutable markets are, there are substantive
testably-predictive models of markets that can allow us to attempt to predict crashes and study them
with impressive accuracy and precision.


The roots of this model lie in model developed by Johansen, Ledoit, and Sornette, in their
paper
["Crashes as Critical Points."](http://www.worldscientific.com/doi/abs/10.1142/S0219024900000115?journalCode=ijtaf) Their
model (which I will refer to as the JLS model for brevity) is remarkable for its interpretability:
instead of a data-first approach that attempts to synthesize a predictive model of market crashes by
analyzing prior ones, they first create a theoretical model of a market before a crash, and
mathematically work forward from the theory to create a practical, usable predictive model.


In this post, I will only explain the JLS model of the market. I will, in subsequent posts, expand
this theory into a practical model and, with a computer, develop a real-world test of whether a
market (given only the daily price) tends towards a possible crash or not.


I’ve been a bit loose with terminology, so let’s fix that. When I refer to a *crash*, I am referring
to what I should really call a *speculative crash*: a crash created by speculation. In a speculative
crash, the traded price of a good exceeds any substantive ‘concrete’ value it has: the crash simply
reflects the price dropping to closer the ‘true’ value (speaking loosely).


To start with, what does a normally-functioning market look like? Consider Anna, a trader in the
silver market. Anna, for whatever reason, thinks silver prices will rise, and so plans to buy silver
in the hopes that in the future she can sell it for a profit. There’s a very important assumption of
the JLS model hidden in this idea—that, fundamentally, the vast majority of traders do not have
access to different information than everyone else, and their decisions are mostly noise.


This might seem like an unstable system, but it actually gives the market its stability when it
functions normally. Here’s why—if for some reason more traders are buying than selling at any given
moment, the demand increase (coupled with a possible supply decrease) will raise prices. This, in
turn, will reduce the amount of buyers: anyone who thought silver was only a good deal by a small
amount will be priced out and stop buying. Similarly, if there is an imbalance in favor of selling,
the supply increase and demand decrease will drive the price down, decreasing the number of sellers
and increasing the number of buyers. This creates a stable equilibrium: if the price deviates in
either direction, it will go back to where it started instead of magnifying changes.


An important note is that for most real-world goods the price rises steadily because of global faith
in the economy. In terms of this model, there will always be a slight buyer-seller imbalance, but
buyers do have one extremely limited piece of information: on average, the price rises. Therefore,
the margin we consider as the point that prices buyers out may in fact be a point where they believe
they will make a profit by buying, but simply not *enough* of a profit to compete with other
investments.


This bare-bones model will never deviate from an orderly procession upwards, with random
fluctuations in between. To make this market crash, we need to introduce one more thing: local
networks.


Imagine Anna has two friends, Bennett and Chloe. They are also traders in the silver market, and
their decisions influence each other. To make this more mathematical, imagine each trader has a
*bias*, $$\sigma$$, that represents their random, noisy evaluation of the market: a positive value
means they believe in the market, and a negative value means they do not have faith in the
market. However, each trader also has a *network*, each with values $$V_0, V_1, V_2, …, V_n$$
indicating their current market position. Then we can imagine every discretized cycle of a market in
which everyone communicates with their peers updates each $$V$$ according to the following formula:


$$V' = \sigma + K (\sum_{i=0}^{n} V_i)$$


The value of $$K$$ determines how tightly-connected the market is. We’ll assume $$K$$ is non-negative:
that is, if Bennett thinks the silver market will fall, that opinion alone will not make Anna think
the silver market is more likely to rise. If $$K = 0$$, the market is just as before: complete
unconnected chaos, but around a stable equilibrium. (We could make multiple values of $$K$$, one for
each trader in the network, but that wouldn’t really add anything to the model as we will see.)


Things only get interesting when $$K > 0$$. Now, order and chaos fight: the chaos created by the
negative feedback loop of supply and demand against the order created by trader’s trust in their
peers.


We are used to thinking of order as a good thing, but in this case we want chaos to win: chaos is
what keeps the market stable. In the JLS model, a speculative crash is a symptom of too much order
in the market: everyone simultaneously buys and then simultaneously sells.


This is an ingenious solution to the quandary one quickly runs into otherwise: how can a
disconnected market of noise traders all decide to sell at once? The only other way this happens is
through new public information: a war breaks out, or a company releases a bad earnings report.


The ingenuity becomes apparent from the second major assumption of the JLS model: if we imagine a
global $$K$$ value for the entire market, $$K$$ rises as the price rises. To see the intuitive rationale
behind this, imagine again Anna, Bennett, and Chloe. If the silver price shoots up to the record
high, it piques all of their attention. Chloe might see that Bob thinks the price might fall soon,
and so might have some apprehension, but the promise of rising prices might entice her into
buying. However, that apprehension will create tension in the market: Chloe might constantly gauge
how Anna and Bob feel about the silver market due to her insecurity. This tension means that Chloe
is more likely, under the assumptions of our model, to listen to what Anna and Bob have to say: she
has more to lose if she is wrong, and more to gain if she is right.


Now we are equipped to tell the JLS story of a speculative crash. Imagine Anna, Bob, and Carol are
all buying silver like never before; the price climbs and climbs, so there’s a fortune to be made!
As prices rise, traders listen more to their peers, represented by $$K$$ increasing. This shifts the
balance of order and chaos in favor of order: despite prices skyrocketing, traders have faith in
their peers and networks that overrides their initial balking, and so demand continually outstrips
supply.


Eventually, the price shoots far higher than the ‘actual value’ of the traded good (defined here as
meaning that traders are speculating: buying so that they can sell later at a higher price instead
of believing in the rising true value of the good.) This ‘greater fool’ strategem (just hold on to
it for a week, and sell it to someone else who thinks the price will rise) eventually reaches a
tipping point where the market is coordinated and acting as close to a single unit, or even in total
unity as happened on Black Tuesday when there were literally no buyers in the market for some
stocks.


At this point, either new public information or general anxiety in the market triggers someone,
somewhere, to begin selling. (Alternatively, something restores stability to the market and it
levels out without a crash. Although this is a theoretical possibility, I will make the assumption
that predicting a crash is equivalent to predicting the pre-crash curve and ignore this
possibility.) This would normally be inconsequential, but because the market is so tightly connected
this causes a selling panic. Instead of buying to take advantage of the rapidly-lowering prices, as
the market would rely on for stability, the overwhelming urge to follow the consensus that the price
will collapse overrides that, crashing the price. At this point, the JLS model has nothing more to
say.


There is one final assumption of the JLS model: traders are rational, given their implicit noisy
bias. How can rational traders stay in the market even if they predict a crash coming (like if a
computer model told them)? The only logical conclusion is that the projected price gain if there is
no crash outweighs the risk of the crash and the projected loss from that. This provides us with the
ability to start mathematically analyzing this theory and get some equations out of it.


In the next post, I will develop this theory of how markets crash into actual equations that model
the price of a good before it crashes. In the subsequent posts after that, we will develop those
equations into a program that can give a projected likelihood of a crash. We will then test and
refine this computer model into a decently robust market crash prediction system.
