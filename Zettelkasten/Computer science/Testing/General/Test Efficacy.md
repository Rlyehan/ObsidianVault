222022-09-08

Style: 

Domain: #TestAutomation 

Tags:

# Test Efficacy
Test efficacy tries to quantify the value of the individual test. Some tests might be very valuable because they deliver a relaible break signal for critical code. Others might not be very valuable because they are non-deterministic or never failed. This vlaue for each test might also change over time.

The only important results come from tests which deterministically fail. Running an additional test that you know will pass is not a valuable signal to developers, and likely a waste of resources.

## pre-submit

A presubmit executes all of the tests which are known to be affected by the edited code within one user's proposed code changes. The "affected tests" are calculated with the help of a "project definition", a configuration maintained by teams. A presubmit can run at any point during the change proposal process, but most importantly it must run before a user can permanently commit their changes.

## Efficacy Presubmit Service

  
Efficacy Presubmit Service is the fusion of "running the right tests at the right time" with one of the largest collections of test/developer data in the world. The service has one simple job: save time and resources by not running, or even compiling, tests that we are very confident will pass at Presubmit. The ideal "Efficacy Presubmit" would predict which tests will pass ahead of time and only run tests which were going to fail. Then the user can get feedback from the failing tests, and fix their mistakes with the minimal possible cost of user and CPU time.

  

To make this idea possible we have made one significant abstraction of the actual presubmit testing process. In a given presubmit there may be zero tests run, or many. In a presubmit with one test, if that test fails then the presubmit fails. In a presubmit with a thousand tests, only one failing test will still fail the presubmit. Efficacy Presubmit makes the abstraction that each of these test executions is an equivalent unit. This greatly simplifies creating a training dataset.

#### Efficacy's Application of ML

Given the abstraction on the presubmit testing process described above, predicting the outcomes of automated testing at a large company is a perfect machine learning problem in many ways. You have:

  

-   The set of test executions and results is a very large labelled dataset
-   Copious numerical feature columns with trustworthy values

1.   Recent failure history of each test
2.   Various "distance" metrics from edited source files to tests - i.e. is this a test for the edited code?
3.  Test size and runtime data

-   Several dimensions that can be aggregated

There are some aspects of the problem which make ML difficult as well:

  

1.  The classes are highly imbalanced with respect to labels (the vast majority of tests are going to pass, not fail)
2.  [Flaky tests](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html) can mislead the model because their labels are "untrue"

  

We chose to reduce the problem to binary classification. The model chooses whether or not to run the test. In other words, failure is the positive class, and everything else is the negative class.

We pick a threshold that results in an extremely low number of false negatives - failing tests which are not run because the model thinks they would have passed. This does reduce the number of skipped tests, true negatives, in exchange for a very high margin of safety. In addition to this, tests will be run afterwards at continuous build anyway, making presubmit skipping very safe.







___
# References
[Google Testing Blog: Efficacy Presubmit](https://testing.googleblog.com/2018/09/efficacy-presubmit.html)