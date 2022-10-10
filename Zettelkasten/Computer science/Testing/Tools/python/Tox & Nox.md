222022-09-15

Style: 

Domain: #TestAutomation 

Tags:

# Tox & Nox

### How to use tox

tox uses a `tox.ini` file which is in the package root directory. So your project structure might look like this:

your-awesome-project/            # The git repository  
├── README.md  
├── setup.py                     # Dependencies / Package Meta data  
├── your_awesome_package/        # Code of the package  
│   ├── a_module.py  
│   └── another_module.py  
├── tests/                       # Unit tests  
│   ├── test_a_module.py  
│   └── test_another_module.py  
└── tox.ini                      # Why you're reading this article

The `tox.ini` file to run the tests in an isolated Python environment for Python 3.6, Python 3.7 and Python 3.8 looks like this:

```ini
[tox]

envlist = linter,py36,py37,py38

[testenv]

deps =
-r requirements-dev.txt

commands =
pip install -e .[all]
pytest .

[testenv:linter]

deps =
flake8
flake8-bugbear
flake8-builtins
flake8-comprehensions
flake8-string-format
black
pydocstyle

commands =
flake8
black --check .
pydocstyle```


### Now… what is nox?

[nox](https://nox.thea.codes/en/stable/) is a spin-off of tox. Instead of using a `tox.ini` configuration file, it uses a `noxfile.py` Python file. It’s pretty similar to tox, but more flexible as it uses Python code:

```python
# Third party

import nox

@nox.session(python=["3.6", "3.7", "3.8"])

def test(session):
session.install(".[all]")
session.install("-r", "requirements-dev.txt")
session.run("pytest")

@nox.session(python="3.8")
def lint(session):
session.install("-r", "requirements-lint.txt")
session.run("flake8", ".")
session.run("black", ".", "--check")
```


___
# References
[Unit Testing in Python — tox and nox | by Martin Thoma | Python in Plain English](https://python.plainenglish.io/unit-testing-in-python-tox-and-nox-833e4bbce729)