222022-07-22

Status: #Idea

Tags: #TestAutomation 

# Test Automation Best Pratices

### General best practices:
- tests should be focused
- tests should be modular
- tests shoud be parameterized
- tests should be maintainable
- tests should be readable
- tests should generated actionable artifacts

### more specific goals to achieve:
- test/feature/behaviour coverage
- [[Code Coverage]]
- test performance
- rework or replace flaky tests (describe what flaky means in this context)

It is also important to talk about testing from not only from the perspective of what it is, but also waht it is for and what it should mean for the team. Testing, and by extension quality, should not be seen as some elses concern. If we want to make sure to delivery hihg quality ptoducts quality is everyones concern and starts before the first line of code was written. An testing in that context is not that evil thing that hinders us from pushing to production, but it is a verification that we the features we created work as we expected them to. to make an analogy, seeibng the test for the feature we created pass should be see as teh same as when your favorit soccer team scores a goal. It should be something that makes th team happy.  So if we can have our team get into a quality first mindset, than that also means that the testing becomes part of daily operations and everyones concern. And that in turn leads to better code quality, better tests, more confidence in the products the team creates and generally also to faster turn over times.

### How much testing is enough?

This is a common question that we ask ourselves, how much testing do I need to do? Sadly that question is a hard one to answer in definitive terms. To make this clearer we should have a testing strategy for the specific project.

To get started here are some helpful tips to get started:

-   **Document your process or strategy.**
-   **Have a solid base of unit tests.** At many companies, including Google, there are best practices of requiring any code change to have corresponding unit test cases that pass. As the code base expands, having a body of such tests that is executed before code is submitted is an important part of catching bugs before they creep into the code base. This saves time later both in writing integration tests, debugging, and verifying fixes to existing code.
-   **Don’t skimp on integration testing.** As the codebase grows and reaches a point where numbers of functional units are available to test as a group, it’s time to have a solid base of integration tests. An integration test takes a small group of units, often only two units, and tests their behavior as a whole, verifying that they coherently work together.
-   **Perform end-to-end testing for Critical User Journeys.** The discussion thus far covers testing the product at its component level, first as individual components (unit-testing), then as groups of components and dependencies (integration testing). Now it’s time to test the product end to end as a user would use it. This is quite important because it’s not just independent features that should be tested but entire workflows incorporating a variety of features. At Google these workflows - the combination of a critical goal and the journey of tasks a user undertakes to achieve that goal - are called Critical User Journeys (CUJs).
-   **Understand and implement the other tiers of testing.**
-   **Understand your coverage of code ([[Code Coverage]]) and functionality.**
-   **Use feedback from the field to improve your process.**

### Test data managment

Test data is always one of the key topics we must pay attention to, whether we are running automated tests or doing them manually. 

criteria:
- We need data for all the different scenarios our test should test.
- The data needs to be easily accessable and can be aquired on demand
- The data is not a limiting factor for our test suite
- sensitive data has been masked or otherwise been dealt with
- the data is not just a copy of production data
- The data is not out-dated
- Tests should not overly rely on the data, depending on the type of test of course
- The data should be version controlled where applicable

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
