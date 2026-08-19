---
title: "A Solution to the Secret Santa Problem"
date: 2026-08-17
---

My husband being one of eight children (half of which are married with kids) means that getting a gift for everyone in the family each Christmas isn't financially feasiable. To remedy this, my MIL began the tradiition of using a Secret Santa approach to gift giving: each member of the family is (randomly) assigned to give one other person a gift. This way, we know everyone is giving to one other person, and recieving from one other person.

Of course, each family members' spouse shouldn't be drawn as their giftee-- obviously, whether or not a husband draws their wife in the family Secret Santa, they are still going to get their wife a Christmas present lest they incur her well-deserved ire. Also, we want to make sure no one has drawn the same name two years in a row. These problems arose for my MIL in the free Secret Santa name drawing applications available online-- they don't allow for these types of exclusions.

So, -- in what could only be described as abject deperation, no doubt -- she sent out a request to the Python-inclined among the family asking for a Secret Santa algorithm and desktop application that could intake these exclusions as a CSV file and account for such exclusions when drawing names for the upcoming Christmas. 

Mortified, by the time I saw this email request one of my well-intentioned (if not misguided) brother-in-law's (who will remain nameless as to not be known as a Clanker-sympathizer) offered to create something via ChatGPT. I knew I needed to beat the Clanker to the creation of the Secret Santa script. And here is where my claim to fame comes in: I managed to make a script to handle this task before ChatGPT. 

## The Brute Force Approach: Guess-and-Check

The method wasn't elegant, but it did work. Basically, it loops through each family member (gifter) and assigns them a giftee, without replacement. Then, it checks each pairing to see if any of the exclusion constraints were violated. If there were no violations, it returned the pairings, if there were violations it looped through again and again until a random configurations doesn't violate the exclusion constraints.

That's it! Keepin' it stupid simple.


```python
    def secret_santa_generator(exclusions):
        names = [key for key, _ in exclusions.items()]
        n = len(names)
        while True:
            random_order = random.sample(range(0,n), n)
            pairings = {names[i]: names[random_order[i]] for i in range(0, n)}
            exclusions_check = True
            for giver, receiver in pairings.items():
                if giver == receiver or receiver in exclusions.get(giver):
                    exclusions_check = False
                    break

            if exclusions_check:
                pairings_str = []
                for key, value in pairings.items():
                    pairings_str.append(f"{key} DREW {value}")
                return pairings_str
```

Eventually, I turned this into a usable desktop application, courtesy of the tkinter Python package.

Before the bonafide application was made, I just ran it locally as a script, and sent the results to my MIL to disseminate to the rest of the family. 

## The Better Brute Force Approach with a Sprinkle of Graph Theory

I didn't think much of the results; all the exclusion criteria were met and the script had the flexibility to add more or remove exclusions if needed. But, shortly after the Secret Santa results were circulated to the rest of family, I was reminded that the family I married into has a propensity for nerdiness with this reply-all email:

> There are three circles here. Dad, [Brother #3] and I are part of the smallest circle. 

(This particular BIL has a PhD so I shouldn't have been surprised by his sagacity.)

This comment reminded me of something I had thought about when first approaching this problem: the problem of drawing names for Secret Santa can be represented as a directional graph.

A directional graph (or digraph) is a set of nodes connected by directional edges. For example, if Brother #1 drew Brother #4 last year, then in this year's Secret Santa Brother #1 cannot draw Brother #4, but Brother #4 is allowed to draw Brother #1. We are looking for a Hamiltonian Cycle in our graph: a cycle that goes through every node (family member in Secret Santa) exactly once, ending at the same node where it began.  

In other words, the Secret Santa Problem is a version of the Traveling Salesman Problem (TSP) which asks:

> Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city exactly once and returns to the origin city?

The difference being we are not looking to minimize (or maximize) the distance travelled between all nodes in the Hamiltonian Cycle, we simply want to find any cycle (if it exists). This isn't actually a particularly helpful, since the TSP is a NP-hard problem (nondeterministic polynomial). This means it is **at least** as complex as the hardest (essentially slowest to solve) problems in NP time. In other words, there exist no "fast" solutions or algorithms to solve these problems. Dijkstra's and A* won't work as they find the fastest route between two nodes, not necessarily creating a Hamiltonian Cycle. There are some heuristics that can help on edges cases, and dynamic programming offers an approach to reduce the the number of routes checked in a brute force algorithm

