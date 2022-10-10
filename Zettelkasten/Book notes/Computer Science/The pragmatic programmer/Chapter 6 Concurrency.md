222022-09-13

Style: #BookNotes 

Domain: #Development 

Tags:

# Chapter 6 Concurrency

> Shared State isn Incorrect State.


### Blackboards

**Messaging systems can be like blackboards**
>These messaging systems (such as Kafka and NATS) do far more than simply sned data from A to B. In particular, they offer persistance (int the form of an event log) and the ability to retrieve messages through a form of pattern matching.

**But it's not that simple**
>You'l also need good tooling to be able to trace messages and facts as they progress through the system. (A useful technique is to add a unique trace id when a particular business function is initiated and then propagate it to all the actors involved. You'll then be able to reconstruct waht happens from teh log files.)


___
# References
