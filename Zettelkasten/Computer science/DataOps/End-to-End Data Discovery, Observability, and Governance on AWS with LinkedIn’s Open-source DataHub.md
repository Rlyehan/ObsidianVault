222022-08-31

Style: 

Domain: #DataOps 

Tags:

# End-to-End Data Discovery, Observability, and Governance on AWS with LinkedIn’s Open-source DataHub

Lets start with defining a data catalogue:
“_a collection of metadata, combined with data management and search tools, that helps analysts and other data users to find the data that they need, serves as an inventory of available data, and provides information to evaluate the fitness of data for intended uses._”

DataHub is an open-source tool that provides us with the capability to create data catalogues for our system. it can integrate with most of the commonly used data sources across the public cloud providers.


# Data Catalog Features
DataHub secribes itself as:
“_a modern data catalog built to enable end-to-end data discovery, data observability, and data governance._”

some of its features are:
-   Metadata ingestion
-   Data discovery
-   Data governance
-   Data observability
-   Data lineage
-   Data dictionary
-   Data classification
-   Usage/popularity statistics
-   Sensitive data handling
-   Data fitness (aka data quality or data profiling)
-   Manage both technical and business metadata
-   Business glossary
-   Tagging
-   Natively supported datasource integrations
-   Advanced metadata search
-   Fine-grain authentication and authorization
-   UI- and API-based interaction

# Datasources

When considering a data catalog solution, in my experience, the most common datasources that customers want to discover, inventory, and search include:

-   Relational databases and other OLTP datasources such as PostgreSQL, MySQL, Microsoft SQL Server, and Oracle
-   Cloud Data Warehouses and other OLAP datasources such as Amazon Redshift, Snowflake, and Google BigQuery
-   NoSQL datasources such as MongoDB, MongoDB Atlas, and Azure Cosmos DB
-   Persistent event-streaming platforms such as Apache Kafka (Amazon MSK and Confluent)
-   Distributed storage datasets (e.g., Data Lakes) such as Amazon S3, Apache Hive, and AWS Glue Data Catalogs
-   Business Intelligence (BI), dashboards, and data visualization sources such as Looker, Tableau, and Microsoft Power BI
-   ETL sources, such as Apache Spark, Apache Airflow, Apache NiFi, and dbt

# DataHub on AWS

DataHub’s convenient [AWS setup guide](https://datahubproject.io/docs/deploy/aws/) covers options to deploy DataHub to AWS. For this post, I have hosted DataHub on Kubernetes, using Amazon Elastic Kubernetes Service (Amazon EKS). Alternately, you could choose [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine) on Google Cloud or [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/) on Microsoft Azure.

Conveniently, DataHub offers a [Helm chart](https://datahubproject.io/docs/deploy/kubernetes/), making deployment to Kubernetes straightforward. Furthermore, Helm charts are easily integrated with popular CI/CD tools. For this post, I’ve used [ArgoCD](https://argoproj.github.io/cd/), the declarative GitOps continuous delivery tool for Kubernetes, to deploy the [DataHub Helm chart](https://artifacthub.io/packages/helm/datahub/datahub).

# Secure Metadata Ingestion

Often, datasources are not externally accessible for security reasons. Further, many datasources may not be accessible to individual users, especially in higher environments like UAT, Staging, and Production. They are only accessible to applications or CI/CD tooling. To overcome these limitations when extracting metadata with DataHub, I prefer to perform my DataHub-related development and testing locally but execute all DataHub ingestion securely on AWS.

I use my local development environment with my favorite IDE, JetBrains PyCharm, to author the Python and YAML-based DataHub configuration files and ingestion pipeline [recipes](https://datahubproject.io/docs/metadata-ingestion), then commit those files to git and push to private GitHub repositories. Finally, I use GitHub Actions to test DataHub files.

To run DataHub ingestion jobs and push the results to DataHub running in Kubernetes on Amazon EKS, I have built a custom [Python-based Docker container](https://hub.docker.com/_/python). The container runs the DataHub CLI, required DataHub plugins, and any additional Python dependencies. The container’s pod has the appropriate AWS IAM permissions, using [IAM Roles for Service Accounts (IRSA)](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html), to securely access datasources to ingest and the DataHub application.

Scheduling and managing multiple metadata ingestion jobs on AWS is best handled with Apache Airflow with Amazon Managed Workflows for Apache Airflow (Amazon MWAA). Ingestion jobs run as Airflow DAG tasks, which call the EKS-based DataHub CLI container. With MWAA, datasource connections, credentials, and other sensitive configurations can be kept secure and not be exposed externally or in plain text.

When running the ingestion pipelines on AWS with DataHub, all communications between AWS-based datasources, ingestion jobs running in Airflow, and DataHub, should use secure private IP addresses and DNS resolution instead of transferring metadata over the Internet. Make sure to create all the necessary [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) connections, network route table configurations, and [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) to connect all relevant services.

SaaS services such as Snowflake or MongoDB Atlas, services provided by other Cloud Service Providers such as Google Cloud and Microsoft Azure, and datasources in corporate datasources require alternate networking and security strategies to access metadata securely.

![AWS-based DataHub high-level architecture](https://pocket-image-cache.com//filters:format(jpg):extract_focal()/https%3A%2F%2Fmiro.medium.com%2Fmax%2F1400%2F1*bmh56j9psddQHDzJslhRBg.png)

AWS-based DataHub high-level architecture

## Using Python

You can configure and run a pipeline entirely from within a [custom Python script](https://datahubproject.io/docs/metadata-ingestion/#using-as-a-library) using DataHub’s Python API as an alternative to YAML. Below, we see two nearly identical ingestion recipes to the YAML above, written in Python. Writing ingestion pipeline logic programmatically gives you increased flexibility for automation, error checking, unit-testing, and notification. Below is a basic pipeline written in Python. The code is functional, but not very Pythonic, secure, scalable, or Production ready.

The second version of the same pipeline is more Production ready. The code is more Pythonic in nature and makes use of error checking, logging, and the [AWS Systems Manager (SSM) Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html). Like recipes written in YAML, environment variables can be used for convenience and security. In this example, commonly resued and sensitive connection configuration items have been extracted and placed in the SSM Parameter Store. Additional configuration is pulled from the environment, such as AWS Account ID and AWS Region. The script loads these values at runtime.

# Sinking to DataHub

When syncing metadata to DataHub, you have two choices, the [GMS REST API](https://datahubproject.io/docs/metadata-ingestion/sink_docs/datahub#datahub-rest) or [Kafka](https://datahubproject.io/docs/metadata-ingestion/sink_docs/datahub#datahub-kafka). According to DataHub, the advantage of the REST-based interface is that any errors can immediately be reported. On the other hand, the advantage of the Kafka-based interface is that it is asynchronous and can handle higher throughput. For this post, I am DataHub’s REST API.

# Column-level Metadata

In addition to column names and data types, it is possible to extract column descriptions and key types from certain datasources. Column descriptions, tags, and glossary terms can also be input through the DataHub UI.

# Business Glossary

DataHub can assign business glossary terms to entities. The [DataHub Business Glossary](https://datahubproject.io/docs/metadata-ingestion/source_docs/business_glossary/) plugin pulls business glossary metadata from a YAML-based configuration file.

# SQL-based Profiler

DataHub also can extract statistics about entities in DataHub using the . According to the DataHub documentation, the Profiler can extract:

-   Row and column counts for each table
-   Column null counts and proportions
-   Column distinct counts and proportions
-   Column min, max, mean, median, standard deviation, quantile values
-   Column histograms or frequencies of unique values

In addition, we can also track the historical stats for each profiled entity each time metadata is ingested.

# Data Lineage

DataHub’s data lineage features allow us to view upstream and downstream relationships between different types of entities. DataHub can trace lineage across multiple platforms, datasets, pipelines, charts, and dashboards.


Additional capabilities, including [GraphQL](https://datahubproject.io/docs/api/graphql/overview) and [Timeline](https://datahubproject.io/docs/dev-guides/timeline) APIs, robust [authentication and authorization](https://datahubproject.io/docs/how/auth/jaas), application [monitoring observability](https://datahubproject.io/docs/advanced/monitoring), and [Great Expectations](https://datahubproject.io/docs/metadata-ingestion/integration_docs/great-expectations/) integration. All these qualities make DataHub an excellent choice for a data catalog.
___
# References
[End-to-End Data Discovery, Observability, and Governance on AWS with LinkedIn’s Open-source DataHub | by Gary A. Stafford | Medium](https://garystafford.medium.com/end-to-end-data-discovery-observability-and-governance-on-aws-with-linkedins-datahub-8c69e7e8c925)