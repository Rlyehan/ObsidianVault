222022-07-18

Status: #Research 

Tags: #TestAutomation 

# Test Architecture

## What is test architecture
by the ISTQB definition:

>Test Automation Architecture:
An instantiation of the generic test automation architecture to define the architecture of a test automation solution, i.e., its layers, components, services and interfaces.

The goal of test architecture can be described overly simplified as finding the best way to test our application. Some questions test architecture is concerned with are for example:
- what types of tests do we need, and to what extent do we need them to achieve the desired test coverage? 
- What tests do we automate, and where/how should they be executed? 
- What tools do we want to use?

But to answer these more general questions, we also need to look at our application from a more technical point of view to understand what areas to prioritize, how to test these different areas, etc. 

For all of these topics we need to work together with different key personnel like the software architects, business stakeholders and Ops team, to get the full picture and create a test architecture & strategy that will help us to get where we want to, faster. 

It can also be at this point that we start to already go into the rough test case design, so that we can achieve as high a coverage as possible with as few tests as necessary. But for that we again need good collaboration between the developers and testers, or even have the developers be part of the testing so that the code is written and design with testability in mind.

## What are some tools to help with test architecture?

One famous one would be the often cited test pyramid (or the test honeycomb when looking at more microservice style applications). 

But what is this Test pyramid (or honeycomb)?
Basically what we have are tiers of the different types of testing, with the bulk at the bottom and getting smaller and smaller with each consecutive tier, hence a pyramid.

The lowest Tier of testing generally refers to unit tests. The reason for that is that they are tests that cover the smallest part of our system, the individual units of code, and are run in isolation, so they don't require anything but the code and a runtime. This makes them very fast to run, allowing for fast feedback. The isolation also helps to ensure that all we test is the specific code itself. They also represent a critical part of any CI/CD pipeline, you can read more about that in the [[CICD]] section.

The next level in the pyramid is often represented with integration tests. It is also this tier that differentiates between the pyramid and the honeycomb since for microservice systems integration tests can take over the bulk of tests, compared to unit testing.
Integration testing can have many definitions, but the simple one we want to use here is that integration tests are tests used to validate the interaction between components. This definition already shows the most impactful difference between unit and integration tests, the interaction between components. This is also one reason why we want them after unit tests, after all, we want to know that our components work as they should before we want to know if the interaction between them does what we expect. Another reason is that to test these interactions we often need them to be available to us, that can mean that we need to set up docker images, VMs or similar to host those services, make sure that they are up and running, and only then can we run the integration tests. Since these tests also need the components to communicate, the network latency, execution times of the different components, etc. all add up to make the execution times much longer than with unit tests. 

And at the top of the pyramid, the last tier, we have generally some form of acceptance testing, often in the form of what is called user acceptance testing (UAT). This usually refers to tests to testing business processes from start to finish, involving the entire application that we need for that process. The tests on this level represent the interactions a user might have with our application. This types of tests are often still done manually, but if we have a proper test automation architecture in place, than we often can start with automating them.

Disregarding whether we look at a pyramid or a honeycomb, they are both simple yet powerful tools to give as a general guidance in our test architecture, with the underlying principles being applicable to almost any testing initiave.

But using these principles needs a good understanding of the application under test, and of course what it actually means that we should have the bulk of our testing done at the unit test level for example. And it requires an even deeper understanding when we want to adapt the underlying principles to our specific application that might not quite fit into the more generic framework. 

Now you might be wondering, but what about XYZ tests? First, the classic pyramid represents what are often called functional tests, as such tests that are commonly referred to as non-functional, like performance or security, are not considered directly in it. But here are more types of functional tests out there, many we can link to the different tiers of the pyramid. 

To define how we want to go about actually testing our application and what specific types of tests and the associated tools to create and execute them are is not something that can be defined in a general manner, that is why you would want to have a test architect to make the specific testing architecture for you.

## Testing commercial of the shelf software (COTS)

Having read about the testing pyramid you might be thinking, thats nice and all, but I am not developing a software, I'm just using some commercial of the shelf software, so what does any of this have to do with me?

In many cases these COTS solutions are not just installed to runand work on their own but often we want to integrate them into our ecosystem. This means we need to do some level configuration and costumization to get working. So even if we were to trust our supplier completely, these integrations into our system are not something that we can jsut take for granted to be working and as such they need to be tested properly.

Since the software wasn't developed by us, someone might look at the testing pyramid and think, greate since there is no unit testing to be done I just have to do the integration tests and some acceptance test and we are done! But sadly it is not that easy. 

If the application under test is something that we have developed ourselves, then that also means have a a full understanding of how it works, maybe not as an idividual, but we ahve access to the designers, developers and so on that were involved in the development so far and we simply can get clarification if sonethig is unclear. When we are looking at of the shelf sotware however, what we are looking at is basically a black box. The information we have about the system is often very limited, often only what is present in the documentation. 

This fact alone makes designing a good testing framework for the COTS quite complex, and once we add the fact on to that we shouldn't just trust what the suppliers claim, especially in critical areas, it becomes even harder, since we need to figure out ways to verify these claims without beeing able to access the underlying systems fully. As such the testing effort for COTS can often be just as high if not higher than for self developed solutions. And there is an other point that we cannot forget about. The software is often still being further developed to add new features and keep up with new developments, but these changes don't consider how we integrated the software into our ecosystem, as such with each new update to the sotware comes the risk of our integration breaking. To prevent that from happening we need to have tests in place that we can use in the form of regression tests to verify that new updates don't break our integrations, or if they do, give us enough time and information to fix the integration on our side before we need to release the update into our production systems.

Now that we have shown some of the challanges and why testing is a nessacity for any COTS that should be integrated into a wider system, how can a test architecture help us here? One of the biggest impacts could be had if we look at testing from the very begining, so when we are evaluating the different options. If we do this we can evaluate the options faster and in a broader scope, helping us to narrow down the selection by potentially uncovering issues with false or inaccurate claims, or other potential show stoppers like security or data model issues that make the option unfit for us. If we uncovery this problems only during the integration phase of the software we might either lose a lot of time by going back to step one or even worse, have to find fixes for these problems in the integration, often in the form of band-aids rather than proper splints, that are prime targets for regression problems or other issues.

But finding out potential problems earlier is not the only benefit of having testing as part of the evaluation process. It also allows us to get a better understanding of the missing functionality for our implementation, gives us a baseline of the quality we could expect, lets the testers already have experience with the COTS, so that they can focus on the challanges of the integration rather than having to deal with both at the same time, already shows us what infrastructure us needed for further testing, and so on.

But even after the software has been integrated we still need to test that any updates or other cahnges did not impact us. This kind of testing is also known as regression testing. To be more specific, regression testing refers to the pratice of re-running the tests we created to verify that they still pass after chagnes to the system have been made. It is the most common form of testing COTS after they have been integrated into our ecosystem, since we are not invovled in the further development of them. All we need to verify is that our existings tests, an with them all the functionality we need, are still working after any changes. If we have involved testing from the start and have created our tests based on a test architecture and its acomaning strategy, we can have more confidence that the tests cover all the important functionalities that we need to monitor and have been created in a more robust and maintainable manner, meaning that the tests are less likely to break unless there have been major changes to the software and even then they are easier to update, allowing us to have them back up and running faster.



## Test architecture & Test case design 

Our test architecture can have a big impact on our test case design. It can help with understanding the mapping of functionalities from a testing perspective and where we reuse what functionality to better inform us on the full scope we need to test them for. This in turn allows us to make our tests more modular and reuseable, meaning we need to write fewer tests and get better coverage. Modularity and reuseability also often have the side effect of making our tests more robust, if the underlying functionality can be tested properly, and decreasing the maintainance needs.


## DevOps integration

One final key aspect of the test architecture is the collaboration with the Ops team to allign with them on when to execute wich automated test and where to have the manual tests if they are still needed. We also want to have a clear collaboration here so that we can generate the test artifacts we need for analysis and feedback loops.

If our test autoamtion is part of a CI/CD pipeline, we also need to make sure we have a proper managment and review process of our tests in a continuous manner.

To scale efficiently, you must also know the quality of your release, how your quality is trending, and how you can improve. You’ll need insights to help identify failure trends, duplication levels, and team members who may need more training. In general, you can better understand how you perform, why you’re performing well in some areas, and what areas need improvement.

## benefits of test architecture

The benefits of having a proper test architecture in place are numerous and can sually be seen at every stage in the SDLC, but here are a few concret examples:
- shorter feedback times
- proper test coverage
- a proper plan
- better control mechanisms
- better test case and test data managment
- faster investigating failures
- More helpful insights and reports

to name just a few of the ones that are directly associate with test architecture, but the benefits usually go much further, since for example the better planning, controling and managing of test cases makes their quality go up, beeing easier to maintain and less generaly less flaky for example.

## Important factors for test architecture

platform used
code language
requirments
risk assessment
methodology
GxP or not


There are different ways to plan out the testing, like bottom-up or top-down. It is not that important wich approach we choose, they should only serve us as a systematic approach to the planning.

points to consider for our testing plan are both flexibility and scalability, and the impact on our testing should the system scale.

It is also part of the architecture to decide on when to test what. For that we need to consider different factors, like feedback time, significance of the tests, criticality of the function under test, the effort needed to test said function, etc.




___
# References
[Test Architecture: A Holistic Look at Application Testing - AI-driven E2E automation with code-like flexibility for your most resilient tests](https://www.testim.io/blog/test-architecture/
[Test Automation Framework Architecture - Simple Programmer](https://simpleprogrammer.com/test-automation-framework-architecture/)
[Assessment System for Tests' Architecture Design](https://www.automatetheplanet.com/assessment-system-tests-architecture-design/)
[Test Automation Framework: What is, Architecture & Types](https://www.guru99.com/test-automation-framework.html)