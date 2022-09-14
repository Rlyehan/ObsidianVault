222022-09-06

Style: 

Domain: #TestAutomation 

Tags:

# flaky tests
We define a "flaky" test result as a test that exhibits both a passing and a failing result with the same code.

Flaky tests can lead to many issues in any given software project. For one, from developers to managers, there are many people relying on the results of tests to make desicions about the readiness of the software they are developing.

The reasons why the test might be flaky are many, ncluding concurrency, relying on non-deterministic or undefined behaviors, flaky third party code, infrastructure problems, etc.

We can and should make an effort to keep the number of flaky tests small, but trying to eliminate them entirly is hard if not impossible. 

One big issues that can stem from to many flaky tests is that stakeholders start do distrust our test results, with developers arguing that the results are because of flaky tests rather then issues with the software and so on.

So how can we mitigate the problems that come from flaky tests?

There are a number of different approaches we can take. 

One simple one could be that we execute tests 3 times instead of only once. If the test fails 3 out of 3 times it gets marked as a failure, if it passes it gets marked as passed and if it changes between failure and pass it will get marked for review. This strategy can be quit successful but one of the major drawbacks is that the tests need to be executed multiple times. Especially for long running tests this could make them unfeasable for the developers to execute, making them test their changes less frequently and potentially amassing bugs in the process.

We could also put systems in place to monitor the flakyness of our tests to notify developers if tests become to flaky and also what changes caused them to become more flaky.

### Sources of flakiness
-   The tests themselves
-   The test-running framework
-   The application or system under Test (SUT) and the services and libraries that the SUT and testing framework depend upon
-   The OS and hardware that the SUT and testing framework depend upon

next lets go over the different sources in more detail:

**the tests themselves:**
-   Improper initialization or cleanup.
-   Invalid assumptions about the state of test data.
-   Invalid assumptions about the state of the system. An example can be the system time.
-   Dependencies on the timing of the application.
-   Dependencies on the order in which the tests are run. (Similar to the first case above.)

so how can we deal with that?

![[Pasted image 20220906140903.png]]

**The test runnning framework:**
-   Failure to allocate enough resources for the system under test thus causing it to fail coming up. 
-   Improper scheduling of the tests so they “collide” and cause each other to fail.
-   Insufficient system resources to satisfy the test requirements.

![[Pasted image 20220906141014.png]]

**The SUT and its dependencies:**
-   Race conditions.
-   Uninitialized variables.
-   Being slow to respond or being unresponsive to the stimuli from the tests.
-   Memory leaks.
-   Oversubscription of resources.
-   Changes to the application (or dependent services) happening at a different pace than those to the corresponding tests.

![[Pasted image 20220906141034.png]]

**The OS and hardware:**
-   Networking failures or instability.
-   Disk errors.
-   Resources being consumed by other tasks/services not related to the tests being run.

![[Pasted image 20220906141124.png]]


___
# References
[Google Testing Blog: Flaky Tests at Google and How We Mitigate Them](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html)
[Google Testing Blog: Test Flakiness - One of the main challenges of automated testing](https://testing.googleblog.com/2020/12/test-flakiness-one-of-main-challenges.html)
[Google Testing Blog: Test Flakiness - One of the main challenges of automated testing (Part II)](https://testing.googleblog.com/2021/03/test-flakiness-one-of-main-challenges.html)