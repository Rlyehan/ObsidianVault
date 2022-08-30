222022-08-26

Style: #Research 

Domain: #TestAutomation 

Tags: #Data 

# Ways I Use Testing as a Data Scientist
One easy to use for ad-hoc scenarios could be to use the powerful tool we can use is the simple ```assert``` statement. This tool can help us to make sure we do not run into problems even for problems we think that might be to trivial to even test, just to make sure we didn't make any silly mistakes like typos.

One more powerful tool we can use for to ease our testing needs would be tools like pythons [[hypothesis]] . This tool makes use of our specification of the properties of the data to generate example inputs.

**as an Example**:
I wrote a function that returned a bunch of information about a column of these values, similar to the following:

```python

def summarize_likert(series: pd.Series):
    n = len(series)
    n_completed = series.notnull().sum()
    mean = series.mean()
    empty = series.isnull().sum()
    pct_empty = empty / n
    small = n_completed <= 7
    summary = dict(
        n=n,
        n_completed=n_completed,
        mean=mean,
        empty=empty,
        pct_empty=pct_empty,
        small=small,
    )
    return summary
```

Pretty simple, right? Now I’ll show how to use `hypothesis` to run some tests with this type of function. There is a bit of setup to create a hypothesis strategy that mimics our data, but we only have to do it once and it’s reusable.

```python

from hypothesis import assume, given, strategies as st
from hypothesis.extra.pandas import range_indexes, series

# DEFINE STRATEGY
@st.composite
def plus_nan(draw, strat):
    return draw(st.one_of(st.just(np.nan), strat))


index_strategy = range_indexes(min_size=0, max_size=500)
likert_data = st.integers(min_value=1, max_value=5)
likert_data_with_nan = plus_nan(likert_data)

likert_series = series(elements=likert_data, index=index_strategy)
likert_series_with_nan = series(elements=likert_data_with_nan, index=index_strategy)


# CREATE TEST
@given(likert_series_with_nan)
def test_likert_na(series):
    summary = summarize_likert(series)
    # Likert are 1-5, so mean is in that interval
    assert 1 <= summary["mean"] <= 5

# EXECUTE TEST
if __name__ == "__main__":
	# actually run test
	# can use pytest as well
    test_likert_na()
```

If we run this simple test, we’ll get the following error:

```
RuntimeWarning: invalid value encountered in long_scalars
  pct_empty = empty / n
Falsifying example: test_likert_na(
    series=Series([], dtype: float64),
)
```

This is `hypothesis` telling us we had an error at runtime with the code and showing us the example it generated that gave that error. It turns out we have behavior that doesn’t work when passed an empty `Series`. At first I thought this wasn’t worth considering, but in my use case I was often automatically filtering data based on other columns, so it is realistic at some point I might pass an empty series and want to know about it. At this point, we’d write some behavior to handle the case where we are passed an empty series. A simple fix would be to add the following at the top of our function and add an assumption to the test that the series is not empty.

```python
def summarize_likert(series: pd.Series)
	if series.empty:
   		raise ValueError("Series Empty")
	...

	
@given(likert_series_with_nan)
def test_likert_na(series):
	assume(not series.empty)
    summary = summarize_likert(series)
    # Likert are 1-5, so mean is in that interval
    assert 1 <= summary["mean"] <= 5
```

So we’ve fixed that issue, let’s run the test again.

```
Falsifying example: test_likert_na(
    series=0   NaN
    dtype: float64,
)
Traceback (most recent call last):
  File "likert.py", line 50, in <module>
    test_likert_na()
  File "likert.py", line 41, in test_likert_na
    @settings(verbosity=Verbosity.verbose)
  File "/Users/peter/opt/miniconda3/envs/blog-test/lib/python3.8/site-packages/hypothesis/core.py", line 1190, in wrapped_test
    raise the_error_hypothesis_found
  File "likert.py", line 46, in test_likert_na
    assert 1 <= summary["mean"] <= 5
AssertionError
```

A similar issue: we haven’t defined what happens when the series contains all `NaN`values. If we look up further in the traceback, we actually see that `hypothesis` is smart enough that it found a more complex example that didn’t work (A longer series of `NaN`), and then reduced it down to the simpler case with only one `NaN`.


So far we looked at tests that made sure our logic was working correctly, but we generally also want to test the data itself.

### Tests on the Data: [[pandera]] & [[Great Expectations]]

It’s a good idea to write tests on the data itself. To do this, we’ll need to do a bit of exploratory work to understand the qualities of the data, then translate that into tests. Testing data is extremely helpful if we will be repeatedly receiving new data with the same structure: tests are a quick way to make sure there are no issues with new data.

For lightweight use cases, I like [`pandera`](https://pandera.readthedocs.io/en/stable/). With `pandera`, I do two high level activities with a dataset: explicitly define a schema for the data and add in any supplemental info I have about the data structure.

For the first definition of the schema, all I do is define the column names and types. We can use the [`infer_schema`](https://pandera.readthedocs.io/en/stable/schema_inference.html) method to get started with this with an existing DataFrame. From here, we need to manually validate column types and make corrections. For example, an `object` type column might be better as a categorical variable, and we’ll want to check that any timestamp columns are correctly inferred. Even something as simple as this has saved me from a costly error when a new dataset I received was missing a column I was expecting from the first version of the dataset.

From there, I create v2 of the schema, which adds [Checks](https://pandera.readthedocs.io/en/stable/checks.html) to the columns. Checks are information we gain after exploring the data – for example, whether a column should always be positive, whether the column name should be formatted a certain way, or whether a column should only contain certain values (e.g. a `bool` represented as a `0/1 int`).

If we’re expecting to repetedly read in new data, I would recommend exploring [Great Expectations](https://docs.greatexpectations.io/docs/). The killer feature of Great Expectations is that it will generate a template of tests for the data based on a sample set of data we give it, like `pandera`’s `infer_schema`on steroids. Again, this is only a starting point for adding in future tests (or _expectations_), but can be really helpful in generating basic things to test.

Great Expectations is a little more involved to setup, so I think the investment is worth it if we know we’ll be repeatedly reading in new data with the same structure. There are also some stellar additional features like the data docs, which has been helpful for me communicating any data quality issues with a larger team.

Finally, even if we are not expecting new versions of the data, writing tests about the data is still a good idea. It’s great documentation for ourselves when we come back to this project or when others join our team. Additionally, it gives us something to communicate to others to help in validating data assumptions.

___
# References
[Ways I Use Testing as a Data Scientist | Peter Baumgartner](https://www.peterbaumgartner.com/blog/testing-for-data-science/)