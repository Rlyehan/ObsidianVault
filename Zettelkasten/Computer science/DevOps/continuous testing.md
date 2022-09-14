222022-07-22

Status: #Idea

Tags: #DevOps 

# continuous testing

continuous testing can be seen as the next step after [[automated testing]] and is a key part of any successful [[CICD]] impelentation.

You could say that without continuous testing any form of [[continuous integration]]
and by extention [[continuous delivery]] are not possible.

That comes from the fact that the pipelines are to be executed at least once a day, but generally even multiple times a day, whenever a dev is ready to check in the small incremental changes he made. If we were to still rely on manual testing for that, we would most likely end up with the tests lagging behind the development so far that the feedback loop would stop working as intended and the whole CI/CD process fails.

Add to that the fact that the tests we run, especially for regression, hardly ever change, [[automated testing]] is the logical next step.

Once the tests have been automated they can run as part of our CI/CD process and the feedback times are reduced to minutes for most issues, allowing developers to fix them before they move on to the next change to implement.

So in general it is not so much a question of should we automate tests but more one of where we should start. 

In order to do so we must map out our [[deployment piplines]] and see what test we can execute at what stage, here it is also important to make sure that we take the feedback time into consideration. Next we must look at the feedback loop and identify the areas were short feedback times are of most value. And we also want to prioritize waht to automate properly(backlink to the relevant section).

But all of these benefits are of little help if the developers are not involved properly. As such it is important to have the developers create some tests on their own, at a minimum the unit tests for teh code they created. But even for tests beyond that level the developers should for example not be allowed to set the task to completed unless the build based the testing phase entirely. 

![[Pasted image 20220727162547.png]]
The automated tests in the diagram above shoud be part of the [[deployment piplines]] in place. From there the pipeline gets split up into multiple stages, and we need to identify and create the automated test for each one, this could then look like this:
![[Pasted image 20220727163233.png]]
This shows us the entire [[continuous delivery]] process. One aspect of continuous tesitng that we don't see here is testing in production. 

Some of the main issues we might run into with continuous testing are:
- **Broken test suits**. one of the main factors here is if the developers are invovled into the testong or not. In general, if they are that leads to a more robust testing suite and tests that are updated regularly to reflect code changes. Another Issue that can often comes from not involving the developers in the testing is that the code they create is hard to test.

- Another point is that we must **curate our test suite regularly**. That can include things like pruning out dated tests, updating or creating tests, and also topics like checking our test suit against the proposed test architecture. We need to make sure that the testing suit stays track with any new development also, so we might want to monitore things like the number of the different types of tests created against the changes in our Jira boards for example.


### Test data managment

Test data is always one of the key topics we must pay attention to, whether we are running automated tests or doing them manually. 

criteria:
- We need data for all the different scenarios our test should test.
- The data needs to be easily accessable and can be aquired on demand
- The data is not a limiting factor for our test suite
- sensitive data has been masked or otherwise been dealt with
- the data is not just a copy of production data
- The data is out-dated
- Tests should not overly rely on the data, depending on the type of test of course

So that we can manage our test data properly there are some key aspects we should pay attention to:
- **Favor unit tests**. Unit tests should be independent of each other and any other part of the system except the code being tested. Unit tests should not depend on external data. As defined by the [test automation pyramid](https://martinfowler.com/articles/practical-test-pyramid.html#TheTestPyramid), unit tests should make up the majority of your tests. Well-written unit tests that run against a well-designed codebase are much easier to triage and cheaper to maintain than higher-level tests. Increasing the coverage of your unit tests can help minimize your reliance on higher-level tests that consume external data.

-  **Minimize reliance on test data**. Test data requires careful and ongoing maintenance. As your APIs and interfaces evolve, you must update or re-create related test data. This process represents a cost that can negatively impact team velocity. Hence, it's good practice to minimize the amount of test data needed to run automated tests.

- **Isolate your test data**. Run your tests in well-defined environments with controlled inputs and expected outputs that can be compared to actual outputs. Make sure that data consumed by a particular test is explicitly associated with that test, and isn't modified by other tests or processes. Wherever possible, your tests should create the necessary state themselves as part of setup, using the application's APIs. Isolating your test data is also a prerequisite for tests to run in parallel.

- **Minimize reliance on test data stored in databases**. Maintaining test data stored in databases can be particularly challenging for the following reasons:
    
- **Poor test isolation**. Databases store data durably; any changes to the data will persist across tests unless explicitly reset. Less reliable test inputs make test isolation more difficult, and can prevent parallelization.
- **Performance impact**. Speed of execution is a key requirement for automated tests. Interacting with a database is typically slower and more cumbersome than interacting with locally stored data. Favor in-memory databases where appropriate.

- **Make test data readily available**. Running tests against a copy of a full production database introduces risk. It can be challenging and slow to get the data refreshed. As a result, the data can become out of date. Production data can also contain sensitive information. Instead, identify relevant sections of data that the tests require. Export these sections regularly and make them easily available to tests.





___
# References
