222022-07-22

Status: #Idea

Tags: #TestAutomation 

# Investigating failures

**Make sure that we consider teams that are only doing regression testing for of the shelf software**

The reporting of our test results might be the one of the last steps in the testing process, but it can be argued to be the most valuable one. We say that because it is at that stage were any real value from our tests is generated. The value of passing tests is easy to understand and needs no explanation, but the value of failed tests is a different topic.

At first the fact that the test failed might seem valuable, but that fact alone actually has little value by itself. After all, what we want to know when a test fails is not that it failed, but why it did. As such the investigation of the failure is what makes that result valuable.

But what do we mean when we say failure investigation? What we refer to is inspecting the test results in order to pinpoint the cause of failure as much as possible so that this information in turn enables developers to fix the problem quickly. 

So what do we need to do? 

That is a tricky question to answer as it depends on what type of test failed and what kind of system we are testing, but some general points are:

- **Have test atrifacts at hand that help us**. This can be exections thrown by our code, screenshots of where the UI failes, logs, traces, etc.

- **Fint the root cause**.  Here we mus different between two different classes of root causes, **false negatives** and **applications failures**. 

 **make sure that the results is not a false negative**. This is the first step where we really look into the information that we have. Finding false negatives can be hard, to the point that we can run the risk of spending far to much time to verify that the failure was in fact a failure of the application instead of finding out what casued the failure. As such it often is best to just look into the most likely scenarios for a false negative. 
 
 To give you an example of what a fals enegative might be: let's say we want to test the UI of our App. One easy to find false negative here could be that the front-end has not be deployed yet when we executed oru test, so of course it failed. This would suggest that there might be an issue with the deplyoment automation rather then the UI itself and we would have wasted our time if we would not have checked the build logs showing us that. Another common one would be that what failed was not the application but our test. What we mean here is for the UI example could be that we have a wrong class name for the component we want to use, casuing the test to fail. 
 
 In both cases the deciding factor if we are able to identify them as false negatives fast are the artifacts that are available to us. In the first case we would need to have access to understandable build and execution logs that show us that we executed the test before the UI was built. In the second step an execution log that gives as an informative error what we want. it makes a huge difference here if the log tells us that the test failed because the element specified could not be found instead of just saying that the test failed.

**Failure scope** is something need to consider. If we have a CI/CD process we hopefully can isolate the scope to the most current build version, but if the test is a newly introduced one, it still can make sense to analyse the potential failure scope.

**Failure criticality** is also something that we need to consider. 

**automatically run failed tests a second time.** Sometimes there could be small issues that caused our test to fail, like network latency. In those cases having the test run a second time might have it pass. Since these issues have nothing to do with our build we can still go forward, but still get information out of the run showing us that we have certaint tests that might need to be looked at.

**have a failure playbook**. this helps us be faster by having the process outlined for us. 


### Why test fail

There are many reasons why our test might fail. But it is important to note here that the fact that our tests are failing is not bad in general, usually it's quite the opposite. By having our tests fail we were able to find a problem in our system and get the information we need to find out what caused the issue so that we can fix it before we release the system to production.

But that is only one side of the coin. There are many reasons why our tests can fail, and a good number of them might fail because of reasons that have nothing to do with the application we are creating, or even the tests we have written. There a few different ones in the section "frequent types of failures" further below.

### How do we report failures?

The way we have to report failures depends on the specific project. it sould be defined once in a failure playbook so that there will be a standardized way we can create the reports and know where to look for them and what to expect when we read one.


### Investigating the failure cause

For root cause analysis we are generally interrested ind three things:

What failed?
What caused the failure?
How can we fix teh problem?

Root cause analysis can quickly become a very technical and complex topic, depending on the systems and circumstances involved. But for now we want to keep it simple here.


Let's go this with an example:

We have developed a feature for our application that displayes some dataset. The data is stored in an database, as such we need created an API that allows us to retrieve that data. 

Now we run an integration tests to verify that the API returns the expected data and the test fails.

Since we know that we could run into the issue that the database needs to spin up before ut is able to handle our request, we made sure to add some policy for a timeout and retry so that this problem wouldn't turn into a show stopper.

But the test still failed.

So what do we do next?

This depends a bit on how exactly we go abour reporting and managing a failure, but we will leave these steps out here and focus on finding the root cause from now on.

Since we are dealing with an API here, there is a really helpful feature that is usually included, the status code of the response. For agruments sack we assume here that they have been implemented properly. Many of these error messages together with their descriptions can make it easy for us to find what caused the issue, some more so than others. 

So why did our test fail?

for our example lets say we get the staus code 400, telling us that our request was not a valid one but expected a 200 OK. 

So now we know why the test failed, but why was our request bad?

We check the request we send agaisnt the sepcification, see that there is a discrepency between them, fix the request and run the test again. This time everything worked as it should, the test passed.

Many think we would be done here, but that would only be treating the symptoms rathern then the cause.

So next we need to ask ourselves, why was there a discrepancy?

This is the point where our investigation can become quite tricky, even though it looked like such a simple problem, but if we stop here there is a good cahnce that we would run into similar problems again and if we don't deal with it early it might become so big that we can't handle it without a massive impact on our project.

So we look into the problem and see that the specification was changed by the developer, also explaining why the unit tests passed without issue.

From here we might have multiple questions, like why was it changed and why was the test not updated?

And once we have answers hare we might have to go even deeper.


The example above might seem like a simple one, and it might actually be, but it still serves it's purpose, showcasing that we have an structured approach to how we investigate the issue based on what we are dealing with.

Here we were investigating the failure quit infomaly simply but asking why, but for more complex issues we might need the support of a more structured system, thas something the failure playbook is for.

### Frequent types of failures and their root causes

- **Context**
One reason why our tests faile could be because of changes in the context they are executed in. Examples could be that we execute tests against services that are not yet deployed, that the credentials were changes, etc.

But these context related failures can also have other causes, like the update to a package we use without a specified version might break some functionality, or a new browser version might be incompatible with our test runner.

One more thing that we need to be aware of here is that we could break the context ourselves if we don't follow best pratices when creating our tests.

- **Flaky tests**
Sometimes there might be issues with the tests themselves, making them what we call "flaky". The two main reasons for this are not adhering to the best practices (LINK best pratices) or when trying to automate tests where this is not appropriate (LINK what to automate)

- **Code changes**
Another common failure reason are code changes. If the failures for these cahnges appear at the unit tesitng level, that is good, we catch the problem and either fix it or roll back until we were able to fix the issue. But when code change related failrues crop up at higher levels of testing these can lead to problems, one beeing finding the root cause.

___
# References
[Foresight Blog | Why Are Your Tests Failing?](https://www.runforesight.com/blog/why-are-your-tests-failing)
[What is Root-Cause Analysis? Templates and Examples](https://www.spiceworks.com/tech/devops/articles/what-is-root-cause-analysis/)
