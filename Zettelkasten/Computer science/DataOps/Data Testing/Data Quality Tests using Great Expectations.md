222022-10-18

Style: 

Domain:

Tags:

# Data Quality Tests using [[Great Expectations]]

It doesn’t matter how fast data lands into your spreadsheets or dashboards, if it is not correct, then it is useless. Moreover, it could cause bad decisions to take place and could lead to irreversible repercussions. A robust data quality tool is integral to any data workload to prevent catastrophes. In this article, I’ll take you through how I’ve used Great Expectations with Pyspark to perform tests through data transformations.

## Handling Data Quality

While PySpark does its job as an efficient transformation tool, the ultimate goal of Data Engineering is not just to transform data from its raw form to a consumable form but to ensure that the end product meets the expected quality standards. The data should align with the business rules agreed upon by subject matter experts.

Below are some examples of things we ask about data:

-   Is the column mandatory?
-   Are these values valid?
-   Is the format correct?
-   How do we determine that an account is active during a specific period?
-   If the column is numeric in format, is it expected to be within a certain range?

The answers to these questions are translated to business rules and are validated against the data.

## The Goal

1.  To explore Kickstarter Campaign dataset downloaded from [Kaggle](https://www.kaggle.com/kerneler/starter-kickstarter-campaigns-9f7d98d1-9/data).
2.  Produce a metric that counts the number of successful campaigns per defined categories per assessment year
3.  To use Great Expectations to execute unit and integration tests

## The How

1.  Dataset goes through several layers of transformation from raw form to the final metric output
![[Pasted image 20221018094704.png]]

![[Pasted image 20221018094711.png]]

You can find the Project code here:
[pyspark_greatexpectations/Great Expectations - SPARK DataFrame.ipynb at main · karenbajador/pyspark_greatexpectations · GitHub](https://github.com/karenbajador/pyspark_greatexpectations/blob/main/notebooks/Great%20Expectations%20-%20%20SPARK%20DataFrame.ipynb)




___
# References
[Data Quality Unit Tests in PySpark Using Great Expectations | by Karen Bajador Valencia | Towards Data Science](https://towardsdatascience.com/data-quality-unit-tests-in-pyspark-using-great-expectations-e2e2c0a2c102)