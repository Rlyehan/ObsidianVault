222022-09-08

Style: 

Domain: #TestAutomation 

Tags:

# Test Plan
Creating a test plan is often a complex undertaking. An ideal test plan is accomplished by applying basic principles of [cost-benefit analysis](https://en.wikipedia.org/wiki/Cost%E2%80%93benefit_analysis) and [risk analysis](https://en.wikipedia.org/wiki/Risk_analysis), optimally balancing these software development factors:

-   **Implementation cost**: The time and complexity of implementing testable features and automated tests for specific scenarios will vary, and this affects short-term development cost.
-   **Maintenance cost**: Some tests or test plans may vary from easy to difficult to maintain, and this affects long-term development cost. When manual testing is chosen, this also adds to long-term cost.
-   **Monetary cost**: Some test approaches may require billed resources.
-   **Benefit**: Tests are capable of preventing issues and aiding productivity by varying degrees. Also, the earlier they can catch problems in the development life-cycle, the greater the benefit.
-   **Risk**: The probability of failure scenarios may vary from rare to likely, and their consequences may vary from minor nuisance to catastrophic.

Effectively balancing these factors in a plan depends heavily on project criticality, implementation details, resources available, and team opinions. Many projects can achieve outstanding coverage with high-benefit, low-cost unit tests, but they may need to weigh options for larger tests and complex corner cases. Mission critical projects must minimize risk as much as possible, so they will accept higher costs and invest heavily in rigorous testing at all levels.

### Test plan vs. strategy

Before proceeding, two common methods for defining test plans need to be clarified:

-   **Single test plan**: Some projects have a single "test plan" that describes all implemented and planned testing for the project.
-   **Single test strategy and many plans**: Some projects have a "test strategy" document as well as many smaller "test plan" documents. Strategies typically cover the overall test approach and goals, while plans cover specific features or project updates.

Either of these may be embedded in and integrated with project design documents. Both of these methods work well, so choose whichever makes sense for your project. Generally speaking, stable projects benefit from a single plan, whereas rapidly changing projects are best served by infrequently changed strategies and frequently added plans.

### Content selection
  
A good approach to creating content for your test plan is to start by listing all questions that need answers. The lists below provide a comprehensive collection of important questions that may or may not apply to your project. Go through the lists and select all that apply. By answering these questions, you will form the contents for your test plan, and you should structure your plan around the chosen content in any format your team prefers. Be sure to balance the factors as mentioned above when making decisions.

  
  

### Prerequisites

  

-   **Do you need a test plan?** If there is no project design document or a clear vision for the product, it may be too early to write a test plan.
-   **Has testability been considered in the project design?** Before a project gets too far into implementation, all scenarios must be designed as testable, preferably via automation. Both project design documents and test plans should comment on testability as needed.
-   **Will you keep the plan up-to-date?** If so, be careful about adding too much detail, otherwise it may be difficult to maintain the plan.
-   **Does this quality effort overlap with other teams?** If so, how have you deduplicated the work?

  

### Risk

  

-   **Are there any significant project risks, and how will you mitigate them?**Consider:
    -   Injury to people or animals
    -   Security and integrity of user data
    -   User privacy
    -   Security of company systems
    -   Hardware or property damage
    -   Legal and compliance issues
    -   Exposure of confidential or sensitive data
    -   Data loss or corruption
    -   Revenue loss
    -   Unrecoverable scenarios
    -   SLAs
    -   Performance requirements
    -   Misinforming users
    -   Impact to other projects
    -   Impact from other projects
    -   Impact to company’s public image
    -   Loss of productivity
-   **What are the project’s technical vulnerabilities?** Consider:
    -   Features or components known to be hacky, fragile, or in great need of refactoring
    -   Dependencies or platforms that frequently cause issues
    -   Possibility for users to cause harm to the system
    -   Trends seen in past issues

  

### Coverage

  

-   **What does the test surface look like?** Is it a simple library with one method, or a multi-platform client-server stateful system with a combinatorial explosion of use cases? Describe the design and architecture of the system in a way that highlights possible points of failure.
-   **What platforms are supported?** Consider listing supported operating systems, hardware, devices, etc. Also describe how testing will be performed and reported for each platform.
-   **What are the features?** Consider making a summary list of all features and describe how certain categories of features will be tested.
-   **What will not be tested?** No test suite covers every possibility. It’s best to be up-front about this and provide rationale for not testing certain cases. Examples: low risk areas that are a low priority, complex cases that are a low priority, areas covered by other teams, features not ready for testing, etc. 
-   **What is covered by unit (small), integration (medium), and system (large) tests?** Always test as much as possible in smaller tests, leaving fewer cases for larger tests. Describe how certain categories of test cases are best tested by each test size and provide rationale.
-   **What will be tested manually vs. automated?** When feasible and cost-effective, automation is usually best. Many projects can automate all testing. However, there may be good reasons to choose manual testing. Describe the types of cases that will be tested manually and provide rationale.
-   **How are you covering each test category?** Consider:
    -   [accessibility](http://www.w3.org/wiki/Accessibility_testing)
    -   [functional](http://en.wikipedia.org/wiki/Functional_testing)
    -   [fuzz](http://en.wikipedia.org/wiki/Fuzz_testing)
    -   internationalization and localization
    -   [performance](http://en.wikipedia.org/wiki/Software_performance_testing), [load](http://en.wikipedia.org/wiki/Load_testing), [stress](http://en.wikipedia.org/wiki/Stress_testing), and [endurance](https://en.wikipedia.org/wiki/Soak_testing) (aka soak)
    -   privacy
    -   [security](http://en.wikipedia.org/wiki/Security_testing)
    -   [smoke](http://en.wikipedia.org/wiki/Smoke_testing_(software))
    -   [stability](http://en.wikipedia.org/wiki/Stability_testing)
    -   [usability](http://en.wikipedia.org/wiki/Usability_testing)
-   **Will you use static and/or dynamic analysis tools?** Both [static analysis tools](https://en.wikipedia.org/wiki/Static_program_analysis) and [dynamic analysis tools](https://en.wikipedia.org/wiki/Dynamic_program_analysis) can find problems that are hard to catch in reviews and testing, so consider using them.
-   **How will system components and dependencies be stubbed, mocked, faked, staged, or used normally during testing?** There are good reasons to do each of these, and they each have a unique impact on coverage.
-   **What builds are your tests running against?** Are tests running against a build from HEAD (aka tip), a staged build, and/or a release candidate? If only from HEAD, how will you test release build cherry picks (selection of individual changelists for a release) and system configuration changes not normally seen by builds from HEAD?
-   **What kind of testing will be done outside of your team?** Examples:
    -   [Dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food)
    -   External crowdsource testing
    -   Public alpha/beta versions (how will they be tested before releasing?)
    -   External trusted testers
-   **How are data migrations tested?** You may need special testing to compare before and after migration results.
-   **Do you need to be concerned with backward compatibility?** You may own previously distributed clients or there may be other systems that depend on your system’s protocol, configuration, features, and behavior.
-   **Do you need to test upgrade scenarios for server/client/device software or dependencies/platforms/APIs that the software utilizes?**
-   **Do you have line coverage goals?**

  

### Tooling and Infrastructure

  

-   **Do you need new test frameworks?** If so, describe these or add design links in the plan.
-   **Do you need a new test lab setup?** If so, describe these or add design links in the plan.
-   **If your project offers a service to other projects, are you providing test tools to those users?** Consider providing mocks, fakes, and/or reliable staged servers for users trying to test their integration with your system.
-   **For end-to-end testing, how will test infrastructure, systems under test, and other dependencies be managed?** How will they be deployed? How will persistence be set-up/torn-down? How will you handle required migrations from one datacenter to another?
-   **Do you need tools to help debug system or test failures?** You may be able to use existing tools, or you may need to develop new ones.

  

### Process

  

-   **Are there test schedule requirements?** What time commitments have been made, which tests will be in place (or test feedback provided) by what dates? Are some tests important to deliver before others?
-   **How are builds and tests run continuously?** Most small tests will be run by [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) tools, but large tests may need a different approach. Alternatively, you may opt for running large tests as-needed. 
-   **How will build and test results be reported and monitored?**
    -   Do you have a team rotation to monitor continuous integration?
    -   Large tests might require monitoring by someone with expertise.
    -   Do you need a dashboard for test results and other project health indicators?
    -   Who will get email alerts and how?
    -   Will the person monitoring tests simply use verbal communication to the team?
-   **How are tests used when releasing?**
    -   Are they run explicitly against the release candidate, or does the release process depend only on continuous test results? 
    -   If system components and dependencies are released independently, are tests run for each type of release? 
    -   Will a "release blocker" bug stop the release manager(s) from actually releasing? Is there an agreement on what are the release blocking criteria?
    -   When performing canary releases (aka % rollouts), how will progress be monitored and tested?
-   **How will external users report bugs?** Consider feedback links or other similar tools to collect and cluster reports.
-   **How does bug triage work?** Consider labels or categories for bugs in order for them to land in a triage bucket. Also make sure the teams responsible for filing and or creating the bug report template are aware of this. Are you using one bug tracker or do you need to setup some automatic or manual import routine?
-   **Do you have a policy for submitting new tests before closing bugs that could have been caught?**
-   **How are tests used for unsubmitted changes?** If anyone can run all tests against any experimental build (a good thing), consider providing a howto.
-   **How can team members create and/or debug tests?** Consider providing a howto.

  

### Utility

  

-   **Who are the test plan readers?** Some test plans are only read by a few people, while others are read by many. At a minimum, you should consider getting a review from all stakeholders (project managers, tech leads, feature owners). When writing the plan, be sure to understand the expected readers, provide them with enough background to understand the plan, and answer all questions you think they will have - even if your answer is that you don’t have an answer yet. Also consider adding contacts for the test plan, so any reader can get more information.
-   **How can readers review the actual test cases?** Manual cases might be in a test case management tool, in a separate document, or included in the test plan. Consider providing links to directories containing automated test cases.
-   **Do you need traceability between requirements, features, and tests?**
-   **Do you have any general product health or quality goals and how will you measure success?** Consider:
    -   Release cadence
    -   Number of bugs caught by users in production
    -   Number of bugs caught in release testing
    -   Number of open bugs over time
    -   Code coverage
    -   Cost of manual testing
    -   Difficulty of creating new tests






___
# References
[Google Testing Blog: The Inquiry Method for Test Planning](https://testing.googleblog.com/2016/06/the-inquiry-method-for-test-planning.html)