222022-10-17

Style: 

Domain:

Tags:

# Data Validation in Training Pipelines using [[Pandera]]

```python
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.base import BaseEstimator
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
import pandera as pa
from typing import Optional

house_data = pd.read_csv('https://github.com/NatanMish/data_validation/blob/a77b247b25c6622ce0c8f8cbc505228161c31a3c/data/train.csv?raw=true')
```

#### Train basic model

We'll start by setting up a training pipeline using Scikit Learn's native class. We only want to select a few basic features for the purpose of this example, so we'll set up a pipeline step class that will select only those features.

```python
class ChooseFeatures(BaseEstimator):
    def __init__(self, features=None):
        self.features = features
    def fit(self, X, y=None):
        return self
    def transform(self, X, y=None):
        return X[self.features]

feature_names = ['LotArea','YearBuilt','1stFlrSF','2ndFlrSF','FullBath','BedroomAbvGr','TotRmsAbvGrd', 'LotFrontage']

pipe = Pipeline([
     ('feature_selection', ChooseFeatures(features=feature_names)),
     ('scaler', StandardScaler()),
     ('rf', RandomForestRegressor())
])

X = house_data
y = house_data.SalePrice
pipe.fit(house_data, y)

```

Pandera provides a flexible and expressive API for performing data validation on dataframes to make data processing pipelines more readable and robust. Dataframes contain information that pandera explicitly validates at runtime. This is useful in production-critical data pipelines or reproducible research settings. We'll take a look at these Pandera features:

1.  Check the types and properties of columns in a pd.DataFrame or values in a pd.Series.
    
2.  Perform more complex statistical validation like hypothesis testing.
    
3.  Integrate with existing data analysis/processing pipelines via function decorators.
    
4.  Define schema models with the class-based API with pydantic-style syntax and validate dataframes using the typing syntax.
    
5.  Synthesize data from schema objects for property-based testing with pandas data structures.
    
6.  Lazily Validate dataframes so that all validation rules are executed before raising an error.
    

For more information, see [Pandera's documentation](https://pandera.readthedocs.io/en/latest/).

#### 1. DataFrame Schemas - Type Validation

```python
# We'll add one more feature to make it more interesting
feature_names.append('LotConfig')

# Create a basic schema for the house_data DataFrame to check types for just 2 of the feature
basic_types_schema = pa.DataFrameSchema({
    "LotArea": pa.Column(int),
    "LotConfig": pa.Column(str),
    })

# Validate the house_data DataFrame against the basic_schema
# notice that although we only defined two of the features in the dataframe, and Pandera ignored the rest.
basic_types_schema.validate(house_data[feature_names])
```

There is an output from the validation, this means that the data is valid. There are different ways we can specify the type:

-   a string alias, as long as it is recognized by pandas.
-   a python type: int, float, double, bool, str
-   a numpy data type
-   a pandas extension type: it can be an instance (e.g pd.CategoricalDtype([“a”, “b”])) or a class (e.g pandas.CategoricalDtype) if it can be initialized with default values.
-   a pandera DataType: it can also be an instance or a class.

```python
# Now let's create a schema that does not fit the data types in house data
bad_types_schema = pa.DataFrameSchema({
    "LotArea": pa.Column(int),
    "LotConfig": pa.Column(float),
})

# The bad schema validation will throw an error
bad_types_schema.validate(house_data[feature_names])
```

#### 2. DataFrame Schemas - Value Ranges Validation

```python
# Pandera also allows validating value ranges for numerical columns
value_range_schema = pa.DataFrameSchema({
    "LotArea": pa.Column(int, pa.Check(lambda s: s <= 1000000), nullable=False),
    "YearBuilt": pa.Column(int, pa.Check.in_range(1800, 2022)),
})

# Validate the house_data DataFrame against the value_range_schema
value_range_schema.validate(house_data[feature_names])
```

#### 3. DataFrame Schemas - Catch Bad Data

What if instead of breaking on error we want to continue processing the dataframe? or we want to skip the bad data? we can use the `failure_cases` attribute of the error message to capture the bad data indices and the `lazy` argument for going over the entire dataframe instead of failing on the first bad row. We can do that by utilizing a try-except block.

```python
# We'll use a small sample of the data to make the example more clear
sample_data = house_data.sample(n=10)

# Create a schema that will fail on the first bad data point
catch_bad_data_schema = pa.DataFrameSchema({
    "LotArea": pa.Column(int, pa.Check(lambda s: s <= 1000000)),
    "YearBuilt": pa.Column(int, pa.Check.in_range(1900,1990)),  # notice that the year built has a restrictive range
})

# Validating the house_data DataFrame against the catch_bad_data_schema will throw an error
catch_bad_data_schema.validate(sample_data[feature_names])
```

Now let's use a try except block to catch the bad data indices. This is a common and valid practice in Python called EAFP - "easier to ask for forgiveness than permission" which might not be as well recieved in other languages.

```python
try:
    catch_bad_data_schema.validate(sample_data[feature_names], lazy=True)
except pa.errors.SchemaErrors as e:
    failure_cases = e.failure_cases

# Failure cases is a dataframe of the bad data only
failure_cases.head()

# We can easily filter out the bad data from the original dataframe using the failure_cases dataframe
filtered_df = sample_data[~sample_data.index.isin(failure_cases["index"])]

# Let's see that the filtered data passes the validation test
catch_bad_data_schema.validate(filtered_df[feature_names])

```

#### 4. DataFrame Schemas - Validate acceptable categorical values

```python
lot_config_values = ["Inside", "Corner", "CulDSac", "FR3"]

lot_config_values_schema = pa.DataFrameSchema({
    "LotArea": pa.Column(int, pa.Check(lambda s: s <= 1000000)),
    "LotConfig": pa.Column(str, pa.Check.isin(lot_config_values)),
})

# Validating the house_data DataFrame against the lot_config_values_schema will throw an error
lot_config_values_schema.validate(house_data[feature_names])
```

#### 5. DataFrame Schemas - `Coerce`

`Coerce` allows forcing type onto a specific dataframe column

```python
house_data.LotArea.dtype

coerce_schema = pa.DataFrameSchema(
    columns={"LotArea": pa.Column(float)},
    coerce=False,
)

coerce_schema.validate(house_data)

# and if we set coerce to True, we can coerce the dataframe to the schema
coerce_schema = pa.DataFrameSchema(
    columns={"LotArea": pa.Column(float)},
    coerce=True,
)

coerce_schema.validate(house_data)
```


### 7. Pandera Decorators

Pandera offers decorators which allow a seamless integration of Pandera validations with our code. The available decorators are:

-   @check_input
-   @check_output
-   @check_io
-   @check_types

We will use a different way for defining the schemas in the next example, but the same principles apply. Here we will construct a class based Pandera model which we can use to validate inputs and outputs to our data in Pydantic style syntax.

```python

feature_names.remove('LotConfig')

from pandera.typing import Series

# Define a class based Pandera model for the input data to the feature engineering step
class FeaturesSchemaPreEngineering(pa.SchemaModel):
    LotArea: Series[int] = pa.Field(nullable=False, ge=0)
    YearBuilt: Series[int] = pa.Field(nullable=False, ge=1700)
    FirstFlrSF: Series[int] = pa.Field(nullable=False, ge=0, alias="1stFlrSF") # alias is used to give the column a different name in the schema because the column name starts with a number
    SecondFlrSF: Series[int] = pa.Field(nullable=False, ge=0, alias="2ndFlrSF")
    FullBath: Series[int] = pa.Field(nullable=False, ge=0)
    BedroomAbvGr: Series[int] = pa.Field(nullable=False, ge=0)
    TotRmsAbvGrd: Series[int] = pa.Field(nullable=False, ge=0)
    LotFrontage: Series[float] = pa.Field(nullable=True, ge=0)
    class Config:
        strict=True

# Define a class based Pandera model for the output data to the feature engineering step, notice how we inherit the FeaturesSchemaPreEngineering class and extend it with the output data schema.
class FeaturesSchemaPostEngineering(FeaturesSchemaPreEngineering):
    LotFrontage: Series[float] = pa.Field(nullable=False, ge=0)
    HouseAge: Series[int] = pa.Field(nullable=False, ge=0)
    AllFloorsSF: Series[int] = pa.Field(nullable=False, ge=0)
    NonBedRmAbvGrd: Series[int] = pa.Field(nullable=False, ge=0)

    class Config:
        strict=True
```

```python
from pandera import check_types
from pandera.typing import DataFrame as DataFramePa
@check_types
def feature_engineering(df: DataFramePa[FeaturesSchemaPreEngineering]) -> DataFramePa[FeaturesSchemaPostEngineering]:
    df = df.copy()
    df["HouseAge"] = 2022 - df["YearBuilt"]
    df["AllFloorsSF"] = df["1stFlrSF"] + df["2ndFlrSF"]
    df["NonBedRmAbvGrd"] = df["TotRmsAbvGrd"] - df["BedroomAbvGr"]
    return df

# This run should fail because we haven't filtered out the null values
feature_engineering(house_data.loc[:, feature_names])

# This run should complete without error
feature_engineering(house_data.loc[~house_data.LotFrontage.isnull(), feature_names])
```


### 8. Data Synthesis

Pandera offers a simple way to generate synthetic data. We can use the `example` method to generate a DataFrame with a given schema.
```python
FeaturesSchemaPreEngineering.example(size=5)
```

Notice how some of the columns have 'crazy' values, this is because the random data generating process is using the checks we have defined in the schema for detecting the acceptable ranges possible.

We can use the hypothesis library to generate data for our schema and then use it in a unit test:
```python
import hypothesis
@hypothesis.given(FeaturesSchemaPreEngineering.strategy(size=1))
def test_processing_fn(dataframe):
    feature_engineering(dataframe)
    
test_processing_fn()
```

### 9. Schema Inference

Pandera can infer schemas from data. This is useful when you have a large dataset and you don't want to define a schema manually.

```python
schema = pa.infer_schema(house_data)
print(schema)

```


# References
[data_validation/2_training_pipeline_data_validation.ipynb at main · NatanMish/data_validation · GitHub](https://github.com/NatanMish/data_validation/blob/main/notebooks/2_training_pipeline_data_validation.ipynb)