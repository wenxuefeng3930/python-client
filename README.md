# Appium Python Client

[![PyPI version](https://badge.fury.io/py/Appium-Python-Client.svg)](https://badge.fury.io/py/Appium-Python-Client)
[![Downloads](https://pepy.tech/badge/appium-python-client)](https://pepy.tech/project/appium-python-client)

[![Build Status](https://dev.azure.com/AppiumCI/Appium%20CI/_apis/build/status/appium.python-client?branchName=master)](https://dev.azure.com/AppiumCI/Appium%20CI/_build/latest?definitionId=56&branchName=master)

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

An extension library for adding [WebDriver Protocol](https://www.w3.org/TR/webdriver/) and Appium commands to the Selenium Python language binding for use with the mobile testing framework [Appium](https://appium.io).

## Notice

Since **v1.0.0**, only Python 3 is supported.

Since **v2.0.0**, base selenium client version is v4.
The version only works W3C WebDriver protocol format.
If you would like to use old protocol (MJSONWP), please use v1 Appium Python cleint.

## Getting the Appium Python client

There are three ways to install and use the Appium Python client.

1. Install from [PyPi](https://pypi.org), as
['Appium-Python-Client'](https://pypi.org/project/Appium-Python-Client/).

    ```shell
    pip install Appium-Python-Client
    ```

    You can see the history from [here](https://pypi.org/project/Appium-Python-Client/#history)

2. Install from source, via [PyPi](https://pypi.org). From ['Appium-Python-Client'](https://pypi.org/project/Appium-Python-Client/),
download and unarchive the source tarball (Appium-Python-Client-X.X.tar.gz).

    ```shell
    tar -xvf Appium-Python-Client-X.X.tar.gz
    cd Appium-Python-Client-X.X
    python setup.py install
    ```

3. Install from source via [GitHub](https://github.com/appium/python-client).

    ```shell
    git clone git@github.com:appium/python-client.git
    cd python-client
    python setup.py install
    ```

## Usage

The Appium Python Client is fully compliant with the WebDriver Protocol
with some helpers to make mobile testing in Python easier.

To use the new functionality now, and to use the superset of functions, instead of
including the Selenium `webdriver` module in your test code, use that from
Appium instead.

```python
from appium import webdriver
```

From there much of your test code will work with no change.

As a base for the following code examples, the following sets up the [UnitTest](https://docs.python.org/3/library/unittest.html)
environment:

```python
# Android environment
import unittest
from appium import webdriver

desired_caps = dict(
    platformName='Android',
    platformVersion='10',
    automationName='uiautomator2',
    deviceName='Android Emulator',
    app=PATH('../../../apps/selendroid-test-app.apk')
)
self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
el = self.driver.find_element_by_accessibility_id('item')
el.click()
```

```python
# iOS environment
import unittest
from appium import webdriver

desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
el = self.driver.find_element_by_accessibility_id('item')
el.click()
```

## Direct Connect URLs

If your Selenium/Appium server decorates the new session capabilities response with the following keys:

- `directConnectProtocol`
- `directConnectHost`
- `directConnectPort`
- `directConnectPath`

Then python client will switch its endpoint to the one specified by the values of those keys.

```python
import unittest
from appium import webdriver

desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps, direct_connection=True)
```

## Relax SSL validation

`strict_ssl` option allows you to send commands to an invalid certificate host like self-certificated SSL.

```python
import unittest
from appium import webdriver

desired_caps = dict(
    platformName='iOS',
    platformVersion='13.4',
    automationName='xcuitest',
    deviceName='iPhone Simulator',
    app=PATH('../../apps/UICatalog.app.zip')
)

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps, strict_ssl=False)
```

## Documentation

- https://appium.github.io/python-client-sphinx/ is detailed documentation
- [functional tests](test/functional) also may help to see concrete examples.

## Development

- Code Style: [PEP-0008](https://www.python.org/dev/peps/pep-0008/)
  - Apply `black`, `isort` and `mypy` as pre commit hook
  - Run `make` command for development. See `make help` output for details
- Docstring style: [Google Style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
- `gitchangelog` generates `CHANGELOG.rst`

### Setup

- `pip install --user pipenv`
- `python -m pipenv lock --clear`
  - If you experience `Locking Failed! unknown locale: UTF-8` error, then refer [pypa/pipenv#187](https://github.com/pypa/pipenv/issues/187) to solve it.
- `python -m pipenv install --dev --system`
- `pre-commit install`

### Run tests

You can run all of tests running on CI via `tox` in your local.

```bash
$ tox
```

You also can run particular tests like below.

#### Unit

```bash
$ pytest test/unit
```

Run with `pytest-xdist`

```bash
$ pytest -n 2 test/unit
```

#### Functional

```bash
$ pytest test/functional/ios/search_context/find_by_ios_class_chain_tests.py
```

#### In parallel for iOS

1. Create simulators named 'iPhone 8 - 8100' and 'iPhone 8 - 8101'
2. Install test libraries via pip, `pip install pytest pytest-xdist`
3. Run tests

```bash
$ pytest -n 2 test/functional/ios/search_context/find_by_ios_class_chain_tests.py
```

## Release

Follow below steps.

```bash
$ pip install twine
$ pip install git+git://github.com/vaab/gitchangelog.git # Getting via GitHub repository is necessary for Python 3.7
# Type the new version number and 'yes' if you can publish it
# You can test the command with DRY_RUN
$ DRY_RUN=1 ./release.sh
$ ./release.sh # release
```

## License

Apache License v2
