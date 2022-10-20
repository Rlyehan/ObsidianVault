222022-10-18

Style: 

Domain:

Tags:

# Does [[great expectations]] cover our needs


## Unit tests or expectations translated directly into documentation

We know how powerful and convenient documentation is but nobody wants to maintain separate documentation just to ensure it is synced with the latest code. I prefer an out-of-the-box feature that generates documentation out of unit tests defined.

## Add extra information about the unit test

I want to be able to elaborate on what is exactly being tested instead of generic descriptions to make it comprehensible enough for anyone who checks the data quality reports.

According to the [style guide](https://docs.greatexpectations.io/docs/contributing/style_guides/code_style), it is recommended to use google style docstrings.

## Documentation can be hosted in S3

AWS S3 is a convenient location to host static websites and since we already use S3, might as well use the same to deploy the shared documentation.

By default, docs are stored locally but can also be [deployed in a cloud blob storage](https://docs.greatexpectations.io/docs/tutorials/getting_started/customize_your_deployment/#options-for-hosting-data-docs) including S3.

## Ready-to-use common data quality test functions

It would greatly expedite the refactoring process if there are already [native unit test functions](https://greatexpectations.io/expectations/) that we can leverage to avoid having to write dynamic tests from scratch.

## Support most of the data quality checks we already do

I’ve searched for available [expectations](https://greatexpectations.io/expectations/) that potentially match our most common data quality checks at least semantically.

## Support for multiple Pandas/Spark DataFrame and Snowflake Data source

It should work for both our current and future environment with minimal effort to refactor.

Data sources supported: [Pandas DataFrame](https://docs.greatexpectations.io/docs/guides/connecting_to_your_data/in_memory/pandas), [Spark DataFrame](https://docs.greatexpectations.io/docs/guides/connecting_to_your_data/in_memory/spark), and [Snowflake via SQLAlchemy](https://docs.greatexpectations.io/docs/guides/connecting_to_your_data/database/snowflake)

## Extensible

Even though it supports most of our data quality expectations out of the box, it should be flexible enough to be able to [write custom tests](https://docs.greatexpectations.io/docs/guides/expectations/creating_custom_expectations/how_to_create_custom_expectations) for those that it doesn’t natively support.

## Integration with orchestration tools like Airflow or DBT

It should support integration with the existing orchestration platforms in our data ecosystem.

[Validations](https://docs.greatexpectations.io/docs/deployment_patterns/how_to_run_a_checkpoint_in_airflow) can be run via [Airflow](https://airflow.apache.org/) through BashOperator or PythonOperator. There is also an [extension package](https://github.com/calogica/dbt-expectations) for [DBT](https://www.getdbt.com/) as well.
___
# References
[Great Expectations: The Data Testing Tool – Is This the Answer to Our Data Quality Needs? | by Karen Bajador Valencia | Towards Data Science](https://towardsdatascience.com/great-expectations-the-data-testing-tool-is-this-the-answer-to-our-data-quality-needs-f6d07e63f485)