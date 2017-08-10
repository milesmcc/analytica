---
layout: post
date: 2017-3-15
title: "Predicting Stock Market Crashes, Part Two: Mathematical Underpinnings"
categories: market-crashes, finance
---

Hello! In Part One, we discussed the theoretical underpinnings of the
Johansen-Ledoit-Sornette (JLS) model, a market model that allows us to attempt
an otherwise-inscrutable phenomenon: sudden market crashes.


In this part, we will discuss turning this theory of the idealized behavior of
the market into a more concrete, practical model: an equation that models the
price of anything in the time preceding a crash or a return to stable market
behavior.


A quick note: I am trying to make these posts accessible to anyone with a broad
interest in economics or computing. This section, however, is intended to
provide a deeper mathematical understanding of the model we are going to explore
later using computers. Because of this, it will be dense and technical, moreso
than my other posts on this subject. The math underpinning the JLS model is
itself a recent invention for use in statistical physics, and is complex to say
the least: I would be lying if I said I understood it enough to explain it to
anyone else, much less someone interested in a topical overview. Feel free to
skip this section and keep reading from Part Three, which will be more familiar
to those with a programming background.


*Renormalization theory* is the mathematical framework that allows study of
self-similar systems like the one we will develop. It is an offshoot of
statistical physics (studying systems like particles interacting together), and
the fact that it pops up here (along with fields like mathematical ecology) is
quite profound and interesting. I am not a physicist or an economist (well, I’m
not an economist or computer scientist either, but don’t worry about
that!), so I will simply refer the interested reader to the papers the original
JLS paper cite and summarize their results, but I won’t discuss their actual
methods.


Now with that out of the way, let us proceed to a mathematical analysis of the
JLS model. A quick refresher from Part One: the JLS theoretical model of the
pre-crash market is a network of noise traders (traders without any special
knowledge or trading ability) that influence each other through local networks:
if we imagine a graph of all the traders, it is extremely sparse. A global value
$K$ describes the conformity of the network: $K = 0$ would be total chaos, and
$K = 1$ would be the entire market acting as one.


The genius of the JLS model is tying $K$ to the market price: it is reasonable
to believe that as the market gets more volatile, traders become more skittish
and quick to join the bandwagon if their network seems to think the price will
rise or fall heavily. This creates a positive feedback loop that eventually
raises $K$ so high that, once the general belief in the strength of the market
wanes, the result is a drastic plummeting of prices.


We start our analysis by postulating that every trader knows there is a nonzero
risk of the market crashing (we will, as elsewhere, treat crashes as binary
events without worrying about magnitude), that we will dub $h(t)$, or the
*hazard rate* as a function of time. 


Let $Q(t)$ be the probability distribution for the time of the crash. Then we
have $q(t) = \frac{dQ}{dt}$ as the probability density function of the crash and
the hazard rate becomes $$h(t) = \frac{q(t)}{1 - Q(t)}$$


This simply expresses that the hazard rate is the probability of a crash
happening in the next instant if it hasn't already happened. We'll treat a crash
as a single drop by a factor $\kappa$. Then, the price movement is given by
$$dp = \mu(t)p(t)dt - \kappa p(t) dj$$

where $j$ is 0 before the crash and 1 afterwards. $\mu$ is a function we are
adding to preserve the *martingale condition*: the expectation of the future
price, with all current information, is equal to the current price. This is
because when we say price in this context we are referring to speculation: i.e.,
we are assuming a good has no intrinsic benefit besides what we can sell it
for later. This can be easily adapted to cover over goods, by making $p(t)$ the
difference between the trading price and the actual value of the good.

This condition is equivalent to $E[dp] = 0$ (where $E$ is the expectation
given the information up to time $t$). The expectation of $j$ is given by $h(t)
dt$. Using this, we get
$$E[dp] = \mu(t)p(t)dt - \kappa p(t) h(t) dt = 0$$

This gives us $\mu(t) = \kappa h(t)$. Now we can plug back in and get the
differential equation
$$dp = \kappa \left( h(t) p(t) dt - p(t) dj \right)$$
