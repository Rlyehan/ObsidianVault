222022-09-08

Style: 

Domain: #TestAutomation 

Tags:

# Code Coverage
One of the areas that we have consistently advocated for is the use of code coverage data to assess risk and identify gaps in testing. However, the value of code coverage is a highly debated subject with strong opinions, and a surprisingly polarizing topic. Every time code coverage is mentioned in any large group of people, seemingly endless arguments ensue.

We put forth best practices in the domain of code coverage to work effectively with code health.

-   **Code coverage provides significant benefits to the developer workflow**. It is not a perfect measure of test quality, but it does offer a reasonable, objective, industry standard metric with actionable data. It does not require significant human interaction, it applies universally to all products, and there are ample tools available in the industry for most languages. You must treat it with the understanding that it’s a lossy and indirect metric that compresses a lot of information into a single number so it should not be your only source of truth.  Instead, use it in conjunction with other techniques to create a more holistic assessment of your testing efforts.

-  It is an open research question whether code coverage alone reduces defects, but our experience shows that efforts in increasing code coverage can often lead to culture changes in engineering excellence that in the long run reduce defects. For example, teams that give code coverage priority tend to treat testing as a first class citizen, and tend to bake stronger testability into their product design, so that they can achieve their testing goals with less effort. All this in turn leads to writing higher quality code to begin with (more modular, cleaner contracts in their APIs, more manageable code reviews, etc.). They also start caring more about their overall health, and engineering and operational excellence.

-   **It is an open research question whether code coverage alone reduces defects**, but our experience shows that efforts in increasing code coverage can often lead to culture changes in engineering excellence that in the long run reduce defects. For example, teams that give code coverage priority tend to treat testing as a first class citizen, and tend to bake stronger testability into their product design, so that they can achieve their testing goals with less effort. All this in turn leads to writing higher quality code to begin with (more modular, cleaner contracts in their APIs, more manageable code reviews, etc.). They also start caring more about their overall health, and engineering and operational excellence.

-   **A high code coverage percentage does not guarantee high quality in the test coverage**. Focusing on getting the number as close as possible to 100% leads to a false sense of security. It could also be wasteful, burning machine cycles and creating technical debt from low-value tests that now need to be maintained. Bad code being pushed to production due to missing tests could happen either because (a) your tests did not cover a specific path of code, a test gap that is easy to identify with code coverage analysis, or (b) because your tests did not cover a specific edge case in an area that did have code coverage, which is difficult or impossible to catch with code coverage analysis. Code coverage does not guarantee that the covered lines or branches have been tested _correctly_, it just guarantees that they have been executed by a test. Be mindful of copy/pasting tests just for the sake of increasing coverage, or adding tests with little actual value, to comply with the number. A better technique to assess whether you’re adequately exercising the lines your tests cover, and adequately asserting on failures, is [[Mutation Testing]].

-   **But a low code coverage number does guarantee that large areas of the product are going completely untested** by automation on every single deployment. This increases our risk of pushing bad code to production, so it should receive attention. _In fact a lot of the value of code coverage data is to highlight not what’s covered, but what’s not covered._

-   **There is no “ideal code coverage number” that universally applies to all products**. The level of testing you want/need for a set of code should be a function of (a) business impact/criticality of the code; (b) how often you will need to touch/change the code; (c) how much longer you expect the code to live, its complexity, and domain variables. We cannot mandate every single team should have x% code coverage; this is a business decision best made by the owners of the product with domain-specific knowledge. Any mandate to reach x% code coverage should be accompanied by infrastructure investments to make testing easy, such as integrating tools into the developer workflow. Be mindful that engineers may start treating your target like a checkbox and avoid increasing coverage beyond the target, even if doing so would be prudent.

-   **In general code coverage of a lot of products is below the bar; we should aim at significantly improving code coverage across the board**. Although there is no “ideal code coverage number,” at Google we offer the general guidelines of 60% as “acceptable”, 75% as “commendable” and 90% as “exemplary.” However we like to stay away from broad top-down mandates and encourage every team to select the value that makes sense for their business needs.

-   **We should not be obsessing on how to get from 90% code coverage to 95%**. The gains of increasing code coverage beyond a certain point are logarithmic. But we should be taking concrete steps to get from 30% to 70% and always making sure new code meets our desired threshold.

-   **More important than the percentage of lines covered is human judgment over the actual lines of code (and behaviors)  that aren’t being covered(analyzing the gaps in testing)** and whether this risk is acceptable or not. What’s not covered is more meaningful than what is covered. Pragmatic discussions over specific lines of code not covered that take place during the code review process are more valuable than over-indexing on an arbitrary target number. We have found out that embedding code coverage into your code review process makes code reviews faster and easier. Not all code is equally important, for example testing debug log lines is often not as important, so when developers can see not just the coverage number, but each covered line highlighted as part of the code review, they will make sure that the most important code is covered.


-   **Just because your product has low code coverage doesn’t mean you can’t take concrete, incremental steps to improve it over time**. Inheriting a legacy system with poor testing and poor testability can be daunting, and you may not feel empowered to turn it around, or even know where to start. But at the very least, you can adopt the ‘boy-scout rule’ (leave the campground cleaner than you found it). Over time, and incrementally, you will get to a healthy location.

-   **Make sure that frequently changing code is covered**. While project wide goals above 90% are most likely not worth it, per-commit coverage goals of 99% are reasonable, and 90% is a good lower threshold. We need to ensure that our tests are not getting worse over time.

-   **Unit test code coverage is only a piece of the puzzle**. Integration/System test code coverage is important too. And the aggregate view of the coverage of all sources in your Pipeline (unit and integration) is paramount, as it gives you the bigger picture of how much of your code is not exercised by your test automation as it makes its way in your pipeline to a production environment. One thing you should be aware of is while unit tests have high correlation between executed and evaluated code, some of the coverage from integration tests and end-to-end tests is incidental and not deliberate. But incorporating code coverage from integration tests can help you avoid situations where you have a false sense of security that even though you’re not covering code in your unit tests, you think you’re covering it in your integration tests.

-   **We should gate deployments that do not meet our code coverage standards.** Teams should debate and decide which gating mechanism makes sense to them. You should however be careful that it doesn’t turn into being treated as a checkbox that is required to be filled, as it can backfire (pressure to 'hit the metric' almost never yields the desired outcome). There are many mechanisms available:  gate on coverage for all code vs gate on coverage to new code only; gate on a specific hard-coded code coverage number vs gate on delta from prior version, specific parts of the code to ignore or focus on. And then, commit to upholding these as a team. Drops in code coverage violating the gate should prevent the code from being checked in and reaching production.


Beyond that, it is imoprtant to keep in mind that there are different forms of code coverage we could/should analyze:

-   **Function coverage** – has each function  in the program been called?
-   **Statement coverage** – has each  in the program been executed?
-   **Edge coverage** – has everin the [control-flow graph](https://en.wikipedia.org/wiki/Control-flow_graph "Control-flow graph") been executed?
    -   **Branch coverage** – has each branch (also called the) of each control structure been executed? For example, given an _if_ statement, have both the _true_ and _false_ branches been executed?
-   **Condition coverage** – has each Boolean sub-expression evaluated both to true and false?


### Code coverage at Google
-   It demonstrates that it is possible to implement a code cov- erage infrastructure based on existing, well-established li- braries, seamlessly integrate it into the development work- flow, and scale it to an industry-scale code base.
-   It details Google’s infrastructure for continuous code cover- age computation and how code coverage is visualized.
-   It details adoption rates, error rates, and average code cover- age ratios over a five-year period.
-   It reports on the perceived usefulness of code coverage, ana- lyzing 512 responses from surveyed developers at Google.

Once the developer is satisfied with the contents of a changelist, they issue a mail command which initiates the code review process. At this point, automated analyses includ- ing code coverage are started and a notification is emailed to the reviewers. The changelist is now said to be “in review”. Through the review process the contents may change, leading to different snapshots. When the reviewers and the author approve of a change- list, the author issues a submit command. In the example above, this command first merges CL 2 with the current submitted state of the codebase (CL 4) and, if the merge is successful, runs more automated tests. Only if the merge is successful and all tests pass is the changelist submitted to the codebase.

Be- cause Google enforces style guides for all major languages, line coverage strongly correlates with statement coverage in Google’s codebase.

For C++, Java, and Python, Google’s code coverage infrastruc- ture also measures branch coverage, which is the percentage of branches of conditional statements (e.g., if statements) executed by a set of tests. Branch coverage is equivalent to edge coverage for all conditional edges in the control flow graph.

This paper considers only code coverage of tests that execute on a single machine, usually in a single process. At Google, these are colloquially referred to as unit tests. This paper does not consider code coverage of more complex (i.e., integration or system) tests that span multiple machines. Since integration tests focus on inte- gration points between (sub)systems and not the internals of each (sub)system, line coverage is not the best suited coverage measure for these tests.




___
# References
[Google Testing Blog: Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html)
[https://storage.googleapis.com/pub-tools-public-publication-data/pdf/36f4140541f8dd555fb8aaee2fd719d59ffab041.pdf](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/36f4140541f8dd555fb8aaee2fd719d59ffab041.pdf)
