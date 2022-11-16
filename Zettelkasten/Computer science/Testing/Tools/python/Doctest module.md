222022-11-08

Style: 

Domain:

Tags:

# Doctest module

Are you interested in writing usage examples for your code that work as **documentation** and **test cases** simultaneously? If your answer is _yes_, then Python’s `doctest` module is for you. This module provides a testing framework that doesn’t have too steep a learning curve. It’ll allow you to use code examples for two purposes: documenting and testing your code.

Apart from allowing you to use your code’s documentation for testing the code itself, `doctest` will help you keep your code and its documentation in perfect sync at any moment.

### What `doctest` Is and How It Works[](https://realpython.com/python-doctest/#what-doctest-is-and-how-it-works "Permanent link")

The `doctest` module is a lightweight **testing framework** that provides quick and straightforward [test automation](https://en.wikipedia.org/wiki/Test_automation). It can read the test cases from your project’s documentation and your code’s docstrings. This framework is shipped with the Python interpreter and adheres to the batteries-included philosophy.

You can use `doctest` from either your code or your command line. To find and run your test cases, `doctest` follows a few steps:

1.  Searches for text that looks like Python [interactive sessions](https://realpython.com/interacting-with-python/) in your documentation and docstrings
2.  Parses those pieces of text to distinguish between executable code and expected results
3.  Runs the executable code like regular Python code
4.  Compares the execution result with the expected result

The `doctest` framework searches for test cases in your documentation and the docstrings of packages, modules, functions, classes, and methods. It doesn’t search for test cases in any objects that you [import](https://realpython.com/python-import/).

In general, `doctest` interprets as executable Python code all those lines of text that start with the primary (`>>>`) or secondary (`...`) REPL prompts. The lines immediately after either prompt are understood as the code’s expected output or result.

### What `doctest` Is Useful For[](https://realpython.com/python-doctest/#what-doctest-is-useful-for "Permanent link")

The `doctest` framework is well suited for the quick automation of [acceptance tests](https://en.wikipedia.org/wiki/Acceptance_testing) at the [integration](https://en.wikipedia.org/wiki/Integration_testing) and system testing levels. Acceptance tests are those tests that you run to determine if the [specifications](https://en.wikipedia.org/wiki/Specification) of a given project are met, while integration tests are intended to guarantee that different components of a project work correctly as a group.

Your `doctest` tests can live in your project’s documentation and your code’s docstrings. For example, a **package-level docstring** containing `doctest` tests is a great and fast way to do integration tests. At this level, you can test the entire package and the integration of its modules, classes, functions, and so on.

A set of high-level `doctest` tests is an excellent way to define a program’s specification up front. At the same time, lower-level [unit tests](https://en.wikipedia.org/wiki/Unit_testing) let you design the individual building blocks of your program. Then it’s just a matter of letting your computer check the code against the tests any time you want.

At the class, method, and function level, `doctest` tests are a powerful tool for testing your code as you write it. You can gradually add test cases to your docstrings while you’re writing the code itself. This practice will allow you to generate more reliable and robust code, especially if you stick to the [test-driven development](https://realpython.com/python-doctest/#using-doctest-for-test-driven-development) principles.

In summary, you can use `doctest` for the following purposes:

-   Writing **quick and effective test cases** to check your code as you write it
-   Running **acceptance**, **regression**, and **integration** test cases on your projects, packages, and modules
-   Checking if your **docstrings** are **up-to-date** and in **sync** with the target code
-   Verifying if your projects’ **documentation** is **up-to-date**
-   Writing **hands-on tutorials** for your projects, packages, and modules
-   Illustrating how to **use your projects’ APIs** and what the expected input and output must be

Having `doctest` tests in your documentation and docstrings is an excellent way for your clients or teammates to run those tests when evaluating the features, specifications, and quality of your code.

## Doctest example

Here is a simple example:
```python
def add(a, b):
"""Compute and return the sum of two numbers.

Usage examples:
 >>> add(4.0, 2.0)
 6.0
 >>> add(4, 2)
 6,0
 """
 return float(a + b)
```

we have two usage examples, or test cases, in our docstring here. We can run these tests using the command: `python -m doctest file.py` 

One important thing to note is that doctest is very strict with the required matching of outputs. So there is no leeway of any form in them and we will need to update our doctests for every little change to the output.

When dealing with exception tracebacks, `doctest` completely ignores the traceback body because it can change unexpectedly. In practice, `doctest` is only concerned about the first line, which reads `Traceback (most recent call last):`, and the last line. As you already know, the first line is common to all exception tracebacks, while the last line shows information about the raised exception.

A more complex example:
```python
from collections import deque
 
 class Queue:
   def __init__(self):
        self._elements = deque()

   def enqueue(self, element):
       """Add items to the right end of the queue.

        >>> numbers = Queue()
        >>> numbers
        Queue([])

       >>> for number in range(1, 4):
       ...     numbers.enqueue(number)

       >>> numbers
        Queue([1, 2, 3])
        """
        self._elements.append(element)

    def dequeue(self):
        """Remove and return an item from the left end of the queue.

        >>> numbers = Queue()
        >>> for number in range(1, 4):
        ...     numbers.enqueue(number)
        >>> numbers
        Queue([1, 2, 3])

        >>> numbers.dequeue()
        1
        >>> numbers.dequeue()
        2
        >>> numbers.dequeue()
        3
       >>> numbers
        Queue([])
        """
       return self._elements.popleft()

    def __repr__(self):
        return f"{type(self).__name__}({list(self._elements)})"
```

If we have to deal with Whitespaces or other character the rules are a bit more complex.

### Embedding `doctest` Tests in Your Code’s Docstrings[](https://realpython.com/python-doctest/#embedding-doctest-tests-in-your-codes-docstrings "Permanent link")

The final, and probably the most common, way to provide `doctest` tests is through your project’s docstrings. With docstrings, you can have tests at different levels:

-   Package
-   Module
-   Class and methods
-   Functions

You can write package-level `doctest` tests inside the docstring of your package’s [`__init__.py`](https://realpython.com/python-modules-packages/#package-initialization) file. The other tests will live in the docstrings of their respective container objects.

## Exploring Some Limitations of `doctest`[](https://realpython.com/python-doctest/#exploring-some-limitations-of-doctest "Permanent link")

Probably the most significant limitation of `doctest` compared to other testing frameworks is the lack of features equivalent to [fixtures](https://docs.pytest.org/en/6.2.x/fixture.html#fixture) in pytest or the [setup](https://docs.python.org/3/library/unittest.html#unittest.TestCase.setUp) and [teardown](https://docs.python.org/3/library/unittest.html#unittest.TestCase.tearDown) mechanisms in [`unittest`](https://realpython.com/python-testing/#unittest). If you ever need setup and teardown code, then you’ll have to write it in every affected docstring. Alternatively, you can use the [`unittest` API](https://docs.python.org/3/library/doctest.html?highlight=doctest#unittest-api), which provides some setup and teardown options.

Another limitation of `doctest` is that it strictly compares the test’s expected output with the test’s actual output. The `doctest` module requires exact matches. If only a single character doesn’t match, then the test fails. This behavior makes it hard to test some Python objects correctly.

As an example of how this strict matching might get in the way, imagine that you’re testing a function that returns a [set](https://realpython.com/python-sets/). In Python, sets don’t store their elements in any particular order, so your tests will fail most of the time because of the random order of elements.


___
# References
[Python's doctest: Document and Test Your Code at Once – Real Python](https://realpython.com/python-doctest/)