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
- test coverage
- test performance
- rework or replace flaky tests (descibe what flaky means in this context)


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

[Test Case Design: a Guide for QA Engineers With Examples - Testim Blog](https://www.testim.io/blog/test-case-design-guide-for-qa-engineers/)
[Google Testing Blog: Code Health: Respectful Reviews == Useful Reviews](https://testing.googleblog.com/2019/11/code-health-respectful-reviews-useful.html)
[Google Testing Blog: Mutation Testing](https://testing.googleblog.com/2021/04/mutation-testing.html)
https://www2.stardust-testing.com/en/best-practices-regression-testing
[Best Practices to Follow for Regression Testing in Agile](https://www.testingxperts.com/blog/regression-testing-best-practices)
[Regression testing - Wikipedia](https://en.wikipedia.org/wiki/Regression_testing#Techniques)
[Regression testing best practices and techniques](https://screenster.io/regression-testing/)
[Regression Test Plan: A Checklist for Quality Assurance](https://www.testim.io/blog/regression-test-plan-a-checklist-for-quality-assurance/)
