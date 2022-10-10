222022-09-08

Style: 

Domain: #TestAutomation 

Tags:

# Mutation Testing
For those who are not familiar with it, mutation testing is a method of evaluating test quality by injecting bugs into the code and seeing whether the tests detect the fault or not. The more injected bugs the tests catch, the better they are. Here’s an example:

Negating the _if_ condition.
```python
def checkout(cart):
  if not cart.items:
    throw Error("cart empty")
  return checkout_internal(cart)

def checkout(cart):
  if cart.items:
    throw Error("cart empty")
  return checkout_internal(cart)

```

If a test fails, we say it kills the mutant, and if no tests fail, we say that the mutant is alive.

It is important for mutation testing taht we keeep the mutations to ones taht have benefit. There are many ways we could "mutate" our code, but a larger number of them would bring no benefit, and most likely would be harmful since they would lead us to not use mutation testing.

[![](https://1.bp.blogspot.com/-zdEPosZhI0o/YHDn654mV9I/AAAAAAAAAfI/TiZ1Y6bJn5kcnv4JvzFdGeInr8sTk_3vwCLcBGAsYHQ/w640-h421/Block%2BDiagram%2Bfor%2BMutation%2BTesting%2BArticle.jpg)](https://1.bp.blogspot.com/-zdEPosZhI0o/YHDn654mV9I/AAAAAAAAAfI/TiZ1Y6bJn5kcnv4JvzFdGeInr8sTk_3vwCLcBGAsYHQ/s711/Block%2BDiagram%2Bfor%2BMutation%2BTesting%2BArticle.jpg)

Each event during the Code review is announced using a publisher-subscriber pattern, and any interested party can listen, and react, to these events. When a change is sent for Code review, many things happen: linters are run, automated tests are evaluated, coverage is calculated, and for the users of mutation testing, mutants are generated and evaluated. Listening on all events coming from the Code review system, the _listener_schedules a mutation run on the _analyzer_. 

The first thing the analyzer does is get the code coverage results for the patch in question; from it, the analyzer can extrapolate which tests cover which lines of source code. This is a very useful piece of information, because running the minimum set of tests that can kill a mutant is crucial; if we just ran all tests that were linked in, or covered the project, that would be prohibitively computationally expensive

Next, for each covered line in each file in the patch, a _mutagenesis server_ for the language in question is asked to produce a mutant. The mutagenesis server parses the file, traverses its AST, and applies the mutators in the requested order (as per mutation context), ignoring arid nodes, nodes in uncovered lines and in lines that are not affected by the proposed patch.

## Fault Coupling

Mutation testing is only valuable if the test cases we write for mutants are valuable. [Mutants do not resemble real bugs](https://ieeexplore.ieee.org/document/6982626), they are simpler than bugs found in the wild. Mutation testing relies on the coupling hypothesis: [mutants are coupled with real bugs](https://dl.acm.org/doi/abs/10.1145/2635868.2635929) if a test suite that is sensitive enough to detect mutants is also sensitive enough to detect the more complex real bugs. Reporting mutants and writing tests to kill them only makes sense if they are coupled with real bugs.

When the _analyzer_ assembles all mutants, it patches them one by one to a version control context and then evaluates all the tests for each mutant in parallel. For mutants for which all tests pass, the _analyzer_ surfaces a finding for the code author and reviewers, and is done for the time being.


___
# References
[Google Testing Blog: Mutation Testing](https://testing.googleblog.com/2021/04/mutation-testing.html)