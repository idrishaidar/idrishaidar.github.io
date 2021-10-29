---
title: "How long should you wait until a customer's second purchase?"
layout: post
mathjax: true
---

Repeat customers can be considered as the key players of a settled and <a href="https://blog.smile.io/repeat-customers-profitable/">profitable</a> business. Repeat customers help us to get more purchases without needing to acquire new ones. Repeat customers are easier to gain a trust from. Repeat customers even help us to acquire more first time buyers. 

Acquiring new customers might cost higher than making the existing ones to purchase more. Even so, the latter seems to be more difficult. Business actors need to formulate when is the right time to appeal an existing customer to make his/her next purchase.

Given a customer has made a purchase once, for example, how long one should wait for him/her to make the second one. By estimating when the second purchase will be made, it might be easier to measure how challenging it is to expect more purchase from someone.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2021-10-23-survival-analysis/waits-faster-meme.jpg" alt="waits-faster-meme" width="350"/></p>
<h6 style="text-align:center">Source: starecat.com</h6>

Let's simulate this question with a *Survival Analysis*.

Survival analysis was usually done to estimate lifespans of individuals (e.g. in medical prognosis). We define an event as "death", which we want to anticipate the time of its happening at someone's life. We'd like to measure how long someone will "survive" since "birth" until meeting his/her "death". 

The measurement comes in the form of probabilities, so one of the questions we can ask is, "What is the probability of surviving until the time $$t$$?" (e.g. until the $$t^{th}$$ day after "birth").

One example: if we define "death" as "death by heart attack at any time $$t$$" and "birth" as "observation by the doctor is started", we can try to answer such questions like 
- "What is the probability of not having a heart attack until the $$t^{th}$$ day after the first observation day?", or 
- "What is the probability of having a heart attack at $$t^{th}$$ day after the first observation day?".

We actually can do survival analysis in many problems in general, as long as we adjust first how we define the "death", "birth", and "survive" event. We can also define how we will measure this "lifespan" by our own.

For our current context, we define
- "birth" as the event that a customer made his/her first purchase
- "death" as the event that a customer made his/her second purchase
- "lifespan" as the time difference between "birth" and "death" (or how long should we wait for a customer to make his/her second purchase).

## Lifetime Plot 

Let's observe the lifetimes of some customers.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2021-10-23-survival-analysis/lifespan.png" alt="lifespan" width="650"/></p>

The plot above simply shows the time difference between someone's first and second purchase. With the dotted line at $$t = 100$$, it means we're observing their "lifespan" until the 100th day after the first purchase. 

Red line means their second purchases are observed before or at the 100th day after the first purchase. Blue line means their second purchases haven't observed yet until the 100th day after the first purchase. Consequently, their second purchase could be happening at the 101st day or 105th day after the first purchase, or *not happening at all*. 

See also that some blue lines can be not too long, but some can be very long beyond the right border of the plot (the upper limit of the x-axis is adjusted on purpose to simplify the plot). We're calling these blue lifetimes as *right-censored*. 

Why the right-censored data are matter to notice? If we'd like to take the average "lifespan" of our customers when we're observing them at time $$t$$, the average can be underestimated since some customers' second purchase aren't observed yet (i.e. we don't know yet whether they will make second purchase beyond at time $$t$$). In other words, having observed their actual lifetimes beyond time $$t$$ can't be useful in this case.

In survival analysis, we want to get a better general estimation of waiting time between a customer's first and second purchase.

## Survival Function

First, let's define a curve called survival function $$S(t)$$ written down as 

$$ S(t) = Pr(T > t) $$

If you're already familiar with probability density function, notation above is similar with "the probability that an event is happened after time $$t$$". We can alter the phrase to "the probability to survive from an event until time $$t$$". The interval between the beginning of time ("birth") until a particular time the event is happening is our "lifespan" here, while the event can be the "death" we have defined before. So, we'd like to find what is the chance that our lifespan is beyond time $$t$$.

The survival function above is also a complement of "the probability that an event is happened before or at time $$t$$", written as $$1 - Pr(T \leq t)$$.

Regarding the right-censored data issue mentioned above, we will rather estimate the survival function with something called Kaplan-Meier estimator. 

$$S(t) = \prod_{i=0}^{t}1-Pr(T=i|T \geq i)$$

With notation above, we'd like to multiply the complement probabilities of death occuring at $$i$$ given surviving up to time $$i$$, for every $$i$$ from 0 to $$t$$. In our context, it's the same as multiplying the probabilities of not making the second purchase at time $$i$$ given the customer hasn't made the second purchase up to time $$i$$. 

$$1-Pr(T=i|T \geq i) = 1-\frac{d_i}{n_i}$$

$$d_i$$ is the number of customers who made their second purchase at $$i^{th}$$ day after their first purchase, and $$n_i$$ is the number of customers who haven't made their second purchase up to $$i^{th}$$ day after their first purchase.

Thus, what we need to calculate is

$$S(t)= \prod_{i=0}^{t}1-\frac{d_i}{n_i}$$

Now, it's just a matter of multiplying a bunch of fractions.

We will use Kaplan-Meier estimator to see how many percent of customers who still haven't made their second purchase until time $$t$$. With the help of <a href="https://lifelines.readthedocs.io/en/latest/Survival%20Analysis%20intro.html"><b>`lifelines`</b></a>, we just need the observed "lifespan" of each customers and whether second purchase was occured at the end of their "lifespan" to get our survival function. 

|    |   Lifespan |   Death |
|---:|-----------:|--------:|
|  0 |        326 |       0 |
|  1 |          9 |       1 |
|  2 |          2 |       1 |
|  3 |         84 |       1 |
|  4 |        253 |       1 |
|  5 |        215 |       0 |

Note that since the time when a customer made their first purchase is varied, the "lifespan" of customers who haven't made their second purchase until the end of observation can also be varied, although here we assume the end of our observation is fixed, which the last purchase date.

Here's the plot based on our estimated survival function.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2021-10-23-survival-analysis/survival-plot.png" alt="survival-plot" width="625"/></p>

The percentage on y-axis can be seen as the probability of someone not making his/her second purchase yet until time $$t$$. 

Some cool insights:
- Around 80% of customers haven't made their second purchase until the 22nd day after their purchase. 
- Around half of customers haven't made their second purchase until the 99th day after their purchase, which is around three months. 

Depending on what kind of business our data are based on, 22 days or 99 days can be relatively short or long (e.g. for corporate retail business, 22 days could be considered as short waiting time, but for consumer retail, it could be too long), they can be our estimation on when is the ideal time to re-engage the customers in order to increase their chance to purchase again.

## Hazard Function

We talked mostly about "survival rate", while our main focus is on the "death" event instead. Now, we'll shift our question to "what is the probability of someone making his/her second purchase at time $$t$$". We can model the probabilities with another function called hazard function. 

Hazard function $$\lambda(t)$$ represents the probabilty of death at certain time $$t$$ given surviving up to time $$t$$.

$$\lambda(t) = Pr(T=t|T \geq t)$$

If you're already have your survival function $$S(t)$$, you can get your $$\lambda(t)$$ by deriving $$S(t)$$, since $$S(t)$$ is just like the sum of some $$\lambda(t)$$

$$S(t) = exp(-\int_{0}^{t}\lambda(i)di)$$

$$\lambda(t) = -\frac{S'(t)}{S(t)}$$

But since our survival function was rather estimated with Kaplan-Meier estimator, the derivation doesn't work quite well. We instead will use another estimator. Initially, this estimator produces a cumulative hazard function $$\Lambda(t)$$, but soon we will be able to get our $$\lambda(t)$$. 

$$\Lambda(t) = \int_{0}^{t}\lambda(i)di$$

The notation above will be estimated into a simpler form by something called Nelson-Aelen estimator.

$$H(t) = \sum_{i=0}^{t}\frac{d_i}{n_i}$$

So here is our cumulative hazard function plot.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2021-10-23-survival-analysis/cumulative-hazard-function.png" alt="cumulative-hazard-function" width="625"/></p>

It seems the interpretation would not be too straightforward since now the y-label shows cumulative probabilities of second purchase at $$t$$ instead of just probabilities. But at least there are some takeaways. The cumulative hazard increase rate looks quite consistent along the timeline.

What if we just want to observe the probability? Well, we can now plot the hazard function.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2021-10-23-survival-analysis/hazard-function.png" alt="hazard-function" width="625"/></p>

Cosmetically, the line is more jagged than the previous one. But at least interpreting one number can be more obvious. 