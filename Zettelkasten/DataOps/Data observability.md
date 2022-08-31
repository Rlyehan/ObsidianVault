222022-08-29

Style: #Research 

Domain: #DataOps

Tags: #Data 

# Data observability
in modern data scenarios we often ingest data from external sources and have little to no corntrol over any potential changes made to it.

And as the demand to get more data, faster, has grown, so have tools that help automate the ETL/ELT process to get the data to the analysts faster. but this also comes with the risk of lower data quality since data engineers have less influence over what exactly happens and how the data gets delivered.

Add to that that a one-size fits all pipeline seldomly is what we actually want, so in the end we have to go back to costume data pipelines anyways.

### what is data observability?
It is a banket term used for understanding the health and the state of data in our system. It is what is already known in software development as [[Observability]], but cotumized for our data needs.

It is the core of what has become known as DataOps, the data equivalent to DevOps.

### DataOps
DataOps is an agile workflow consisiting of a delivery pipeline and feedback loop, similar to waht we see in DevOps. It helps bridge the gap between data analysts and data engineers, two roles that have different goals and responsiblities by tying them together with the feedback loop.

The DataOps cycle consists of three distinct phases:
- Detection
- Awarness
- Iteration

![[DataOps Cycle.png]]


![[data cycle.png]]


### Detection
We start the cycle with the detection phase. Beeing able to monitor the data quality continuoulsly and find potential data drift is key for the end user to be able to trust the data that we delivered.

This first step is what we call validation focused. Here we execute the data quality checks that we already have in place, like column schema checks or row level validations.

It is important to understand that these kinds of checks are reactive in nature, and can as such only detect that something went wrong based on their underlying rules. They can not detect what went wrong beyond potential failure paths we have defined. The main problem with this is when we run into issues we have not been aware of before, often leading to band aid fixes.

### Awareness
Awareness is visiblity focused. Here we talk about data governance and make a meta-data centrict approach. By centralizing and standardizing pipeline & dataset metadata across the orginization gives us a full picture.

This centralized system gives us awareness of the end-to-end health of our data and makes finding the root cause of data issues easier. This way we can track "bad data" to where it was introduced into our system and as such fix the problems.

But this section is also a big challange, as it would need good communication and a shared understanding of what stakeholders need and intend to do.

### Iteration
Here we want to introduce repeatable and sustainable standards that will be applied to all of our data development.

### Data Observabilities make-up

- Monitoring - a dashboard that provides an operational view of your pipeline or system
- Alerting - both for expected events and anomalies
- Tracking - ability to set and track specific events
- Comparisons - monitoring over time, with alerts for anomalies
- Analysis - automated issue detection that adapts to your pipeline and data health
- Logging - a record of an event in an standardized format for faster resolution
- SLA tracking - the ability to measure data quality and pipeline metadata against pre-defined standards

Another set of priciples:
-   **Freshness**: Freshness seeks to understand how up-to-date your data tables are, as well as the cadence at which your tables are updated. Freshness is particularly important when it comes to decision making; after all, stale data is basically synonymous with wasted time and money.
-   **Distribution**: Distribution, in other words, a function of your data’s possible values, tells you if your data is within an accepted range. Data distribution gives you insight into whether or not your tables can be trusted based on what can be expected from your data.
-   **Volume:** Volume refers to the completeness of your data tables and offers insights on the health of your data sources. If 200 million rows suddenly turns into 5 million, you should know.
-   **Schema**: Changes in the organization of your data, in other words, schema, often indicates broken data. Monitoring who makes changes to these tables and when is foundational to understanding the health of your data ecosystem.
-   **Lineage**: When data breaks, the first question is always “where?” Data lineage provides the answer by telling you which upstream sources and downstream ingestors were impacted, as well as which teams are generating the data and who is accessing it. Good lineage also collects information about the data (also referred to as metadata) that speaks to governance, business, and technical guidelines associated with specific data tables, serving as a single source of truth for all consumers.

To make most of that and acheive proper observability we need to introduce these standards across all teams and make sure that we collaborate.

![[Pasted image 20220829133357.png]]


#### Dataset Monitoring
- Did the data arrive on time?
- Is the data beeing updates as often as we need it to?
- Is the expected volume of data availalbe on this dataset?

#### Operational Monitoring
Here we monitore the state of our pipelines, like:

- How does pipeline perfomance impact the dataset quality?
- Under what conditions is a run considered succesful?
- What operations are transforming the dataset before it reaches its target?

while the tasks of dataset and operational monitoring are seperated, we need to couple them to gain a solid foundation for observability.

#### Column-Level profiling

Column level profiling builds upon the solid foundation we built before to enforce existing business rules and establish new ones at the column level.

- What is the expected range for a column?
- What is the expected schema for this column?
- How unique is this column?

#### Row-level validation
The final level of observability is row-level validaton. here we look at the values in each row and validate them.
- Are the values in each row in the expected form?
- Are the values the length you expect?
- Given the context, is there enough information for the end user?

This is the last level of our observability stack and as such should be built on top of a solid foundation.

### Implementing a data observability framework

Data Observability is based on DataOps, and as any such framework, company wide adoption and getting the stakeholders on board is the key to implement data observability. After all even the best reports are worthless if nobody uses them.

We need:
1. DataOps culture
2. Standardized Data Platform
3. Unified Data Observability Platform

Once we have established a DataOps culture we can implement an standardized data platform. That means a platform with end-to-end ownership and accountability across all teams. For that we need shared infrastructure and enable teams to speak the same language and openly communicate about issues. That means we need standarized libraries for APIs and data managment and standardized libraries for data quality. We also need version control, data versioning, and CI/CD processes.

After that we want to make use of a unified observability platform taht gives us open access to our systems health. This platform would serve as a centralized metadata repository. It would encompass all the features mentioned 

![[Pasted image 20220829155451.png]]


### Metrics we can track for data quality
- Null count
- Schema changes
- Data lineage
- Pipeline failures
- Pipeline durations
- Missing data operations
- Record count in a run
- Tasks read from a dataset
- Data freshness




___
# References
[What is Data Observability? Hint: it’s not just data for DevOps. | Towards Data Science](https://towardsdatascience.com/what-is-data-observability-40b337971e3e)