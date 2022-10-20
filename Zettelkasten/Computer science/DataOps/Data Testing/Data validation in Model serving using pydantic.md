222022-10-18

Style: 

Domain:

Tags:

# Data validation in Model serving using [[pydantic]]

Let's remind ourselves the architecture of a modern machine learning model:
![[Pasted image 20221018092248.png]]
So we should expect to receive the input for our model via a REST API in, and return the prediction or classification accordingly. An object arriving in the REST API can be a JSON object, but we can also receive a CSV file, txt file etc. For the purposes of this tutorial, we will assume that the input is a JSON object, and we receive it in batches. Let's create a few model inputs straight from the test data:

```python
test_house_data = pd.read_csv('https://github.com/NatanMish/data_validation/blob/a77b247b25c6622ce0c8f8cbc505228161c31a3c/data/test.csv?raw=true')

feature_names = ['YearBuilt', 'LotFrontage', 'GarageArea', 'OverallQual', 'OverallCond', 'MSZoning','TotalBsmtSF']

# we'll choose one record from the test data
input_dict = test_house_data[feature_names].loc[1108].to_dict()
```

We can already see that one of the features is missing. If our model is not made to handle missing values, it will break and no one wants that.

Pydantic allows data validation and settings management using python type annotations. It enforces type hints at runtime, and provides user-friendly errors when data is invalid. Many great and loved projects are using Pydantic extensively, even the Jupyter project for notebooks we are using right now! Let's see how we can use it to validate our model inputs:

```python
# The most basic building block of Pydantic is the BaseModel class. We can use it to define our custom models that 
# define the structure of our objects:
class Input(BaseModel):
    YearBuilt: int
    LotFrontage: int
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: str
    TotalBsmtSF: float


# We can now use the Input class, and create a Input object from our sample record like so:
model_input_object = Input(
    YearBuilt=input_dict['YearBuilt'],
    LotFrontage=input_dict['LotFrontage'],
    GarageArea=input_dict['GarageArea'],
    OverallQual=input_dict['OverallQual'],
    OverallCond=input_dict['OverallCond'],
    MSZoning=input_dict['MSZoning'],
    TotalBsmtSF=input_dict['TotalBsmtSF']
)

```

As we can see, the Input object raised an error while validating because one of the features is missing. Let's see how we can adjust the model, so it can handle missing values:

```python
# What's cool about Pydantic is that it allows leveraging the built-in Python typing definitions.
from typing import Optional, Any

class Input(BaseModel):
    YearBuilt: int
    LotFrontage: Optional[Any]
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: str
    TotalBsmtSF: float

# Instead of specifying each field one by one, we can use the **kwargs syntax to specify a list of fields:
input_object = Input(**input_dict)
inpout_object_2 = Input.parse_obj(input_dict)
```

#### 1. Recursive models

Pydantic supports recursive models. This means that we can define a model that contains other models. For example, we can define a model that contains a batch of Input objects:

```python
from typing import List

class InputBatch(BaseModel):
    inputs: List[Input]

# We'll grab a sample of the data:
data_sample_dict = test_house_data[feature_names].sample(n=5).to_dict('index')
data_sample_dict

# And we can create a InputBatch object from our sample data:
inputs_list = [Input(**input_dict) for input_dict in data_sample_dict.values()]
input_batch = InputBatch(inputs=inputs_list)

```


#### 2. Enums

Pydantic supports enumerations. This means that we can define a model that has a set of predefined values. For example, we can define a model that has a set of values for the MSZoning feature:

```python
from enum import Enum

class MSZoning(str, Enum):
    C = 'C (all)'
    FV = 'FV'
    RH = 'RH'
    RL = 'RL'
    RM = 'RM'

# We can then include the enum in our model:
class Input(BaseModel):
    YearBuilt: int
    LotFrontage: Optional[float]
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: MSZoning
    TotalBsmtSF: float
```

#### 3. Custom base models

When developing a model you might end up referencing the model's field names many times in many different places using its literal name, for example: `year_built = model_input_object.YearBuilt` in the case that the field name changes in the future, you might have to update all the places that use the literal name which could be a pain. Luckily, we can define an extended base model that allows us to use the field name directly. Then we can create an enum class that holds the field names and use that whenever we set or get a specific field. If the field name changes in the source, we only have to update the enum class.

```python
class ExtendedBaseModel(BaseModel):
    def __getitem__(self, item):
        return getattr(self, item)

    def __setitem__(self, item, value):
        return setattr(self, item, value)

# by using the extended base model, we can use a bracket notation to access the field values:
class Input(ExtendedBaseModel):
    YearBuilt: int
    LotFrontage: float
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: MSZoning
    TotalBsmtSF: float

input_object = Input(**input_dict)
# now we can use a bracket notation to access the field values:
input_object['YearBuilt']

# Let's create an enum class that holds the field names:
class InputFieldNames(str, Enum):
    YearBuilt = 'YearBuilt'
    LotFrontage = 'LotFrontage'
    GarageArea = 'GarageArea'
    OverallQual = 'OverallQual'
    OverallCond = 'OverallCond'
    MSZoning = 'MSZoning'
    TotalBsmtSF = 'TotalBsmtSF'


```

#### 4. Catch invalid data

In the case that an invalid model input arrives, we can catch it, display a warning message, isolate the record and then continue without breaking the model. This can be done using a try except block. This is a common and valid practice in Python called EAFP - "easier to ask for forgiveness than permission" which might not be as well recieved in other languages.

```python
from pydantic import ValidationError

class Input(ExtendedBaseModel):
    YearBuilt: int
    LotFrontage: int
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: MSZoning
    TotalBsmtSF: float

try:
    model_input_object = Input(**input_dict)
except ValidationError as e:
    print(f'Record: {input_dict}')
    print(f'Bad data: {e.errors()}')

    # isolate the record and then continue
```

#### 5. Custom validation

```python
from pydantic import validator
# Let's create a model input class with a custom validation function that checks that the garage area is greater than 1000
class Input(ExtendedBaseModel):
    YearBuilt: int
    LotFrontage: Optional[float]
    GarageArea: float
    OverallQual: int
    OverallCond: int
    MSZoning: MSZoning
    TotalBsmtSF: float

    @validator('GarageArea')
    def check_garage_area(v):
        if v % 5 != 0:
            raise ValueError('Garage area must be divisible by 5')
        return v
    
    @validator(
        "LotFrontage",
        "GarageArea",
        pre=False,
        each_item=True,
    )
    def set_metrics_precision(cls, v):
        """Round all figures to 2 decimal places"""
        return round(v, 2)
```

Example:
```python
class FireplaceQu(str, Enum):
    Ex = 'Ex'
    Gd = 'Gd'
    TA = 'TA'
    Fa = 'Fa'
    Po = 'Po'

class Input(BaseModel):
    YearBuilt: int
    Fireplaces: Optional[int]
    FireplaceQu: Optional[FireplaceQu]

    @validator("FireplaceQu")
    @classmethod
    def validate_fire_places_quality_field(cls, field_value, values):
        if values["Fireplaces"]>0 and field_value is None:
            raise ValueError(f"FireplaceQu is required when Fireplaces is greater than 0")
        return field_value

    class Config:
        extra = 'forbid'
        allow_mutation = False
```

### 6. JSON Serialization

After we have received an object, and our model generated an output, we need to serialize it to JSON so we can send it back to the client. We'll start by creating an output model class to define how our output will look like.

```python
from datetime import datetime
from typing import Tuple
from pydantic import confloat, Field
from uuid import UUID, uuid4

class ImportantFeature(BaseModel):
    FeatureName: str
    FeatureValue: Any
    ImportanceScore: confloat(ge=0, le=1)

class HousePricePrediction(BaseModel):
    PredictionId: UUID = Field(default_factory=uuid4)
    HousePrice: confloat(ge=0)
    PredictionGenerationTime: datetime = Field(default_factory=datetime.now)
    Explanation: Optional[List[ImportantFeature]]
    ConfidenceInterval: Optional[Tuple[float, float]]


# Let's create an output object:
output_object = HousePricePrediction(
    HousePrice=12345,
    Explanation=[
        ImportantFeature(FeatureName='YearBuilt', FeatureValue=1901, ImportanceScore=0.5),
        ImportantFeature(FeatureName='Fireplaces', FeatureValue=0, ImportanceScore=0.5),
        ImportantFeature(FeatureName='FireplaceQu', FeatureValue='Ex', ImportanceScore=0.5)
    ],
    ConfidenceInterval=(12000, 13000)
)
output_object
```

When we want to send this object over an API to the client, we need to serialize it to JSON. We can do this using the `json()` method. This method is super useful because it can detect and handle different fields types and convert them to a JSON friendly format, usually better than just using `json.dumps(output_object)`.

We can even define custom rules for serialization using the `json_encoders` parameter. This is a dictionary that maps the field names to functions that will be used to serialize the field.

```python
def encrypt_feature_value(feature: ImportantFeature, encryption_key="some_secret_key"):
    feature.FeatureValue = feature.FeatureValue.encode(encryption_key)
    return feature

class HousePricePrediction(BaseModel):
    PredictionId: UUID = Field(default_factory=uuid4)
    HousePrice: confloat(ge=0)
    PredictionGenerationTime: datetime = Field(default_factory=datetime.now)
    Explanation: Optional[List[ImportantFeature]]
    ConfidenceInterval: Optional[Tuple[float, float]]

    class Config:
        json_encoders = {
            'prediction_generation_time': datetime.isoformat,
            'explanation': lambda important_features: [encrypt_feature_value(feature_explanation) for feature_explanation in important_features]
        }
```

### 7. Hypothesis plugin

Hypothesis is a Python package used for property based testing. We can use it in combination with Pydantic as a tool for generating random data and testing the validity of a data model.

```python
from hypothesis import given, strategies as st

@given(st.builds(HousePricePrediction))
def test_property(instance):
    # Hypothesis calls this test function many times with varied Models,
    # so you can write a test that should pass given *any* instance.
    assert len(str(instance.PredictionId)) == 36
    assert instance.HousePrice >= 0
    assert instance.PredictionGenerationTime is not None

test_property()
```

### 8. Base settings

One of the most useful features in Pydantic is the `BaseSettings` class. This class allows for centralising config keys and variables that are used throughout the project. The feature allows three different ways to parse environment variables into the object:

```python
import os
os.environ['ENCRYPTION_KEY'] = "some_secret_key"
os.environ['MY_API_KEY'] = "my_api_key"
os.environ['REDIS_URL'] = "redis://redis:6379"

from pydantic import BaseSettings, RedisDsn
class ProjectConfig(BaseSettings):
    RedisUrl: RedisDsn
    ApiKey: str = Field(..., env='MY_API_KEY')
    ENCRYPTION_KEY: str

    class Config:
        fields = {
            'RedisUrl': {
                'env': ['REDIS_URL']
            }
        }
        case_sensitive = False
```

___
# References
[data_validation/3_model_serving_data_validation.ipynb at main · NatanMish/data_validation · GitHub](https://github.com/NatanMish/data_validation/blob/main/notebooks/3_model_serving_data_validation.ipynb)