222022-10-10

Style: #ArticleNotes 

Domain: #DataOps 

Tags: #TestAutomation 

# The challenge of testing Data Pipelines
### How the Oracle Problem, quality thresholds, walled gardens, multi-dimensional quality, and the very purpose of tests makes the challenge of testing data pipelines different from testing traditional software.

First, a bit of context on data pipelines: I’m using the term broadly to describe a wide range of systems, including both ETL and ELT processes and all types of data lake, data mart, and data warehouse architectures. It doesn’t matter if data is processed in batches, micro-batch, or streamed — everything here still applies.

This brings us to the first big difference between data pipeline testing and testing standard applications: Data Pipelines often have “tests” built into the data pipeline itself, executed as data is processed, to ensure data quality. These tests are part of the production functionality of the pipeline. They are usually defined by data stewards or data engineers, and ensure that bad data is identified, then blocked, scrubbed, fixed, or just logged as the pipeline is run. These tests are necessary because data that flows into data pipelines is often from untrusted systems and of low quality. These tests are **not** the type of test we normally think about when using the term in the context of traditional software, and the term “test” is often a source of confusion.

It often helps to visualize data pipelines as two, orthogonal paths. The horizontal path (blue arrows) is the flow of data through the data pipeline. The vertical path (orange) is the promotion of code and logic across environments in the CI/CD pipeline, as new code or features are developed. Data flows horizontally across each of these environments, and code or other pipeline changes flow vertically between environments. Data quality tests, aka runtime tests, are executed in the horizontal flow, to validate data quality, and transformation validation or other normal tests are executed in the vertical flow, to validate code an logic before promotion to the next environment. They are both tests, but test different things for different reasons.

![[Pasted image 20221010103912.png]]

The confusion between runtime data quality tests and development code quality tests is exacerbated by the fact that the two types of tests can be written in the same framework, and a single test could find both a data quality issue as well as a code logic issue, depending on how it is being used. In addition, runtime data quality tests are themselves functionality, so should be tested by logic tests.

**Key takeaway**: Be clear about what your tests are testing and ensure that you are sufficiently testing both data quality and logic. Find and use precise terms for each that have meaning for your team.

# GUI Tools and Walled Gardens
**Key takeaway:** Understand the limitations of your tools. If your organization is moving away from legacy ETL development to modern data engineering, adopt code-first tools (like [dbt](http://getdbt.com/)) over GUI-first, walled garden type tools, regardless of how nice the UI looks.

# The (many) Dimensions of Quality

Here is a list I’ve used that makes sense to me. Again, exactly how you enumerate and define these dimensions is a personal preference, and if expressed too loudly in the wrong part of the office, can get you into unnecessary arguments.

**Completeness:** When whole records within the data set or values within a record that should be present are missing (think null columns in a DB row).

**Uniqueness**: When data is duplicated. Duplicate or redundant data is common in pipelines that pull from a variety of upstream sources, and deduplication is often a big component of data pipeline processing.

**Validity:** For data to be valid, it needs to be presented in a format that is consistent within the domain of values. For example, in a date attribute that assumes MM-DD-YY format, any date in a YYYY-MM-DD format would be invalid, even if the date is actually accurate.

**Consistency:** When data that is expected to be equivalent is not. For example, the customer record from billing says Bob’s address is 123 Main St, but the account record from CMS says its 489 James St. This is a contrived example, and there are more subtle reasons why data could be considered inconsistent based on context and domain.

**Referential Integrity**: When data refers to other data through foreign keys or other less explicit relationships, but that data is missing. This ties into the concept of Master Data Management (MDM).

**Accuracy:** Data values that are just incorrect. It might have been pulled from the wrong source, been corrupted, or simply entered incorrectly in the source system.

**Compliance:** Data might be all the above things, but be out of compliance. Doesn’t seem that important? Now you have a clear text credit card number flowing through your data pipeline, getting written into log files, etc. Yikes.

**Timeliness:** When availability or latency issues prevent data from being valuable for its intended use. Depending on the use case and the data, timeliness expectations can vary widely from minutes to infinite.


# The Oracle Problem
in many cases determining the “expected” result can be a challenge — this is the Oracle Problem. You log into the test environment for AcmeTravel.com. You search for a flight from Seattle to NYC, and get 302 results. Were you supposed to get 302 results? Were these the exact 302 results you were supposed to get? Without deep hooks into the underlying data sources, or full control of the integration points of this search system, this is probably impossible to answer. Again, this is the Oracle Problem: every test is a comparison of two results, an expected and an actual, and determining the expected result is often a challenge.

Data pipelines have a bad case of the Oracle Problem. A data pipeline pulls data from many source systems. It combines this data, transforms it, scrubs it, then stores it in some processed state for consumption by downstream systems, analytics platforms, or data scientists. These pipelines often process data from hundreds or even thousands of sources. If you run your pipeline, get several million new rows in your consumption layer, how do you know if it succeeded? How many rows were you supposed to get? Is every value in each column of each row correct? This problem is just as bad, if not worse, than the above flight search problem!

While this can sometimes be done, the specific nature of data pipelines can make it challenging or even impossible. Often source data is made up of unstructured data (think: blog posts, customer reports, log files, etc.) not rows in a relational database, and it is often challenging to derive representative mock data from unstructured data. Even if all source data was nicely structured database tables, these tables often number in the thousands, and have complex relationships between them. Attempting to set up even a single, consistent set of test data could be incredibly time consuming. Another challenge: this source data is often quite varied (The third “V” of big data), such that you cannot predict beforehand all the permutations that you should account for in your test data. The strategy of mocking, so useful in testing normal software systems, is often impotent in data pipelines.

**Key takeaway**: Think early about how you will obtain expected results for tests — both data quality and transformation logic — as this might be your biggest challenge. For data quality validation, leverage data stewards or other data science experts to develop statistical techniques for identifying data issues.


# “Good Enough” Data — Data Quality Thresholds

This “good enough for the purpose” mentality can be jarring for the seasoned quality engineer coming from traditional quality engineering domains, but is something that will need to be understood to be successful in data pipelines.

**Key takeaway:** Know your data quality expectations and invest in quality assurance accordingly.



___
# References
[The challenge of testing Data Pipelines | by Blake Norrish | Slalom Build | Medium](https://medium.com/slalom-build/the-challenge-of-testing-data-pipelines-4450744a84f1)