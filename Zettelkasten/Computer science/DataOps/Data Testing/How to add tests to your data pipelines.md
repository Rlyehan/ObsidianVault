222022-09-01

Style: #ArticleNotes

Domain: #Data 

Tags:

# How to add tests to your data pipelines
It's important to be aware that testing a data pipeline is a bit different from testing most applications. We have two overarching types:
- Testing for data quality
- Standard tests

## Testing your data pipeline

If you have a data pipeline with no tests in place, we need to define a starting point covering the key areas to create test for the first. This could look somethin glike this:
1. End to End system testing
2. Data quality testing
3. Monitoring
4. Unit and contract/integration testing

Note that this approach is for already existing pipelines to get tests in place fast. It is also under the assumption that the pipeline in question is in what appears to be a working state. This is also just a general approach, it can change on your specific use case.

### End-to-end system testing
We can do this simply by giving the system an sample input, leting our pipeline execute, and comparing the output with what we would expect.

Example:
```python
import pytest

from your.data_pipeline_path import run_your_datapipeline


class TestYourDataPipeline:
    @pytest.fixtures(scope="class", autouse=True)
    def input_data_fixture(self):
        # get input fixture data ready
        yield
        self.tear_down()

    def test_data_pipeline_success(self):
        run_your_datapipeline()
        result = {"some data or file"}
        expected_result = "predefined expected data or file"
        assert result == expected_result

    def tear_down(self):
        # remove input fixture data
```

### Data quality tesing
Here we test the quality of our data instead of testing the code. We are going to inspect the transformed data and will compare its characterisitcs to our expectations.

An example of how we could do this:
1. We load the transformed data into a staging table/dataset
2. Verify that this staging data falls within exeoctations, usually ew check for null count, uniquenes constraints, allowed values, business rules, outliers, etc.

It is also important to keep in mind that we will run these tests each time the pipeline gets executed, compared to our code tests taht are only executed until the pieline goes to production.

In the first step we might only start validating the final dataset, but we could, and often should, add extra cehcks after each transformation so long as the performance requirments allow for it.

### Monitoring
Data quality tests are the first step we need to ensure we only propagate good data, but we can only create so many tests. For any unexpected issue that might crop up, we want to have a robust monitoring system in place to help us out. 

There are many different services that we can use of, like datadog, but they do require us to make sure we set them up for success, by making the relevant traces, logs and other forms of data available to them.

### unit and contract testing
Unit tests should be self explanatory as to why we would want them. Contract tests can serve as a great form of integration tests for external services.


___
# References
[How to add tests to your data pipelines · Start Data Engineering](https://www.startdataengineering.com/post/how-to-add-tests-to-your-data-pipeline/)