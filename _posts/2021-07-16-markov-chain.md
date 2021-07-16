---
title: "[WIP] Do customers tend to give another good (or bad) review after giving one?"
layout: post
mathjax: true
---

In retail and e-commerce industries, customers can share their experience of making purchases in a certain platform. Their reviews usually can also be seen by other customers. Customers might not decide whether to buy a product before seeing other customers' review first. Thus, review can be considered as one way for customers to express their view as a platform's user to the other users.

A review does not only describe the product, the service, and the transaction experience, but also the behaviour of the customer him/herself. Getting know about customers would be a good step to know their painpoints and whether those affect their behaviour. 

Consider the most prominent aspect of a review: rating (e.g. rating in a 1-to-5 star range). In terms of writing reviews, there are some behaviours that can be empirically expected from customers. If a seller offers a certainly bad product or service, then someone buys his product and gives bad review, the upcoming reviews tend to be similar. This is what we believe, both from the user's and the business' point of view. 

But, if a buyer gets a bad experience purchasing from a certain seller, either it is about the product or the service, will the upcoming reviews from the same buyer but for other different sellers be similar? Suppose that someone has given 1 out of 5 star rating for a purchase. What is the probability that he gives another 1 star for his next purchase? Or what is the probability of him giving another star?

How does a previously given review affect another upcoming review, either from the same or different user?

Given a bunch of users' transaction historical data, we can extract review history from each customer. As a dummy example, we're using a <a href="https://www.kaggle.com/olistbr/brazilian-ecommerce">public dataset</a> which contains orders made at a Brazilian e-commerce. 

With few data manipulation steps, we can track the reviews has been given by a customer. Ratings are in 1-to-5 range.



We can also count how many each star a user has given.

From this summary, we can calculate a probability defined as 

$$ P(X_{t+1} = i_{t+1} | X_t = i_t) = \frac{P(X_{t+1} = i_{t+1} \cap X_t = i_t)}{P(X_t = i_t)} $$

This notation tells us the probability of someone giving $$ i_{t+1} $$-star rating given he/she gave $$ i_t $$-star rating in his/her previous purchase at a certain time $$ t $$. Here, $$ i_{t+1} $$ and $$ i_t $$ can be the same or different number. 

We would like to model this with a collection of stochastic processes called Markov chain. Markov chain holds a property defined as 

$$ P(X_{t+1} = i_{t+1} | X_t = i_t) = P(X_{t+1} = i_{t+1} | X_0 = i_0, X_1 = i_1, ..., X_t = i_t)  $$

What the line above says is "the upcoming review is only affected by the latest review". In real life, we may think that someone's reviewing behaviour need to be observed from his/her whole review history. But in Markov chain, we'll assume that only the last given review will affect the next upcoming review. So observing his/her whole review history and the latest review only will be considered the same.

Markov chain tells us the probability of transitioning from a state to the other state at a certain time. We consider "giving a $$ i_{t} $$-star rating for $$t^{th} $$ transaction" as our state here. So "giving a $$ i_{t+1} $$-star rating after giving a $$ i_{t} $$-star rating" is the transition we're talking about. 

Oh, just think stochastic process mentioned above as a family of random variables. One type of stochastic process we might more often see is time series.

Anyway, based on these definitions, we can produce a transition matrix like this.


From this matrix, we can see the probability of a someone transitioning from $$i^{th}$$ state to $$j^{th}$$ state, where $$i$$ is the row number and $$j$$ is the column number. Both $$i$$ and $$j$$ represents a star rating.


To summarize this stuff, we can visualize these probabilities with this directed graph.


Some interesting notes from this graph.
<ul>
    <li>Customers have at least 39.7% probability to give the same rating as their previous rating.</li>
    <li>Customers who gave 5-star rating have 81.7% to give another 5-star</li>
    <li>Customers who gave 1-star rating have 55.1% to give another 1-star</li>
    <li>Customers have about 25 - 30% chance to give 5-star rating given they gave any rating beside 5-star</li>
</ul>

And one more cool stuff. With the same transition matrix, we can do a matrix operation defined as


We can use this notation to see the probability of someone giving $$i$$-star rating at his/her $$t^{th}$$ transaction given he/she gave $$j$$-star at his first transaction. 

Let $$t$$ be 20, which is a quite large number of purchases made by an individual. After a lot of purchases, the probability of giving 5-star rating is higher, regardless what star he/she gave in his/her first purchase.

By seeing these, given that the other variables are staying put, we get an overview approximation on how our customers are on our platform. 

With these such simple assumptions and also data preparation, it turns out that Markov chain can be a quite powerful model for us to explain a certain phenomenon like this review behavour problem. 

There's a very cool lecture from MIT OpenCourseWare which talked about Stochastic Processes (skip to the xx:yy for the Markov chain explanation). You can also read this article from Brilliant to get a little bit of initial exposure on this model.