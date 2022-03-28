---
title: "Planning a honeymoon as a routing problem"
layout: post
mathjax: true
---

My wife and I  spent most of our honeymoon visiting several shopping malls near our first residency. We lived in a gated community, but it is rather isolated and the surroundings looked like a sub urban area. The "nearest" favorite shopping mall even takes 30 minutes drive with a common traffic. Worst traffic can take one hour drive. The same applies for visiting the nearest decent hospital, market, train station, and other mandatory public facilities. 

It's a cool place to live for introverts though. Sadly, I'm not an introvert. I still like to visit public places and meet people.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/house.jpg" alt="house" width="800"/></p>
<h6 style="text-align:center">Source: LeeSecurity.com</h6>




(that is not my house)

It turns out that there are more interesting places that I've discovered some time after that. I haven't got much opportunity to revisit these places since the pandemic started. 

It would be cool if I could visit all of them at one go. It won't be realistic though, since actually I had more than one week honeymoon back then, and who wants to visit multiple malls in one day.

Suppose that here is the list of my favorite places. 

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/fav-places-list.png" alt="fav-places-list" width="400"/></p>
<h6 style="text-align:center">you don't need to know these places to continue reading the post.</h6>

The first one was my home. Since I would go home after visiting these places, it would also be considered as another place to be visited.

For some contexts, here's how those places are located in Google Maps.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/mymaps.png" alt="mymaps" width="400"/></p>
<h6 style="text-align:center">Taken from Google My Maps</h6>

The green icon was my home. While most of these places were flocked, my home was pretty isolated from them. None of these places is quite close to it, but once I have visited any of these places, I could go to any other just in minutes. These are what you could expect if you spend only around Rp1 mio monthly rent fee for a relatively new house in a gated community.


What I'd like to do is to visit all of these places from home and go back home afterwards. Not only visiting them. I also want to use the route with shortest total distance.

Here is one possible route to visit each of these places exactly once before going home.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/route-example.png" alt="route-example" width="400"/></p>
<h6 style="text-align:center">Taken from Google Maps</h6>

I put the locations in arbritrary order, so it may not be necessarily an optimal route. 

The goal here is clearly to find the said shortest route. It's a search problem. With a brute-force approach, I can just compute the total distance of each possible route, compare each other, then find the shortest one. But since there are eight locations, there will be 40,320 different possible routes to compute. Ain't nobody got time for that! 

So let's try another approach.

.
.
.

This honeymoon planning can be framed as a <a href="https://en.wikipedia.org/wiki/Travelling_salesman_problem">traveling salesman problem</a>. In a traveling salesman problem, you will act like someone who needs to visit several places exactly once and go back to where you started, and you want the shortest trip distance possible.

First, let me expand the list above with distances between each pair of locations, like this.

|    | from |   to |   distance_km |
|---:|:---------------------------------|-------------:|---------------:|
|  0 | Serpong Natura City |            The Breeze BSD City |             11.9 |
|  1 | Serpong Natura City |            QBIG BSD City |              14.5 |
|  2 | Serpong Natura City |            Aeon Mall BSD |              11.4 |
|  ... | ... |            ... |              ... |
|  79 | Branchsto BSD |            Branchsto BSD |              0.0 |
|  80 | Branchsto BSD |            Serpong Natura City |              15.3 |

With the distances list, I will generate a distance matrix, which is just a pivoted version of that list, so the distances can be easier to model.

|    | Serpong Natura City |   The Breeze BSD City |   QBIG BSD City |  ... |   Branchsto BSD   |
|---:|:---------------------------------|-------------:|---------------:|
|  Serpong Natura City | 0.0 | 11.9 | 14.5 | ... | 17.3|
|  The Breeze BSD City | 9.7 | 0.0| 3.6| ... | 6.6|
|  QBIG BSD City | 15.2 | 6.3 | 0.0 | ... | 4.3|
|  ... | ... |            ... |              ... |
|  Branchsto BSD | 15.3 | 8.4 | 3.6 | ... | 0.0 |

I'm using <a href="https://developers.google.com/optimization/routing/tsp">Google OR-Tools</a> to find the shortest route faster. Now I will solve this problem using a strategy called <i>path cheapest arc</i> to find the shortest route. What path cheapest arc does is start from a depot (or home in this case), and then simply select the closest location from the current location to extend the path iteratively.

So here is the solution, drawn in a map.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/solution.png" alt="solution" width="400"/></p>

With the details of the route:

> [A/J] Serpong Natura City -> (10.9 km) [B] TerasKota -> (11.3 km) [C] IKEA Alam Sutera -> (3.9 km) [D] Living World Alam Sutera -> (7.4 km) [E] Scientia Square Park -> (4.9 km) [F] Branchsto BSD -> (3.6 km) [G] QBIG BSD City -> (3.0 km) [H] AEON Mall BSD -> (4.4 km) [I] The Breeze BSD City -> (9.7 km) [A/J] Serpong Natura City 
Route distance: 59.1 km

Note that the strategy being used here is rather a <a href="https://en.wikipedia.org/wiki/Heuristic_(computer_science)">heuristic</a>. It will just act like a shortcut for approaching the best solution. So the solution we would find may not be the global optimum one (or the very best one). And indeed, it's not the best solution yet, since a better route do exists by solving the problem with a different strategy.

In that case, can I trust this particular solution? 

Let's just test whether the solver's route is subjectively realistic. Here's the best route that I could imagine, based on some that I usually use. 

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/personal-best.png" alt="personal-best" width="400"/></p>

> [A/J] Serpong Natura City --> (10.9 km) [B] TerasKota --> (2.8 km) [C] The Breeze BSD City --> (1.6 km) [D] AEON Mall BSD --> [E] QBIG BSD City (3 km) --> (4.3 km) [F] Branchsto BSD --> (5.6 km) [G] Scientia Square Park --> (8.8 km) [H] Living World Alam Sutera --> (4.5 km) [I] IKEA Alam Sutera --> (20.7 km) [A/J] Serpong Natura City
Route distance: 62.2 km. 

Based on the number itself, the solution produced by the solver surely wins. Not only shorter, the solution also makes sense in real life. There are some pairs of locations that are quite close to each other and they were included a one of the trips, like from going from IKEA to Living World for instance.

The solver has also successfully avoided a single, long trip that could be substituted by multiple, but shorter trips. As we can see in my personal best route, the last trip is 20km, while the longest single trip in the solutions is only around 11km. I guess the savings come from there. Although a 20km trip home from IKEA is not that stressful since the road is a fun one. Obviously, the solver doesn't consider "fun" factor as a calculated cost.

<p style="text-align:center"><img src="{{ site.baseurl }}/assets/images/2022-03-22-honeymoon-tsp/ikea-blueprint.jpg" alt="ikea-blueprint" width="800"/></p>
<h6 style="text-align:center">Source: ntwrk.com.au</h6>

Nevertheless, there are certainly some factors that can also be considered. I could consider the trip duration, since some trips can be faster even if they're the longer ones. We could limit ourselves to not visiting more than three malls, for example, so we would have more varied places to visit. Or even if I had some honeymoon budget or time constraint, I could incorporate these as well. A trip to IKEA is surely exciting, but nobody wants to impulsively buy a kitchen cabinet or a a book shelf while they can't find the exit.