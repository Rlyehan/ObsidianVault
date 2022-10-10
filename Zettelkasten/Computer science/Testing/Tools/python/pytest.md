222022-09-01

Style: #ArticleNotes 

Domain: #tools 

Tags:

# The Basics

A unit test is atomic- it just tests one unit of code. Typically one function or one method of a class.

we firt need to install pytest:
`pip install pytest

A _test suite_ is an arbitrary collection of tests. Usually, you mean all tests.

# Why do we test at all?

-   **Trust**: You checked at least some cases if they work. So others can have more trust in the quality of your work and you can also put more trust in it.
-   **Breaking Changes**: For a bigger project, it is sometimes hard to have every part in mind. By writing tests, you make it easier to change something and see if / where things break. This does not only help you but also team members. Including once that are not there yet.
-   **Code Style**: When you know that you have to write tests, you write some things slightly differently. Those slight differences usually improve the coding style. Sometimes, they are crucial. For example, if you have to thoroughly test your code you will make smaller chunks.
-   **Documentation**: Some test cases show a little bit of how the code is intended to be used.

`[pytest-cov](https://pypi.org/project/pytest-cov/)` is a pytest plugin to measure branch coverage.

-   Install it with `pip install pytest-cover`
-   Use it by adding `--cov=path/to/file` or `--cov=packagename` to the pytest execution
-   Get output to terminal by adding to pytest `--cov-report term`
-   Get HTML output by adding `--cov-report html:tests/reports/coverage`

There are [more reporting capabilities](https://pytest-cov.readthedocs.io/en/latest/reporting.html).

### Type Checking

If you use [type annotations](https://medium.com/@MartinThoma/type-annotations-in-python-3-8-3b401384403d) (which you totally should!), then you can install `[pytest-mpy](https://pypi.org/project/pytest-mypy/)` . You can then automatically run mypy over your code by adding `--mypy` to your pytest command.


### Linting

[Code linting](https://en.wikipedia.org/wiki/Lint_(software)) is the act of finding bugs, stylistic errors, and suspicious constructs from static code analysis.

There are two linters I can recommend: [black](https://github.com/psf/black) and [flake8](https://flake8.pycqa.org/en/latest/). You can run them with pytest by installing `[pytest-black](https://pypi.org/project/pytest-black/)` and `[pytest-flake8](https://pypi.org/project/pytest-flake8/)`. Again, if you want to execute it just add the flag `--black` or `--flake8` to pytest:

$ pytest --flake8 --black

There is also `[pytest-mccabe](https://pypi.org/project/pytest-mccabe/)` which tries to find sections in the code which are too complex. This makes it easier for coworkers / your future self to understand the code. An alternative to pytest-mccabe outside of pytest is `[radon](https://pypi.org/project/radon/)`. However, a lot of people don’t like this type of test.

### Doctests

[Doctests](https://docs.python.org/3/library/doctest.html) are a weird but pretty awesome part of Python. Python has Docstrings — the first string within a function/class which comes directly after the signature and which is not assigned. This is not just a comment, it has meaning and can be read through the execution:

If you execute this directly, the lines 19–21 will run the doctest. The doctest looks for `>>>` within the docstrings and executes whatever follows as if it was entered in the interactive console. The next line is then the output which is compared to the output of the program.

This is pretty awesome because it makes documentation testable!

And, of course, you can also [execute doctests with pytest](https://docs.pytest.org/en/stable/doctest.html):

$ pytest --doctest-modules

### Test Execution Speed

It’s important to keep the execution time of the tests low so that it doesn’t feel bad to execute the test suite. I like to print the time of the 3 slowest tests which were performed. To [profile the tests](https://docs.pytest.org/en/stable/usage.html#profiling-test-execution-duration) continuously, I simply add the durations flag:

$ pytest --durations=3

# Patching, Mocking and dependency Injection

A dependency of the function we want to test can have an effect in three different ways: By side-effects, return values or exceptions.

### Examples for External Dependencies

There are lots of external dependencies your tests might have:

-   Date or time
-   Internet: A web service you need to use
-   File System: A file you need to create / read / edit / delete
-   Database: Data you select / insert / update/ delete
-   Randomness: Your code might make use of `random` or `np.random`

Just like the example above, they make isolated unit testing hard or even impossible.

The overall strategy to test this is always the same: Replace the external dependency that is causing headaches by something in your control. The act of replacing the dependency is called **_patching_**, the replacement is called a **_mock_**. Depending on what exactly the mock does, you might also hear this being called a Test Double, Test Stub, Test Spy or a Fake Object. In practice in Python, the distinction does not matter. If you’re interested, I recommend [Martin Fowler: The Difference Between Mocks and Stubs](https://martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs#TheDifferenceBetweenMocksAndStubs). I will call all of them just mocks.

Let's see an example:

```python
from external_dependency import dark_magic

def is_credit_card_fraud(transaction):

fraud_probability = dark_magic(transaction)

if fraud_probability > 0.99:

return True

else:

return False
```

to patch the `dark_magic` dependency:
```python
from unittest.mock import patch, MagicMock

def the_mock(input):

return 0.999

@patch("fraud_example.dark_magic", the_mock)

def test_is_credit_card_fraud():

import fraud_example

transaction = {"amount_usd": "9999.99", "overnight_shipping": True}

is_fraud = fraud_example.is_credit_card_fraud(transaction)

assert is_fraud == True
```

and using a context handler with our tests:
```python
def test_is_credit_card_fraud_context_handler():

import fraud_example

transaction = {"amount_usd": "9999.99", "overnight_shipping": True}

with patch("fraud_example.dark_magic", the_mock):

is_fraud = fraud_example.is_credit_card_fraud(transaction)

assert is_fraud == True
```

### Mocking
You now know how to replace a dependency, hence it is time to talk about what to replace it with. This is where `unittest.mock.Mock` and `unittest.mock.MagicMock` come into play.

Everything you do with Mock will return a Mock. Call a function? Get a Mock as a return value. Access an attribute? Get a Mock as a value.

Python has so called “magic” methods. I like the term “dunder” methods better — it just means all methods which start and end with a **d**ouble **under**score. Examples are `__iter__` or `__contains__` . MagicMock has those defined, Mock doesn’t. I would use MagicMock everywhere, except if the mocked object doesn’t define any of the magic functions.

A core feature of mock classes is that they allow you to not only remove a dependency which is hard to test, but also to assert on the way the mock was interacted with. Typical methods are [assert_called](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called)(), [assert_called_with](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called_with)(), [assert_not_called](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_not_called)().

A part that is really bad about `MagicMock` is that you can do anything with it — including accessing non-existing attributes, calling non-existing methods or calling existing methods with the wrong count of parameters. The mock object is missing a **spec**ification. If you don’t like that, use `autospec=True` when patching the object:

patch.object(Foo, 'foo', autospec=True)

The next parameter of `patch`is autospec. Where spec looks at the mocked object, `autospec` also looks at the attributes of that object (and their attributes and those attributes, …).

Finally, there is `spec_set` . That one prevents you from setting attributes that don’t exist.

Usually, I would use `autospec=True` and `spec_set=True` everywhere. Code which uses introspection might be an example where you don’t want that.


### pytests monkeypatch

`monkeypatch` is a fixture from pytest. I will explain what a fixture is in the next article. For now, just accept it as a parameter you can give to your tests without specifying it and pytest will take care of it. You don’t even need to import anything.

For the credit card fraud example, it looks like this:

```python
def test_is_credit_card_fraud_monkeypatch(monkeypatch):

monkeypatch.setattr("fraud_example.dark_magic", the_mock)

import fraud_example

transaction = {"amount_usd": "9999.99", "overnight_shipping": True}

is_fraud = fraud_example.is_credit_card_fraud(transaction)

assert is_fraud == True
```

The question when you should use `unittest.mock.patch` and — if necessary — `unittest.mock.Mock` or pytests `monkeypatch` boils pretty much down to personal taste nowadays. The core Pythons patch / Mock only exist since Python 3.3 which, I guess, is a big part of the reason why `monkeypatch` exists in the first place.

### Dependency Injection

If the above sounded complicated, there is a simpler alternative: Dependency Injection. Essentially adding the external state explicitly as a parameter which makes it easy to adjust in tests. For example, the code from above could be:

import datetimedef generate_filename(now=None):  
    if now is None:  
        now = datetime.datetime.now()  
    return f"{now:%Y-%m-%d}.png"

Now testing is trivial:

import datetime  
from mock_example import generate_filenamedef test_generate_filename():  
    now = datetime.datetime(1990, 4, 28)  
    assert generate_filename(now) == "1990-04-28.png"

In some cases it feels very natural to apply such a pattern, in others it doesn’t. Do this only when it feels natural. For example, it’s very unlikely that I would ever pass a module as a parameter although it’s possible. That would just feel very weird.

### Temporary files: Are Mocks a Code Smell?

It depends very much on the details, but I like to mock as little as possible. Simply for the reason that not mocking means that you test more of your system. Strictly speaking you can’t call the test a _unit test_ anymore if you test more than one unit. It would be an integration test then — but that is also essential, right? You wouldn’t be happy with BMW selling you a motor, some seats and a steering wheel and claiming “all units work”. They need work together. Extensive mocks might prevent you from testing how things work together.



# Pytest plugins

Once you're happy with the terminal output, you might think about getting reports in the browser. This could help once you have to have a look at many things, want to scroll and search. Then `[pytest-html](https://pypi.org/project/pytest-html/)` is your friend. It generates reports like this one:

 For tests which might take a long time or even result in an infinite loop in case of errors, I use `[pytest-timeout](https://pypi.org/project/pytest-timeout/)`

We also want to use our machine properly by using `[pytest-xdist](https://pypi.org/project/pytest-xdist/)` . Install it, execute `pytest -n auto`and your tests run in parallel!`[pytest-parallel](https://pypi.org/project/pytest-parallel/)`might also be worth a shot.

The most extreme speedup is not to execute stuff you don’t need. `[pytest-picked](https://github.com/anapaulagomes/pytest-picked)` executes tests that are related to unstaged files which can be way less than your complete test suite.

-   `[pytest-cov](https://pypi.org/project/pytest-cov)` : Get a test coverage report  I like to generate both, an HTML report and an output to the terminal. In some settings, an XML report is also helpful.

-   `[pytest-random-order](https://pypi.org/project/pytest-random-order/)` : Execute the tests in a random order, to make sure you see when a test leaves the system in a different state.

-   `[moto](https://pypi.org/project/moto/)` : Mocks for boto3 — AWS stuff. I don’t exactly love this one, but it is for sure the best you can do when you want to test code that uses S3.
-   `[pytest-aws](https://pypi.org/project/pytest-aws/)` : Testing AWS resource configurations
-   `[pytest-localstack](https://pypi.org/project/pytest-localstack/)` : Create AWS integration tests via a Localstack Docker container



___
# References
[Unit Testing in Python -The Basics | The Startup](https://medium.com/swlh/unit-testing-in-python-basics-21a9a57418a0)
[Unit Testing in Python — Patching, Mocks and Dependency Injection | by Martin Thoma | Level Up Coding](https://levelup.gitconnected.com/unit-testing-in-python-mocking-patching-and-dependency-injection-301280db2fed)