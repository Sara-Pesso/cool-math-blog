---
title: "The Secret Santa Question"
date: 2026-08-17
image: /assets/images/digraph example.png
---

My husband being one of eight children (half of which are married with kids) means that getting a gift for everyone in the family each Christmas isn't financially feasible. To remedy this, my MIL began the tradition of using a Secret Santa approach to gift giving: each member of the family is (randomly) assigned to give one other person a gift. This way, we know everyone is giving to one other person, and receiving from one other person.

Of course, each family members' spouse shouldn't be drawn as their giftee-- obviously, whether or not a husband draws their wife in the family Secret Santa, they are still going to get their wife a Christmas present lest they incur her well-deserved ire. Also, we want to make sure no one has drawn the same name two years in a row. These problems arose for my MIL in the free Secret Santa name drawing applications available online-- they don't allow for these types of exclusions.

So, -- in what could only be described as abject desperation, no doubt -- she sent out a request to the Python-inclined among the family asking for a Secret Santa algorithm and desktop application that could intake these exclusions as a CSV file and account for such exclusions when drawing names for the upcoming Christmas. 

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

Before the bona fide application was made, I just ran it locally as a script, and sent the results to my MIL to disseminate to the rest of the family. 

## The Better Brute Force Approach with a Sprinkle of Graph Theory

I didn't think much of the results; all the exclusion criteria were met and the script had the flexibility to add more or remove exclusions if needed. But, shortly after the Secret Santa results were circulated to the rest of family, I was reminded that the family I married into has a propensity for nerdiness with this reply-all email:

> There are three circles here. Dad, [Brother #3] and I are part of the smallest circle. 

(This particular BIL has a PhD so I shouldn't have been surprised by his sagacity.)

This comment reminded me of something I had thought about when first approaching this problem: the problem of drawing names for Secret Santa can be represented as a directional graph and we could probably find a Hamiltonian Cycle between all the nodes (family members).

A directional graph (or digraph) is a set of nodes connected by directional edges. For example, if Brother #1 drew Brother #4 last year, then in this year's Secret Santa Brother #1 cannot draw Brother #4, but Brother #4 is allowed to draw Brother #1. We are looking for a Hamiltonian Cycle in our graph: a cycle that goes through every node (family member in Secret Santa) exactly once, ending at the same node where it began.  

In other words, the Secret Santa Problem is a version of the Traveling Salesman Problem (TSP) which asks:

> Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city exactly once and returns to the origin city?

The difference being we are not looking to minimize (or maximize) the distance traveled between all nodes in the Hamiltonian Cycle, we simply want to find any cycle (if it exists). This isn't actually a particularly helpful, since the TSP is a NP-hard problem (nondeterministic polynomial). This means it is **at least** as complex as the hardest (essentially slowest to solve) problems in NP time. In other words, there exist no "fast" solutions or algorithms to solve these problems. Dijkstra's and A* won't work as they find the fastest route between two nodes, not necessarily creating a Hamiltonian Cycle. There are some heuristics that can help on edges cases, and dynamic programming offers an approach to reduce the the number of routes checked in a brute force algorithm.

This is where representing the allowed giver-receiver pairs in our Secret Santa drawing as a digraph comes in handy. Turning this information into a matrix where a 1 represents an allowable giver-receiver pairing and 0 is an exclusion, we can apply Depth First Search (DFS), also known as backtracking, to traverse our graph (matrix for the computer) to find a Hamiltonian Cycle (if one exists) using Python.

For the uninitiated, DFS is simply a search algorithm. Starting at any node, call it node 0, we traverse to an adjacent node, call it node 1. At node 1, we attempt to traverse to a new node that we haven't already passed through. If such a node exists, we traverse to it, call it node 2. If no such node exists, we backtrack to node 0 and attempt to traverse to an adjacent node that is not node 1. Then this process repeats until either 1) a Hamiltonian Path is found, landing us back at node 0, or 2) we have exhausted every possible path and can conclude that our digraph is non-Hamiltonian. 

For example in the diagram below, starting at Husband A, we first traverse to Wife B using DFS, which is a legal move. Then, from Wife B, we go to Unmarried Sibling 1. From Unmarried Sibling 1 we first try Unmarried Sibling 2, then to Husband B, but there is no way to pass through Wife A without first going back to Husband A (the start of the Cycle). So we have to backtrack to Unmarried Sibling 2, then go to Wife A instead, so on and so forth until the non-unique Hamiltonian Cycle is found. 

![Example Digraph](../../../assets/images/digraph%20example.png)

The basic pseudocode the Python Script uses is something like this:

```text 
1. Make matrix describing the directional edges of the graph (that is, allowed/excluded giver-receiver pairings)

2. Initialize an empty 1 x n (where n is the number of family members in the Secret Santa drawing) vector that will hold the node order of the Hamiltonian Cycle (should it exist)

3. Depth First Search:
    - Save current node in Hamiltonian Cycle vector
    - From current node
    - Iterate through all other nodes checking if:
        1) Possible node is adjacent to node X
        2) Possible node is not already in path
        If conditions are met, save Possible node as current node, and repeat DFS
    - Repeat DFS until back at node 0

4. Once (or if) a Hamiltonian Cycle is found through the Secret Santa digraph, display the results. 
```

The full Python script implementing DFS using a user inputted CSV of exclusions can be found on my GitHub [here](https://github.com/Sara-Pesso/secret-santa-app/blob/main/secret_santa_graph.py).

The resultant Hamiltonian Cycle basically gives us a path that tells us "A gives to B, who gives to C, who gives to ..., who gives to A", rather than using a guess and check method. If the digraph was non-Hamiltonian, we would simply break the family in smaller cycles using DFS. Each time DFS finds a smaller cycle, those family members are removed from consideration until all family members have been used in a cycle. 

Was this a little overkill for a pretty simple ask from my MIL? Yes, definitely. But it was a lot of fun to approach this problem from a couple different ways and ultimately prove to myself I don't need a Clanker to be mediocre at math, I'm more than capable of doing that myself. Ultimately, I'd like to make my Secret Santa tkinter app use the graph theory/DFS script instead, but for this Christmas the brute force search will have to work! For anyone interested in playing with the current, brute-force version of the desktop app it can be downloaded from my GitHub as [gui.exe](https://github.com/Sara-Pesso/secret-santa-app/blob/main/build/gui).
