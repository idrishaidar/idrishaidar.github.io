---
title: "[WIP] Planning a honeymoon as a routing problem"
layout: post
mathjax: true
---

My wife and I  spent most of our honeymoon visiting several shopping malls near our first residency. We lived in a gated community, but it is rather isolated and the surroundings looked like a sub urban area. The "nearest" favorite shopping mall even takes 30 minutes drive with a common traffic. Worst traffic can take one hour drive. The same applies for visiting the nearest decent hospital, market, train station, and other mandatory public facilities. 

Those are what you could expect if you spend only around Rp1 mio monthly rent fee for a relatively new house in a gated community.

It's a cool place to live for introverts though. Unfortunately, I'm not an introvert. I still like to visit public places and meet people.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/house.jpg" alt="house" width="800"/></p>
<h6 style="text-align:center">Source: LeeSecurity.com</h6>




(that is not my house)

It turns out that there more interesting places that I've discovered some time after that. Since the pandemic, I haven't got much opportunity to revisit these places until now. 

I would be cool if I could visit all of them at one go. It won't be realistic though, since most of those places are shopping mall and who wants to visit multiple shopping malls in one day. But because I like visiting those malls because of their ambience, I'd consider those malls as any other scenic places.

Suppose that I already have a list of my favorite places. I also put my home to the list since I would go from there.

What I'd like to do is to visit all of those places from home and go back home afterwards. Not only visiting them, but I also want to use the route with shortest total distance.

Here are some possible routes.

The problem here is obviously finding the said shortest route. It's a search problem. With a brute-force approach, I can just calculate the total distance of each possible route, compare each other, then find the shortest one. But since there are eight locations, there will be 40,320 different possible routes.

Ain't nobody got time for that! So let's try another approach.

This honeymoon planning can be framed as a traveling salesman problem.

First, let me complete the places list with distances between each pair of locations, like this.

(distance.csv content)

All distances are in kilometers.

With the distances list, I will generate a distance matrix, which is just a pivoted version of that list, so the distances can be easier to model.

(distance matrix)

I'm using Google OR-Tools the find the shortest route faster. Now I will compare the two different solutions produced by two different kinds of strategy.

Note that these strategies are rather called heuristics. THey will just act like a shortcut for approaching the best solution. So the solution we'll find may not be the global optimum one (or the very best one).

So here they are, with around 60km total distance each.

(maps)

Can I trust this solution?

Well, there are some issues need to mention. Beside that they may not be the best one as mentioned above, let's just test whether these routes are subjectively realistic.

Here's the best route that I would imagine, based on my experiences when visiting those places. 

snc --> (10.9) teraskota --> (2.8) the breeze --> (1.6) aeon --> qbig (3) --> (4.3) branchsto --> (5.6) scientia --> (8.8) living world --> (4.5) ikea --> (20.7) snc

with total distance of 62.2 km. Based on the number itself, the solutions above surely win. But there are some trips that I personally think don't make sense if you should do those in real life. 

(living world -- ikea illustration)

There are some pairs of locations that are closer each other than the other pairs, but they're not included in any of those solutions and kind of replaced by another trips. I think the solver tried to avoid one extremely long trip like the last one in my personal best route. 

Serpong Natura City -> TerasKota -> The Breeze BSD City -> Living World Alam Sutera -> IKEA Alam Sutera -> QBIG BSD City -> Supermal Karawaci -> AEON Mall BSD -> Scientia Square Park -> Branchsto BSD -> The Flavor Bliss - Broadway Alam Sutera -> Serpong Natura City

