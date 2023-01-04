222022-11-29

Style: 

Domain:

Tags:

# Provide data reliability in Amazon Redshift at scale using Great Expectations library

## Use case overview

Before performing analytics or building machine learning (ML) models, cleaning data can take up a lot of time in the project cycle. Without automated and systematic data quality checks, we may spend most of our time cleaning data and hand-coding one-off quality checks. As most data engineers and scientists know, this process can be both tedious and error-prone.

Having an automated quality check system is critical to project efficiency and data integrity. Such systems help us understand data quality expectations and the business rules behind them, know what to expect in our data analysis, and make communicating the data’s intricacies much easier. For example, in a raw dataset of customer profiles of a business, if there’s a column for date of birth in format YYYY-mm-dd, values like `1000-09-01` would be correctly parsed as a date type. However, logically this value would be incorrect in 2021, because the age of the person would be 1021 years, which is impossible.

Another use case could be to use GE for streaming analytics, where you can use [AWS Database Migration Service](https://aws.amazon.com/dms/) (AWS DMS) to migrate a relational database management system. AWS DMS can export change data capture (CDC) files in Parquet format to Amazon S3, where these files can then be cleansed by an [AWS Glue](https://aws.amazon.com/glue) job using GE and written to either a destination bucket for Athena consumption or the rows can be streamed in AVRO format to [Amazon Kinesis](https://aws.amazon.com/kinesis/) or Kafka.

Additionally, automated data quality checks can be versioned and also bring benefit in the form of optimal data monitoring and reduced human intervention. Data lineage in an automated data quality system can also indicate at which stage in the data pipeline the errors were introduced, which can help inform improvements in upstream systems.

## Solution architecture

This post comes with a ready-to-use blueprint that automatically provisions the necessary infrastructure and spins up a SageMaker notebook that walks you step by step through the solution. Additionally, it enforces the best practices in data DevOps and infrastructure as code. The following diagram illustrates the solution architecture.

[![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2021/11/12/BDB-1526-image001-new.jpg)](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2021/11/12/BDB-1526-image001-new.jpg)
The architecture contains the following components:

1.  **Data lake** – When we run the [AWS CloudFormation](http://aws.amazon.com/cloudformation) stack, an open-source sample dataset in CSV format is copied to an S3 bucket in your account. As an output of the solution, the data destination is an S3 bucket. This destination consists of two separate prefixes, each of which contains files in Parquet format, to distinguish between accepted and rejected data.
2.  **DynamoDB** – The CloudFormation stack persists data quality expectations in a DynamoDB table. Four predefined column expectations are populated by the stack in a table called `redshift-ge-dq-dynamo-blog-rules`. Apart from the pre-populated rules, you can add any rule from the Great Expectations [glossary](https://legacy.docs.greatexpectations.io/en/latest/reference/glossary_of_expectations.html) according to the data model showcased later in the post.
3.  **Data quality processing** – The solution utilizes a SageMaker notebook instance powered by Amazon EMR to process the sample dataset using PySpark (v3.1.1) and Great Expectations (v0.13.4). The notebook is automatically populated with the S3 bucket location and Amazon Redshift cluster identifier via the SageMaker lifecycle config provisioned by AWS CloudFormation.
4.  **Amazon Redshift** – We create internal and external tables in Amazon Redshift for the accepted and rejected datasets produced from processing the sample dataset. The external `dq_rejected.monster_com_rejected`table, for rejected data, uses [Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum.html) and creates an external database in the AWS Glue Data Catalog to reference the table. The `dq_accepted.monster_com` table is created as a regular Amazon Redshift table by using the COPY command.

### Run configurations to create a PySpark context

When the notebook is open, make sure the kernel is set to Sparkmagic (PySpark). Run the following block to set up Spark configs for a Spark context.

[![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2021/11/11/BDB-1526-image004.png)](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2021/11/11/BDB-1526-image004.png)

### Create a Great Expectations context

In Great Expectations, your data context manages your project configuration. We create a data context for our solution by passing our S3 bucket location. The S3 bucket’s name, created by the stack, should already be populated within the cell block. Run the following block to create a context:

```python
from great_expectations.data_context.types.base import DataContextConfig,DatasourceConfig,S3StoreBackendDefaults
from great_expectations.data_context import BaseDataContext

bucket_prefix = "ge-redshift-data-quality-blog"
bucket_name = "ge-redshift-data-quality-blog-region-account_id"
region_name = '-'.join(bucket_name.replace(bucket_prefix,'').split('-')[1:4])
dataset_path=f"s3://{bucket_name}/monster_com-job_sample.csv"
project_config = DataContextConfig(
    config_version=2,
    plugins_directory=None,
    config_variables_file_path=None,
    datasources={
        "my_spark_datasource": {
            "data_asset_type": {
                "class_name": "SparkDFDataset",//Setting dataset type to Spark
                "module_name": "great_expectations.dataset",
            },
            "spark_config": dict(spark.sparkContext.getConf().getAll()) //Passing Spark Session configs,
            "class_name": "SparkDFDatasource",
            "module_name": "great_expectations.datasource"
        }
    },
    store_backend_defaults=S3StoreBackendDefaults(default_bucket_name=bucket_name)//
)
context = BaseDataContext(project_config=project_config)
```

## Benefits and limitations

The efficiency and efficacy of this approach is delineated from the fact that GE enables automation and configurability to an extensive degree when compared with other approaches. A very brute force alternative to this could be writing stored procedures in Amazon Redshift that can perform data quality checks on staging tables before data is loaded into main tables. However, this approach might not be scalable because you can’t persist repeatable rules for different columns, as persisted here in DynamoDB, in stored procedures (or call DynamoDB APIs), and would have to write and store a rule for each column of every table. Furthermore, to accept or reject a row based on a single rule requires complex SQL statements that may result in longer durations for data quality checks or even more compute power, which can also incur extra costs. With GE, a data quality rule is generic, repeatable, and scalable across different datasets.

Another benefit of this approach, related to using GE, is that it supports multiple Python-based backends, including Spark, Pandas, and Dask. This provides flexibility across an organization where teams might have skills in different frameworks. If a data scientist prefers using Pandas to write their ML pipeline feature quality test, then a data engineer using PySpark can use the same code base to extend those tests due to the consistency of GE across backends.

Furthermore, GE is written natively in Python, which means it’s a good option for engineers and scientists who are more used to running their extract, transform, and load (ETL) workloads in PySpark in comparison to frameworks like [Deequ](https://github.com/awslabs/deequ), which is natively written in Scala over Apache Spark and fits better for Scala use cases (the Python interface, PyDeequ, is also available). Another benefit of using GE is the ability to run [multi-column](https://legacy.docs.greatexpectations.io/en/latest/reference/glossary_of_expectations.html%22%20/l%20%22multi-column) unit tests on data, whereas Deequ doesn’t support that (as of this writing).

However, the approach described in this post might not be the most performant in some cases for full table load batch reads for very large tables. This is due to the _serde_ (serialization/deserialization) cost of using UDFs. Because the GE functions are embedded in PySpark UDFs, the performance of these functions is slower than native Spark functions. Therefore, this approach gives the best performance when integrated with incremental data processing workflows, for example using AWS DMS to write CDC files from a source database to Amazon S3.

___
# References
[Provide data reliability in Amazon Redshift at scale using Great Expectations library | AWS Big Data Blog](https://aws.amazon.com/blogs/big-data/provide-data-reliability-in-amazon-redshift-at-scale-using-great-expectations-library/)