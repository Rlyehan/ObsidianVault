222022-08-12

Style: 

Domain: #TestAutomation 

Tags: #Data

# Data Validation Tests

Data validation tests ensure that the data present in final target systems are valid, accurate, as per business requirements and good for use in the live production system.

### What is Data Validation?
In simple terms, Data Validation is the act of validating the fact that the data that are moved as part of [[ETL]] or data migration jobs are consistent, accurate, and complete in the target production live systems to serve the business requirements.

### Data Validation for ETL
Since we apply transformations to the source data based on certain requirements, we need to validate that the data loaded into the target system is complete and has no discrepancies.

### Data Validation for Data Migration
In data migration projects we want to move our data from one system to another. Here we must validate that all the data has arrieved in the target system and that it did so correctly.

### Data Mapping
If we want to do any data validation, we want to first start with mapping the data. This could be an excel sheet or some visual representation.


# Data Validation tests

### Data uniformity
Data uniformity tests are conducted to verify that the actual value of the entity has the exact match at different places.

### Entity presence
Here we want to verify that all entities(tables, columns) that we expect are present. We can do this in different ways.

First we could verify that all entities that should be present in the source and their correspondig entites int the target are present. This is simple if the same names are used throughout the system.

If the names of entities can change depending on were they are in the system, we make us of the data mapping to creat an entity mapping for this kind of comparison.

we also could make use of the data mapping information to test that entities that should not be present, are in fact not.

### Data Accruacy
Here we check for logical accuracy. we can do this is many different ways, and what we need to check depends on the data we want to test.

some examples:
- verify that entries are in a list of possible values
- verify that entries fullfill format criteria (like valid email, etc.)
- verify that referential values are correct
- etc.#

### Metadata validation
In Metadata validation, we validate that the Table and Column data type definitions for the target are correctly designed, and once designed they are executed as per the data model design specifications.

### referential integrity
Here, we mainly validate the integrity constraints like Foreign key, Primary key reference, Unique, Default, etc.

### Data completeness
These are sanity tests that uncover missing record or row counts between source and target table and can be run frequently once automated.

We can do this for example by simply comparing the record counts, or even have reconciliation checks to go a step further. We could also check by comparing the rows containing null values.

Another option for large sets of records could be to check if the column profiling matches.

### Data Transformation
These tests form the core tests of the project. Review the requirements document to understand the transformation requirements. Prepare test data in the source systems to reflect different transformation scenarios. These have a multitude of tests and should be covered in detail under ETL testing topics.

**Below is a concise list of tests covered under this:**

**(i) Transformation:**

-   **Example:** ETL code might have logic to reject invalid data. Verify these against requirements.
-   ETL code might also contain logic to auto-generate certain keys like surrogate keys. We need to have tests to verify the correctness (technical and logical) of these.
-   Validate the correctness of joining or split of field values post an ETL or Migration job is done.
-   Have tests to verify referential integrity checks. **For example,** a type of defect could be ProductId used in Orders table is not present in parent table Products. Have a test to verify how the orphan records behave during an ETL job.
-   At times, missing data is inserted using the ETL code. Verify the correctness of these.
-   ETL or Migration scripts sometimes have logic to correct data. Verify data correction works.
-   Verify if invalid/rejected/errored out data is reported to users.
-   Create a spreadsheet of scenarios of input data and expected results and validate these with the business customer.

**(ii) Edge cases:** Verify that Transformation logic holds good at the boundaries.

-   **Example:** What happens when TotalSales with a value of 1 Trillion is run through an ETL job? Does the end to end cases work? Identify fields that can potentially have large values and run tests with these large values. They should include numerical and non-numerical values.
-   For date fields, including the entire range of dates expected – leap years, 28/29 days for February. 30, 31 days for other months.

### Data uniqueness
In this type of test, identify columns that should have unique values as per the data model. Also, take into consideration, business logic to weed out such data. Run tests to verify if they are unique in the system. Next run tests to identify the actual duplicates.

### Aggregate functions
Aggregate functions are built in the functionality of the database. Document all aggregates in the source system and verify if aggregate usage gives the same values in the target system sum, max, min, count.

Quite often the tools on the source system are different from the target system. Check if both tools execute aggregate functions in the same way


___
# References
