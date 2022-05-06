If we want to pass the arguments from command line in `pytest`, we can use the hook function in `conftest.py` called `pytest_addoption` and use request fixture. The `pytest_addoption` hook passed a `parser` object which can be used to add as many command-line arguments as we want as `parser.addoption(…)`

> [From the pytest documentations](https://docs.pytest.org/en/latest/reference/reference.html?highlight=addoption#_pytest.hookspec.pytest_addoption), To add command line options, call `parser.addoption(...)`. To add ini-file values call `parser.addini(...)`

For example:

`conftest.py`

```
import pytest


def pytest_addoption(parser):
    parser.addoption("--value1", action="store", default="default value1")
    parser.addoption("--value2", action="store", default="default value2")



@pytest.fixture
def value1(request):
    return request.config.getoption("--value1")


@pytest.fixture
def value2(request):
    return request.config.getoption("--value2")
```

And the `test_value.py` will be 

```
import pytest

@pytest.mark.unit
def test_value(value1, value2):
    print ("Displaying value1: %s" % value1)
    print("Displaying value2: %s" % value2)
```

And the output will be:
```
pytest test_value.py -- value1 abc -- value2 123
```

Displaying value1: abc
Displaying value2: 123
