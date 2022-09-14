222022-07-22

Status: #Idea

Tags: #DevOps

# continuous delivery

In continuous delivery we build our software in a way so that it can be released to production at any time.

that meanst that:
- our software is deployable throughout it's lifecycle
- We prioritize keeping our software deployable over creating nre features
- we can perfome push button deployments of any version to any environment on demand

In oder to enable continuous delivery we must first have a robust [[continuous integration]] pipeline in place. Furthermore we must build and test our software in increasingly production like environments by using [[deployment piplines]].

A key test is to see that you can deploy any version of your software on request into production at a moments notice.

Continuous delivery often gets confused with [[continuous deployment]]. It would be acurate to see continuous delivery as a required step before we would be able to achieve continuous deployment if we wanted to, as is [[continuous integration]] for continuous delivery.

But implementing continuous delivery is not an easy process as DORA reserch has show us.

![[Pasted image 20220728144519.png]]
![Diagram of the J curve of typical transformations, from the
2018 State of DevOps Report.](https://cloud.google.com/static/architecture/devops/images/continuous-delivery-2.png)

In the diagram, the following stages are labeled:

-   At the beginning of the curve, teams begin transformation and identify quick wins.
-   In an initial improvement, automation helps low performers progress to medium performers.
-   In a decrease to efficiency—the bottom of the J curve—automation increases test requirements, which are dealt with manually. A large amount of technical debt blocks progress.
-   As teams begin to come out of the curve, technical debt and increased complexity cause additional manual controls and layers of process around changes, slowing work.
-   At the top of the curve, relentless improvement work leads to excellence and high performance. High and elite performers leverage expertise and learn from their environments to see increases in productivity.

As we can see there are certain bottlemecks that we can run into. These bottlenecks are also often the reason why teams abandon proper CI/CD. But there are ways that they can be mitigated. One way is to start out with value stream mapping. Another on is to monitor and measure the continuous delivery pipeine. here teams can run into the issue of looking at the wrong metrics, leading them to believe that they are not getting as much benefits from the practices as the actually are. The DORA key metrics for example would be:

-   Short lead times for both regular and emergency changes, with a goal of using your regular process for emergency changes.
-   Low change fail rates.
-   Short time to restore service in the event of outages or service degradations.
-   Release frequencies that ensure that high-priority features and bug fixes are released in a timely manner.

___
# References
