# Py Password Manager

[![Build Status](https://travis-ci.org/patarapolw/pypwm.svg?branch=master)](https://travis-ci.org/patarapolw/pypwm)
[![PyPI version shields.io](https://img.shields.io/pypi/v/pypwm.svg)](https://pypi.python.org/pypi/pypwm/)
[![PyPI license](https://img.shields.io/pypi/l/pypwm.svg)](https://pypi.python.org/pypi/pypwm/)
[![PyPI pyversions](https://img.shields.io/pypi/pyversions/pypwm.svg)](https://pypi.python.org/pypi/pypwm/)
[![PyPI status](https://img.shields.io/pypi/status/pypwm.svg)](https://pypi.python.org/pypi/pypwm/)
[![Examples tested with pytest-readme](http://img.shields.io/badge/readme-tested-brightgreen.svg)](https://github.com/boxed/pytest-readme)

A library for password manager for Python

## Features

- Automatic vault locking and saving after predefined time (default 60 sec)
- Vault file generation
- Passcode lock with RSA (based on PyCryptodome)


## Installation

```commandline
pip install pypwm
```
or
```commandline
pipenv install -e git+https://github.com/patarapolw/pypwm.git#egg=pypwm
```

## Usage

```python
from pwm.vault import Vault

with Vault('amasterpassword') as vault:
    vault['reddit'] = {
        'password': 'averycomplexpassword'
    }
with Vault('amasterpassword') as vault:
    print(vault['reddit']['password'])
```

## Real-world usage

```python

do_exit = False

while not do_exit:
    try:
        while True:
            try:
                vault = Vault(getpass('Please enter the master password : '))
                break
            except ValueError:
                continue

        while not do_exit:
            print('Password available for:', ', '.join(dict(vault).keys()))
            name = input('Please type the name of password to view or create a new one, or press q to exit. : ')
            if name == 'q':
                do_exit = True
                break

            new_entry = dict(vault).get(name, {
                'password': generate_password(),
            })
            print(new_entry)
            if input('Do you want to save? Press [y/Y] to save: ').lower() == 'y':
                vault[name] = new_entry

        vault.close()

    except AttributeError:
        continue
```

## Found in

- https://github.com/patarapolw/memorable-password
