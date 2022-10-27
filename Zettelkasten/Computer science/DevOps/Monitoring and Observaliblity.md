222022-10-27

Style: 

Domain:

Tags:

# [[Monitoring]] and [[Observability]]

In general, we tend to measure at three levels: network, machine, and application. Application metrics are usually the hardest, yet most important, of the three.

These are the four pillars of the Observability Engineering team’s charter:

- Monitoring  
- Alerting/visualization  
- Distributed systems tracing infrastructure  
- Log aggregation/analytics

Blackbox monitoring — that is, monitoring a system from the outside by treating it as a blackbox — is something I find very good at answering the _what is broken_ and alerting about a problem that’s already occurring (and ideally end user-impacting). Whitebox monitoring, on the other hand, is fantastic for the signals we can _anticipate_ in advance and be on the lookout for. In other words, whitebox monitoring is for the **_known, hard_** failure modes of a system, the sort that lend themselves well toward exhibiting any deviation in a _dashboardable_ manner, as it were.

A good example of something that needs “monitoring” would be a storage server running out of disk space or a proxy server running out of file descriptors. An I/O bound service has different failure modes compared to a memory bound one. An AP system has different failure modes compared to a CP system.

Building “monitorable” systems requires being able to understand the failure domain of the critical components of the system **_proactively._** And that’s a tall order. _Especially_ for complex systems. More so for simple systems that interact _complexly_ with one another.

As strange as it might sound, I’m beginning to think one of the design goals while building systems should be to make it as _monitorable_ as possible — which means minimizing the number of unknown-unknowns.

The corollary of the aforementioned points is that _monitoring_ data needs to _actionable._ What I’ve gathered talking to many people is that when not used to directly drive alerts, monitoring data should be optimized for providing a bird’s eye view of the _overall health_ of a system.

Quoting the SRE book again:

> It can be tempting to combine monitoring with other aspects of inspecting complex systems, such as detailed system profiling, single-process debugging, tracking details about exceptions or crashes, load testing, log collection and analysis, or traffic inspection. While most of these subjects share commonalities with basic monitoring, blending together too many results in overly complex and fragile systems.

The SRE book doesn’t quite use the term “observability”, but clearly lays out everything that “monitoring” isn’t and shouldn’t aim to be.

“Monitoring” is best suited to report the overall health of systems. Aiming to “monitor everything” can prove to be an anti-pattern. Monitoring, as such, is best limited to key business and systems metrics derived from time-series based instrumentation, known failure modes as well as blackbox tests.

![[Pasted image 20221027105404.png]]



Observations can lead a developer to the answers, it can’t make them necessarily find it. The process of examining the evidence (observations) at hand and being able to deduce still requires a good understanding of the system, the domain as well as a good sense of intuition. No amount of “observability” or “monitoring” tooling can ever be a substitute to good engineering intuition and instincts.



___
# References
[Monitoring and Observability. During lunch with a few friends in late… | by Cindy Sridharan | Medium](https://copyconstruct.medium.com/monitoring-and-observability-8417d1952e1c)