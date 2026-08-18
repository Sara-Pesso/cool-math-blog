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

<!-- <figure>
<figcaption><b>Figure 1:</b> Baby's First Secret Santa "Algorithm"</figcaption>
<figcaption><b>Note:</b> the variable "exclusions" is simply a Python dictionary of the form {giver:[exclusion1, exclusion2, ...]}</figcaption> -->



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
