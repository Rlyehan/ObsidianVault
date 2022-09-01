222022-09-01

Style: 

Domain: #MLOps

Tags:

# Testing features with pytest
If we want to move our models to operations, we need to have tests in place for our features.

### Introduction

MLOps has become a widely adopted idea in the industry, but the adoption is still missing some key points that are needed to fill in the gaps for development and operational processes.

Since data still needs to be transformed into an approrpiate numerical format, and embeddings are becoming an increasingly popular way to compress high dimensional data, both things python excels at, we fill in part of the MLOps lifecycle with guidelines and examples of how you can test your feature logic and your feature pipeline with pytest.


![[Pasted image 20220901144121.png]]


### Unit tests for feature logic
Here we want to validate that the code to compute our featrues is correct and adhers to specification. These unit tests should guarantee invariants, preconditions and postconditions for the feature logic we wrote.

### Refactor features into functions
To do the uni testing we want to have oru features as functions. Many data scientists use notebooks to create their code. And while taht itself is no longer as much of a problem as before, the structure of our code generally is not fit for testing.

As such we need to make sure to encapsualte the different features in their own functions to allow for unit testing.

### pytest

**[[Pytest]] is built on 3 main concepts:** test functions, assertions, and test setup. In pytest, unit tests may be written either as functions or as methods in classes. Pytest has a naming convention to automatically discover test modules/classes/functions. A test class must be named “Test*”, and test functions or methods must be named “test_*”.

In figure 2, we can see that pytest is run [**during development**](https://realpython.com/pytest-python-testing/) as offline tests, not when feature pipelines have been deployed to production (online tests).

![](https://miro.medium.com/max/1400/0*Fi7lDfWv5_aEV4XG.png)


### A testing methodology

You should always make sure to follow a strucutred approach, like Arange, Act, Assert, when writing your tests. But taht only helps with how to test, not what to test.

What to test depends on your scenario, but in general you should be able to identify the most important parts and make sure to test them more thorougly.

A good general approach is:
1. test the common code paths end-to-end
2. write uni tests for the untested code paths

But this apporach won't be able to catch all teh bugs, you will still encounter them. For that it is important to create tests for the bugs you uncover before fixing them to ensure regression will not expose the bug again.

You also need to take into consideration if you want to test "offline" (during development, with [[pytest]]), "online" (at runtime, for example with [[Great Expectations]]), or a combination of both.

### Example

here are our feature functions:
```python
!pip install ip2geotools requests

import ipaddress

from ip2geotools.databases.noncommercial import DbIpCity

!pip install ip2geotools requests

import ipaddress

from ip2geotools.databases.noncommercial import DbIpCity

def ip_str_to_int(ip: str) -> int:

""" Converts IP address from string to int

Args:

ip (str): IP address as a string

Returns:

ip (int): IP address as an int

"""

return int(ipaddress.ip_address(ip))

def ip_int_to_str(ip: int) -> str:

""" Converts IP address from string to int

Args:

ip (str): IP address as a string

Returns:

ip (int): IP address as an int

"""

return str(ipaddress.ip_network(ip)).partition("/")[0]

def ip_str_to_city(ip: str) -> str:

""" Looks up city name for a given stringified IP address

Args:

ip (str): IP address as a string

Returns:

city-name (str): City where the IP address is located

"""

response = DbIpCity.get(ip, api_key='free')

return response.city
```


and the associated uni tests:
```python
from features import ip_features as ipf

from unittest import TestCase

import hsfs

import time

import pytest

from contextlib import nullcontext as does_not_raise

class IpInToCityTest(TestCase):

@pytest.fixture(autouse=True) # this method is only called once per class - use for common setup

def init_ips(self):

self._ips = ['92.33.156.248', '23.13.249.185', '15.197.242.139', '54.229.181.153']

self._ips_as_ints = [1545706744, 386791865, 264630923, 921023897]

# We expect these cities to be returned for the above IP addresses

self._cities = ['Stockholm (Södermalm)', 'Stockholm (Kungsholmen)', 'Montreal', 'Dublin']

def test_ip_str_to_int(self):

with does_not_raise():

ips_as_ints = [ ipf.ip_str_to_int(ip) for ip in self._ips ]

for i in range(0, len(ips_as_ints)):

assert self._ips_as_ints[i] == ips_as_ints[i]

def test_ip_int_to_str(self):

ips_as_ints = [ ipf.ip_str_to_int(ip) for ip in self._ips ]

ips_as_strs = [ ipf.ip_int_to_str(ip) for ip in ips_as_ints ]

for i in range(0, len(ips_as_strs)):

assert self._ips[i] == ips_as_strs[i]

def test_ip_str_to_city(self):

ip_to_cities = [ ipf.ip_str_to_city(ip) for ip in self._ips ]

for idx in range(len(ip_to_cities)):

assert ip_to_cities[idx] == self._cities[idx]
```

we verify:
1. That IPs are corectly transformed into ints.
2. The IPs are corretly transformed back into strings
3. THe IPs are correcly mapped to the city values


### Unit tests for transformation functions
We still nod consider teh fact that we want to convert our data into nuerical values, even if they are city names if we want to train a model. For that we use transformation functions.

**Transformation functions prevent training-serving skew by providing a single piece of code to transform an input feature value into an output format used by the model for training and serving.**

There are many of these transformation functions built into the frameworks we use, but sometimes we need to create our own.

To show an example:
```python
from datetime import datetime

def date_string_to_timestamp(input_date):

""" Converts stringified date to a timestamp

Args:

input_date (str): The input

Returns:

timestamp (int): The Unix timestamp

"""

date_format = "%m-%d-%Y %H:%M %S"

return int(float(datetime.strptime(input_date, date_format).timestamp()) * 1000)
```

To transform a date into an numeric value. Now we wan to verify that this works correctly, and only with valid dates.

```python
from features import date_transform as dt

from unittest import TestCase

import time

import pytest

from contextlib import nullcontext as does_not_raise

@pytest.mark.parametrize(

"example_date, excp",

[('03-31-1970 12:20 59', does_not_raise())

,('12-01-2020 09:59 00', does_not_raise())

,('13-01-2020 09:59 00', pytest.raises(Exception))]

)

def test_date_transform(example_date, excp):

with excp:

date_as_int = dt.date_string_to_timestamp(example_date)

intvalue = int(date_as_int)
```



## Pytest for Feature pipelines

**A feature pipeline is a program that can be run on a schedule or continuously that reads new input data from some data source, computes features using that input data, and writes the features to the feature store.**

We can use pytest to run these end-to-end tests. 

Here is a simple example:
```python
import hsfs

import pandas as pd

connection = hsfs.connection(

# hostname for your Hopsworks cluster

host="791bb4a0-bb1c-11ec-8721-7bd8cdac0b54.cloud.hopsworks.ai",

project="demo_fs_jim00000",

engine="hive",

secrets_store="local",

api_key_file="./api-key.txt"

)

fs = connection.get_feature_store()

df = pd.read_csv("./click-logs.csv", dtype={'is_attributed': 'bool'}, parse_dates=['click_time'])

fg = fs.create_feature_group("clicks",

version=1,

description="User clicks on our website",

primary_key=['ip', 'click_time'],

online_enabled=True)

fg.save(df)
```

Here, we write a simple end-to-end test that performs a row count validation on the features written. The feature pipeline reads sample data into a Pandas DataFrame, computes some features in the DataFrame, writes the DataFrame to a feature group to the feature store, and then reads the data back from the feature store as a DataFrame, validating that the number of rows in the original DataFrame is equal to the number of rows in the DataFrame read back from the feature store.

```python
from pipeline import feature_pipeline

from unittest import TestCase

import hsfs

import time

import pytest

class HsfsTest(TestCase):

def test_create_write(self):

self._connection = feature_pipeline.connect(host="<hostname>",project="dev")

self._fs = self._connection.get_feature_store()

df = feature_pipeline.read_data("sample-click-logs.csv")

df = feature_pipeline.engineer_features(df)

try:

fg = self._fs.get_feature_group("clicks",version=1)

except:

fg = self._fs.create_feature_group("clicks",

description="User clicks on our website",

primary_key=['id'],

online_enabled=True)

fg.insert(df)

df_read = fg.read(online=True)

# Validate number of rows in the 'clicks' FG same as num of rows in DF written.

assert df_read.count() == df.count()

self._connection.close()
```

You can see the init_fs function is run first to set up the tests by first deleting the clicks feature group if it exists, then creating it, then reading up the sample data into a Pandas DataFrame. Then the tests are run against the feature group and the DataFrame.




___
# References
[Testing features with pytest. Testing feature logic, transformations… | by Jim Dowling | Towards Data Science](https://towardsdatascience.com/testing-features-with-pytest-82765a13a0e7)