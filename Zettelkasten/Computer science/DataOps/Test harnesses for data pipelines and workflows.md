232023-01-04

Style: 

Domain:

Tags:

# Test harnesses for data pipelines and workflows

In my [prior post](https://ayc-data.com/data_engineering/2020/07/25/dataops-the-foundation-of-data.html) about DataOps, I covered using ‘**Statistical Process Controls**’ to monitor data infrastructure, simliar to physical manufacturing production lines. In particular, I talked about automated checkpoints as a quality control method so data irregularity is picked up and automated alerts are sent.

Whether it be data science workflows (e.g. machine learning pipelines) or data integration or data processing pipelines (e.g. ELT), once these production systems become very critical and large, it becomes very difficult (practically impossible) to monitor their status and quality without automated testing.

Building on from this, I’m going to explore how to do this automated testing and the different types of testing for data workflows and pipelines. The two types I will explore are:

1.  **Unit tests** - code to test code
    
2.  **Data quality and data pipeline tests** - code to test data and workflows

# Why is Data Quality is so hard to test

Data pipeline, like any other software, you can also build a **test harness for automated data pipeline/quality tests**. A **test harness** is essentially a framework that allows you to do automated tests by automating executing the test scripts against the code or data and comparing the results to the desired value.

We covered off some of the challenges in data pipeline testing in the [prior post](https://ayc-data.com/data_engineering/2020/07/25/dataops-the-foundation-of-data.html), but to recap, the main reasons are:

1.  Data that comes into data pipelines and workflows is dynamic and unknown, so even if the code for the data pipeline **doesn’t change**, it could still break.
    
2.  Data changes over time, so edge cases that used to be rare may become common. For example if only 5% of data was dirty and now it is 40%, extra steps need to be taken to deal with the 40% now.
    
3.  Data schema and structure changes over time, and static assumptions on what the data looks like are no longer valid
    
4.  ‘Flow’ is very important in data workflows and pipelines, as they are generally directed acyclic graphs, which is series of steps that must be executed in a particular order. **Order is very important**.


# Recap on Unit Tests and Best Practices

Before we continue, we should revisit the tried and test (excuse the pun) aspect of software engineering - **unit tests**. Ask any software engineer about unit testing and they will say it is a crucial part of software development. It’s been around for a while and there’s many good frameworks for it - for example JUnit for Java or Pytest for Python.

At it’s core, a **test case** is a set of inputs with corresponding expected outputs, while a group of test cases is a **test suite**. When a test is executed, a **test oracle** then checks whether the expected output matches the actual output of the test case - if so, it passes. A good framework will handle the automatic execution of tests in a test suite, as well as the automatic comparison between expected vs actual output.

The expected output is generally by way of an **assertion** - that is:

1.  Expecting a **return value or exception as output**
2.  Expecting a **state change** - e.g. whether a variable changed after applying a method
3.  Expecting whether a **specific method was invoked**

Unit tests will generally cover **3 types of inputs**:

1.  **Bad inputs** - e.g. string when expecting integer input
2.  **Special inputs** - e.g. boundary/edges cases or when not using default arguments
3.  **Normal inputs** - ‘vanilla’ scenarios

The following are some key characteristics and good tips on how to write good unit tests.

## Unit tests is testing in units

The concept of unit has many facets:

1.  Each test is **isolated** - can be run in parallel, before or after other unit tests. Most unit test frameworks can and will run unit tests in any order and can and will run in parallel. Tests should avoid sharing resources and not interfere with each other, nor have any order dependency.
    
2.  Each test is **independent** and **modular**- has minimal dependency, including external resources and system functions (e.g. clocks). All factors that make input or output unclear or contingent should be removed and **random input values should not be used**.
    
3.  Each test should have its own variables - where functions or modules have **global states and variables**, be wary what sets or alters these during testing. Furthermore, **don’t share mutable states between tests**.
    
4.  Each test is **deterministic** - that is, it has fixed inputs that produces a known fixed output. The input should never change for every run and the output should always be the same.
    
5.  Each test focuses on **one function** - do not have unit tests that also test a function’s dependencies, or compounding errors may occur.
    
6.  Each test should have a **binary outcome** - pass or fail. There is no ‘partial’ fail, or this means you are testing more than one thing. If a function has multiple potential inputs and outputs, you could end up with **more tests than functions** (e.g. 5 tests for 1 function). For each unit test, share setups in fixtures, rather than testing 5 things in the one test. For example, for a moveFile function, asserting there is a file, the file is named ‘open.txt’ and the file has been deleted from the source system) should be in three tests, not one.

## Unit Tests should have consistent outcomes and not create any lasting effects

Each test **should not leave permanent effects** - do not access real network, file system, cloud or database resources. These resources should be mocked or monkey patched during runtime. The next section will discuss mocking resources and data.

**It is important a unit test outcome is consistent and correct** - a unit test that **doesn’t fail when it’s supposed to is dangerous**. Whenever you write a bug fix, you should write a failing test to make sure it does its job. A good way to do this is randomly comment out code and see if tests still pass. If they do, you have untested code!

In some instances, unit tests end up being **‘flaky’**, which is a situation where sometimes tests pass and sometimes they fail. This makes them inconsistent and not reliable, as you cannot say with 100% confidence all the tests have passed. Main causes for this inconsistency include:

1.  An input into the module/function is **randomised or time-dependent**(e.g. running it at 12am vs 12pm)
    
2.  **Multithreading** - race conditions cause some parts to be executed out of order
    
3.  **Assumptions about the underlying system** - e.g. Linux vs Windows operating system libraries
    
4.  **Hardware-level inconsistencies** - e.g. FLOATs being implemented across different hardware with different precision While unit testing is white box testing, you **should avoid tightly coupled costs**, as the tighter it’s coupled with the function, the more blind spots there are.

## Use mock resources and data to control your inputs

There are different ways you can introduce ‘**mock**’ resources and data into unit tests. For complex functions that are internal and are dependent on the outcome of another component, you should **mock that component which always returns a fixed value**.

For example, creating a mock alarm object so its constructor will always deem the isAlarm flag to ‘Y’. Otherwise, if you relied on the real Alarm object, it may require multiple other dependencies, such as checking external sensors, detectors and validating these inputs before returning a Y.

There are different types of ‘mock’ objects including:

1.  **Stub**: Doubles, unlike real objects, are 100% configurable (to code looks like the same thing, but its customisable). Stubs contain nothing except what you put in there - i.e. you make it hardcode to return certain values.

```python
unittest.Mock
Stub_sensor = Mock(sensor)

Stub_sensor.get_reading.return_value = 15
```

1.  **Dummy** - act like a ‘placeholder’ and usually is an empty implementation.
    
2.  **Fake**: similar to stub, but it has real implementation inside it. e.g. replacing File with StringIO or normal database with in-mem database. These are often used just to make tests run faster. Not suitable for production.
    
3.  **Spy** - a special type of stub which makes assertions about the test case. That is, a spy may cause test to fail if it’s not called correctly. Spies records the method calls it receives (e.g. can’t construct unless you call is_valid first).

An example of mocking AWS resources is via moto, an open-source library that creates a virtual and fake AWS account:

```python
# main.py - Your actual function you want to test
import boto3

def s3_put(data, bucket, key):
  response = boto3.resource('s3').put_object(
    Body=data,
    Bucket=bucket,
    Key=key
  )

  return response

# test_main.py - in your test file, you would have this
import pytest
import boto3
from moto import mock_s3

@pytest.fixture(autouse=True, scope='module')
@mock_s3
def setup_s3():
  '''Returns mock bucket name'''
  bucket_name = 'TEST_BUCKET'
  region = 'ap-southeast-2'

  boto3.resource('s3', region_name=region).create_bucket(
    Bucket=bucket_name,
    CreateBucketConfiguration={'LocationConstraint': region} # To avoid IllegalConstraint issue. Moto defaults to us-east-1
  )

  return bucket_name

@mock_s3
class TestS3:
  def test_s3_put(self):

    # Create mock resources
    bucket_name = setup_s3()
    data = 'Letmein'.encode()

    # Run function
    s3_put(data, bucket_name, 'test.txt')

    # Assert the file exists
    response = boto3.client('s3').get_object(
      Bucket=bucket_name,
      Key='test.txt'
    )['Body']
    assert response is not None 
```

Another way of controlling inputs and dependencies is **monkey patching**, which is dynamically change attribute of object at runtime. An example is monkey patching the HTTP API call function (e.g. requests in Python) so it will always return a dummy payload. That way, this avoid the dependency on a real URL/API which will cause the unit test to fail (where the code itself has no issues).

Another example is monkey patching the datetime.now() object so it returns a fixed value for ‘today’. This is useful if you have a data validation check to see whether today’s data was ingested.

Two common examples of monkey patching:

**Controlling the Response from an API by mocking it**

```python
# Controlling the Response from an API

# main.py - Function to be tested
def api_caller(url):
  '''
  Hits API and returns value
  :param url : str

  :return response : str
  '''
  response = requests.get(url)

  return response.text

# test_main.py - Pytest test
def test_api_caller(monkeypatch)
  """
  GIVEN a monkeypatched version of requests
  WHEN requests response is set to a valid response
  THEN check api_caller works
  """

  # First monkeypatch
  class MockResponse(object):
    def __init__(self):
      self.status_code = 200
      self.url = 'http://testurl.com'
      self.text = json.dumps({'name': 'bob', 'age': 15})

  def mock_get(self, url):
    return MockResponse()

  monkeypatch.setattr(requests, 'get', mock_get)

  # Execute function as required
  url = 'http://testurl.com'
  response = api_caller(url)

  # Assert response is correct
  assert response.status_code == 200
```

**Controlling datetime to always return a specified time**

```python
# Controlling the time

# main.py - Data Quality test to be checked
def validate_today(df):
  '''
  Validate whether data has today's date

  :param df : DataFrame
  :return boolean - True if validated, False if failed validation
  '''
  today = datetime.datetime.now().date()

  return df[['date'] == today]

# test_main.py - Pytest test
def test_api_caller(monkeypatch)
  """
  GIVEN a monkeypatched version of datetime.now()
  WHEN datetime.now always return fixed time
  THEN check validation function works
  """

  today = '2020-10-10 10:00:00'
  # First monkeypatch
  class MockResponse(object):
    def __init__(self):
      self.status_code = 200
      self.url = 'http://testurl.com'
      self.text = json.dumps({'name': 'bob', 'age': 15})

  def mock_now(self):
    return today

  monkeypatch.setattr(datetime.datetime, 'now', mock_now)

  # Execute function as required
  df = DataFrame({'date': today})

  response = validate_today(df)

  # Assert response is correct
  assert response == True
```

Furthermore, unit tests allow you test whether a function is invoked in the right circumstances and conditions. This is a great way to test whether the function is behaving as you intended.

In particular, for branching scenarios (e.g. Only sent alarm if distress beacon on), it’s good to test whether the distress beacon was actually sent. This is important in the context of data automation tests (discussed below), as you need to be confident that when alarm thresholds are met, automated responses are done.

An example is below:

```python
# Ensuring a function is invoked when the conditions are met

# Main.py
def execute_pipeline(alarm:boolean):
	if alarm == True:
		send_distress_beacon(topic_name='TEST')
	
def send_distress_beacon(topic_name:str):
	# send beacon
	return True
	

#test_main.py
from unittest.mock import patch
import main

# Run function
with patch("main.send_distress_beacon") as patched_sent:
  main.execute_pipeline(alarm=True)

# Assertion SNS message sent
patched_sent.assert_called()
```

## Unit Testing in the context of big data infrastructure

As discussed above, one of the core principles of unit testing is ensuring the inputs and dependencies are **controlled and fixed**. This allows you to consistently test in a controlled environment whether your code is functionally properly.

However, in the context of big data, static test inputs should not be hardcoded. As the whole point of data pipeline/infrastructure is to deal with data, if the data changes, the code and the tests should change along with it.

Therefore, test inputs should be generated for each test run on-the-fly using pre-defined rules. These libraries allow you to generate large test inputs with high variance, meaning you just need to maintain data generation scripts, rather than hardcoded static data. [Python Faker](https://faker.readthedocs.io/en/master/) is a good example of this type of library.

Python Faker, for example, automatically generates 10,000+ fake customer addresses that you can use to test your data pipelines relating to customer data.

Even if you’re not using a test data generator, you should as a best practice rotate your test data (e.g. don’t always use the same date or file for every test). Using more variety in test data input helps identify edge cases and bugs.

# Pipeline/Data Quality Tests

In data pipelines, ‘**pipeline and data quality tests**’ for data is like unit tests for code. There are various types of testing in big data, but generally, tests are more focused on the data, not the function/code base. Unit tests generally test with _static pre-defined data_, while pipeline tests generally test with _dynamic unseen data_.

Importantly, **_you should have unit tests that test whether the pipeline tests are functioning correctly._**

Testing static data (via unit tests) on data quality tests to ensure they are operating correctly means that when the data quality/pipeline test runs against live (dynamic) data, you have confidence the test is performing properly.

This [medium article](https://medium.com/@expectgreatdata/down-with-pipeline-debt-introducing-great-expectations-862ddc46782a) by the creators of the open-source library described below (Great Expectations) discusses this very well.

For example, the open-source Python test framework [Great Expectations](https://docs.greatexpectations.io/en/latest/intro.html#what-is-great-expectations) is excellent for this. It allows you to run automated tests against data in the pipeline (both before and after the run) to test.

# Data quality as regression tests

Because data pipelines and workflows are so dependent on good data coming in and good data coming out, a great way to do **regression testing** on data infrastructure is on the data quality itself.

**Regression testing** is a concept which tests whether alterations to the code (or in this case, data too) cause features or data quality to go bad. That is, whether any changes have caused features to ‘regress’ or go backwards.

This may involve automated data quality monitoring on the final output of a data pipeline/workflow, such as row count and sum checks on the final FACT tables. As an example from my work - we need to construct pricing and demand curves every night using a complex engine that involves thousands of lines of SQL/Python, mixing both rules-based and ML techniques. This data workflow takes over 6 hours to execute every night, generating a fact table that is over 8 billion rows large.

Therefore ‘spot checking’ is not adequate, as a 1% variance would equate to 80 million rows.

Therefore, we have set up automated data quality checks on the final fact tables to check whether:

1.  every customer has a demand curve that goes up to today’s date
    
2.  every curve does not have gaps/missing data points
    
3.  the size of the fact table does not double - if so, this usually indicates duplicate source data or there’s a bug in the pipeline
    

These checks give us peace of mind in ensuring that any new changes we make to the engine do not cause the data pipeline/workflow to ‘regress’.

An extension to this automated data quality check is **outlier/anomaly detection** - using ML-based techniques, you can create models that check whether there are any outliers in data. A simple example would be if there was a sudden spike of 100,000x in your demand curve. This would not be picked up by ‘gaps’ checks, but should be picked up in any anomaly detection. These spikes are generally attributed to incorrect source data (e.g. typos making 10,000m into 10,000km).

___
# References
[Automated testing - in context of big data pipelines and workflows](https://ayc-data.com/data_engineering/2020/09/30/testing-for-big-data-infrastructure.html)