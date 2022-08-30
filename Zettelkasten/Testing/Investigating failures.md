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

**automatically run failed tests a second time.** Sometimes there could be small issues that caused our test to fail, like network latency, or explicit waits that. In those cases having the test run a second time might have it pass. Since these issues have nothing to do with our build we can still go forward, but still get information out of the run showing us that we have certaint tests that might need to be looked at.

**have a failure playbook**. this helps us be faster by having the process outlined for us. 



### Why test fail

### How do we report failures?

### Investigating the failure cause

### Frequent types of failures and their root causes



___
# References
