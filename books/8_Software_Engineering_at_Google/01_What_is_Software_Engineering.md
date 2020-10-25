## Part 1: Thesis

### 1. What Is Software Engineering?

* > Software engineering is programming integrated over time
* Programming is the act of coding. Software engineering is everything that involves making sure (1) the code lasts a long time, (2) it can scale and grow, and (3) that involves making educated, long-term trade-offs
* Time and Change
    * *Hyrum’s Law*: As the number of people using your API grows, what matters is not what your API contract promises but what observable behavior your system has ([reference](https://www.hyrumslaw.com) | [relevant xkcd](https://xkcd.com/1172/))
        * If you implement a hash table, people might start depending on the ordering of keys. This is not part of the API contract but observable behavior
        * Even if you randomize that ordering, people might start depending on it as a random number generator
    * Programming being clever is a compliment. Software engineering being clever is an accusation
    * Rejecting change is not an option over a long time because of security updates and things becoming more efficient
* Scale and Growth
    * Google’s *churn rule*: When a library is updated, the developers need to move their customers to a new version, *not* the other way around (which is how it is commonly done). This scales much better because it does not require all customers to learn about the details of the new version
    * Feature branches and merging scale badly because feature developers need to merge in more and more changes
    * (*Note: These are discussed in more details in upcoming chapters*)
    * > *Beyonce Rule*: If you liked it, you should have put a CI test on it
    * When an infrastructure update breaks something that wasn’t caught by CI, it’s not the fault of the infrastructure update
    * The earlier in the developer workflow you find problems the cheaper it is to fix them
* Trade-offs and Cost
    * To help developers figure out what is worth implementing, consider publishing how much CPU hours and RAM cost you. That way a developer can compare what they should be working on and for how long
    * *Jevons paradox*: When something becomes more efficient, people pay less attention to how how inefficiently they use it (e.g. light bulbs, or here: distributed build systems)
    * There is a factor of 100,000 difference in how long code needs to live for (hours vs decades). This greatly influences what kinds of things are important
    * Everything your organization needs to do regularly should scale linearly
