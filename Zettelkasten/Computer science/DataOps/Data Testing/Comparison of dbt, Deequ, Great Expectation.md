222022-10-13

Style: 

Domain:

Tags:

# Comparison of dbt, Deequ, Great Expectation

### Great Expectation
Great Expectations allows you to define expectations in a JSON file or inline with your code.

Great Expectations has backend support for Pandas, Spark, and SQLAlchemy. You can see which expectations are available for your choice of backend [here](https://greatexpectations.io/expectations/).

If you don’t meet your Expectations in the list of pre-defined Expectations, you can always [create your own](https://docs.greatexpectations.io/docs/guides/expectations/creating_custom_expectations/how_to_create_custom_column_map_expectations).

Whether feeding data into a [SageMaker model](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2021/11/12/BDB-1526-image001-new.jpg), a [feature validation engine](https://docs.feast.dev/tutorials/validating-historical-features), a set of reports on top of a [Snowflake data warehouse](https://medium.com/snowflake/how-to-ensure-data-quality-with-great-expectations-271e3ca8b4b9), an [ML workflow engine](https://blog.flyte.org/data-quality-enforcement-using-great-expectations-and-flyte), a [data orchestrator like Airflow](https://www.astronomer.io/blog/adding-data-quality-to-dags-ft-great-expectations/), a data lake on Databricks, or even a traditional [transactional database](https://medium.com/hashmapinc/understanding-great-expectations-and-how-to-use-it-7754c78962f4), or a [non-traditional transactional database](https://www.dolthub.com/blog/2021-06-15-great-expectations-plus-dolt/), the only reasonable way to use Great Expectations is to run it as part of your data or CI pipeline.

### dbt
In [this blog post](https://www.getdbt.com/blog/data-testing-framework/), dbt presents a good analogy of a full-scale data pipeline and the different check posts where your pipeline needs testing, data quality checks, and validation. Ideally, such a check post should exist [in your data pipeline where data is being ingested, pushed out, or transformed](https://medium.com/@josh.temple/automated-testing-in-the-modern-data-warehouse-d5a251a866af). Using [dbt utils](https://github.com/dbt-labs/dbt-utils#generic-tests), you can perform both structural and data tests for your models and transformations. You can [start here](https://servian.dev/unit-testing-in-dbt-part-1-d0cc20fd189a) with a post by my colleague where she talks about setting up basic tests and data quality checks using dbt.

dbt offers a deeper level of testing with very useful predefined tests for data warehouse-modeling techniques like Dimensional modeling and Data Vault 2.0. Tests and data quality checks involving aggregates, case statements, table comparisons, and summaries are extremely useful in the transformation layers of the data pipeline. A few examples of what you’d want to check in a hypothetical scenario where you have a data lake in S3, a staging layer, and a data warehouse in Snowflake are as follows:

-   When moving data from S3 to the [Snowflake staging layer](https://docs.snowflake.com/en/user-guide/data-load-considerations-stage.html), you should perform basic data quality checks like [checking for NULLs](https://docs.getdbt.com/reference/resource-properties/tests/#not_null), column formats, data types, etc.
-   When moving data from the [Snowflake staging layer to DataVault](https://danlinstedt.com/allposts/datavaultcat/q-whats-the-best-way-to-test-a-data-vault/comment-page-1/), you should also check for column counts and other aggregates for loose integrity checks.
-   When moving data from the Snowflake DataVault to a dimensional schema, you should be checking again for column counts in the source and the target. The more complex individual, row-level tests are performed with the help of introspective macros, which allow you to iterate over the result of a query.

dbt provides [several helper libraries](https://github.com/dbt-labs/dbt-audit-helper#compare_relations-source) that you can use to achieve this testing and data quality functionality.


### Deequ
Suppose you’re working with Spark in any way or form with EMR, [Glue](https://aws.amazon.com/blogs/big-data/testing-data-quality-at-scale-with-pydeequ/), [Databricks](https://databricks.com/notebooks/streaming-data-quality.html), Jupyter notebooks, etc. In that case, you’d have heard of the Spark-native [library for unit testing](https://www.velotio.com/engineering-blog/test-data-quality-at-scale-using-deequ-and-apache-spark) and measuring data quality called [Deequ](https://github.com/awslabs/deequ). This utility comes from AWS Labs.

Deequ lets you profile and validate data using Analyzers. In addition to the profile summaries, Deequ also helps you with field-level data validation. Based on the profiles of your data and some heuristic rules, Deequ’s [Constraint Suggestion functionality](https://github.com/awslabs/deequ/blob/master/src/main/scala/com/amazon/deequ/examples/constraint_suggestion_example.md) suggests what constraints you should be running in your Verification suite to enforce a quality check on your data.

_For a more detailed explanation of the data validation and verification process, and the philosophy behind the design choices and types of constraints, read_ [_this paper_](https://www.vldb.org/pvldb/vol11/p1781-schelter.pdf) _which contains research done at_ [_Amazon_](https://www.amazon.science/publications/automating-large-scale-data-quality-verification) _for making Deequ possible._

In addition to the suggested checks, there are a [host of checks](https://github.com/awslabs/python-deequ/blob/master/tests/test_checks.py) available that you can add to the [Constraint Verification phase](https://www.aloneguid.uk/posts/2021/02/aws-deequ-any-constraint/). You can write your checks too.

a simple example with pyDeequ:

```python
import pyspark

import pydeequ

from pydeequ.checks import *

from pydeequ.verification import *

spark = (SparkSession

.builder

.config("spark.driver.extraClassPath", classpath)

.getOrCreate())

survey_df = spark.read.parquet("s3a://kovid-rathee/pydeequ/survey_data/")

survey_df_check = Check(spark, CheckLevel.Warning, "Checking Survey Data")

survey_df_check_result = VerificationSuite(spark) \

.onData(survey_df) \

.addCheck(

check.hasMin("age", lambda x: x == 18) \

.hasMax("age", lambda x: x == 30) \

.containsEmail("email") \

.isContainedIn("state", ["VIC", "NSW", "ACT", "QLD"]) \

.isUnique("email") \

.isNonNegative("age")) \

.run()

survey_df_check_result_df = VerificationResult.checkResultsAsDataFrame(spark, survey_df_check_result)

survey_df_check_result.show()
```

Once the constraint verification process completes, PyDeequ will write a summary report on a path of your choosing; in this case, we’re printing the results DataFrame, `survey_df_check_result`, to the console. It helps to see if your fixes to correct data somewhere in your data pipeline or at the source have worked or not. PyDeequ also allows you to define a Metrics Repository, which can help you track the quality of your data over time.

### Conclusion
Deequ provides is a great way to work with your Spark code, whether written in Scala, Python, or SparkSQL. As long as Deequ receives data in a DataFrame (or a DynamicFrame, in the case of AWS Glue), it will be fine. Note how the three tools we’ve discussed have approached the same problem with different techniques. That’s happened because of two main reasons — the processes that these tools support are different, and the types of tests those processes need are different. dbt, as they sell it, is for transformations, Great Expectations is a general-purpose data quality suite, and Deequ is for large-scale data quality checks when you’re underlying infrastructure is on Spark.


___
# References
[Data Quality and Testing Frameworks | by Kovid Rathee | Servian](https://servian.dev/data-quality-and-testing-frameworks-316c09436ab2)