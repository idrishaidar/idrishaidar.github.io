---
title: "Mechanics behind hypothesis testing"
layout: post
mathjax: true
---

Most of us must have been familiar with this one of inferential statistics methods. Despite there are already tools that we can rely on to tell us which hypothesis is more likely based on a certain significance level and an alternative hypothesis setup (e.g. `scipy.stats` if you're a Python user), have you wondered how these things are working behind the curtain?

Knowing the mechanics behind a certain statistical method could help us build an intuition on how it is working (and not working) and what distinguishes it from simpler methods like point estimate and confidence interval. Plus, sometimes it's just cool to learn something that is overlooked by most people, right?

Let's get started.

.
.
.

Suppose that fresh graduates in US are reported to have \$37,888 student debt in average. We then take a random sample from 250 fresh graduates from certain states and we find that their average student debt are \$37,010. Suppose that the population of fresh graduates student debt in US is normally distributed. 

We'd like to test this claim by using 5% significance level.

There would be two possible conclusions regarding the truth of this claim:
1. The average student debt of US fresh graduates in certain states is \$37,888
2. The average student debt of US fresh graduates in certain states is not \$37,888 (<b>it could be less or more than that</b>).

The \$37,888 we're talking about here is the *hypothesized population parameter* which is population average ($$\mu_0$$). With the help of \$37,010 as our sample mean ($$\bar{x}$$), we'll make an inference whether the population parameter is more likely to be the hypothesized one.

In a math notation, we're more likely to represent the first possible explanation with equality sign $$=$$, while the second one with inequality sign $$\neq$$. The statement with an equality sign is oftenly put as the null hypothesis, so the other one will be put as the alternative hypothesis.

Then our null and alternative hypothesis could look like this.

$$ H_0: \mu = 37,888 $$

$$ H_1: \mu \neq 37,888 $$

Some other things we can know from the problem statement: 
- that the student debt population is assumed to be normally distributed;
- we gathered 250 fresh graduates as sample ($$n = 250$$)
- the test will be conducted with 5% level of significance ($$\alpha = 0.05$$)

We are facing a single population mean problem, without knowing the population standard deviation. We'll use Student's T-test as our testing method.

That's all we need to know for now. Let's break down the testing process.

I. 


