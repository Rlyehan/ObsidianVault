222022-09-15

Style: #ArticleNotes 

Domain: #TestAutomation 

Tags:

# Property-based Testing with Python
We were just thinking about a testing strategy for integers. A strategy is a generator of data. The property testing framework `hypothesis` offers [a lot of strategies](https://hypothesis.readthedocs.io/en/latest/data.html) for many types. You can install it with `pip install hypothesis` .

# Example: Integer factorization

We have a function `factorize(n : int) -> List[int]` which takes an integer and returns the prime factors:

```python
from typing import List

import math

def factorize(number: int) -> List[int]:

if number in [-1, 0, 1]:

return [number]

if number < 0:

return [-1] + factorize(-number)

factors = []

# Treat the factor 2 on its own

while number % 2 == 0:

factors.append(2)

number = number // 2

if number == 1:

return factors

# Now we only need to check uneven numbers

# up to the square root of the number

i = 3

while i <= int(math.ceil(number ** 0.5)) + 1:

while number % i == 0:

factors.append(i)

number = number // i

i += 2

return factors
```

so we write tests:
```python
# Third party modules

import pytest

# First party modules

from factorize import factorize

@pytest.mark.parametrize(

"n,expected",

[

(0, [0]), # 0

(1, [1]), # 1

(-1, [-1]), # -1

(-2, [-1, 2]), # A prime, but negative

(2, [2]), # Just one prime

(3, [3]), # A different prime

(6, [2, 3]), # Different primes

(8, [2, 2, 2]), # Multiple times the same prime

],

)

def test_factorize(n, expected):

assert factorize(n) == expected
```

and the tests pass.

How would that look like if we would use [[hypothesis]] for property based testing?

```python
# Third party

import hypothesis.strategies as s

from hypothesis import given

# First party

from factorize import factorize

@given(s.integers(min_value=-(10 ** 6), max_value=10 ** 6))

def test_factorize_multiplication_property(n):

"""The product of the integers returned by factorize(n) needs to be n."""

factors = factorize(n)

product = 1

for factor in factors:

product *= factor

assert product == n, f"factorize({n}) returned {factors}"
```

when we run the tests now they fail.

### Where can I apply property-based testing?

This kind of pattern works for quite a couple of algorithms where verification is cheap:

-   [Arg max](https://en.wikipedia.org/wiki/Arg_max): Iterate over the list and ensure that no other element is larger.
-   Solving a set of equations: Verify that the solution is actually a solution.
-   [Constraint satisfaction](https://en.wikipedia.org/wiki/Constraint_satisfaction): Verify that the solution satisfies all constraints.
-   All [NP complete problems](https://en.wikipedia.org/wiki/NP-completeness): This is a set of decision problems where it is hard to find an answer, but easy to verify a found answer. An example is the traveling salesman. Given a set of cities which the salesman has to visit, is there a tour he can take which has a length of at most L? Given such a tour, it is easy to verify. Computing such a tour can be hard, though.

Weaker, but still helpful are checks which verify if the returned value is in the set of candidates:

-   [Greatest common divisor](https://en.wikipedia.org/wiki/Greatest_common_divisor): Ensure that it actually **is a** divisor.
-   Shortest path: Ensure it **is a** path.
-   Sorting and ranking: Ensure exactly the same elements are in the list as before. Maybe you can also test for the sorting/ranking criterion?
-   Filtering: Assert that the relevant data is still there / that the other data was removed.






___
# References
[Property-based Testing in Python with Hypothesis | Level Up Coding](https://levelup.gitconnected.com/unit-testing-in-python-property-based-testing-892a741fc119)