# ![logo](logo-s.png) DataFlows

[![Travis](https://img.shields.io/travis/datahq/dataflows/master.svg)](https://travis-ci.org/datahq/dataflows)
[![Coveralls](http://img.shields.io/coveralls/datahq/dataflows.svg?branch=master)](https://coveralls.io/r/datahq/dataflows?branch=master)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/dataflows.svg)
[![Gitter chat](https://badges.gitter.im/dataflows-chat/Lobby.png)](https://gitter.im/dataflows-chat/Lobby)

DataFlows is a simple and intuitive way of building data processing flows.

- It's built for small-to-medium-data processing - data that fits on your hard drive, but is too big to load in Excel or as-is into Python, and not big enough to require spinning up a Hadoop cluster...
- It's built upon the foundation of the Frictionless Data project - which means that all data produced by these flows is easily reusable by others.
- It's a pattern not a heavy-weight framework: if you already have a bunch of download and extract scripts this will be a natural fit

Read more in the [Features section below](#features).

## QuickStart 

Install `dataflows` via `pip install.`

Then use the command-line interface to bootstrap a basic processing script for any remote data file:

```bash

# Install from PyPi
$ pip install dataflows

# Inspect a remote CSV file
$ dataflows init https://raw.githubusercontent.com/datahq/dataflows/master/data/academy.csv
Writing processing code into academy_csv.py
Running academy_csv.py
academy:
#     Year           Ceremony  Award                                 Winner  Name                            Film
      (string)      (integer)  (string)                            (string)  (string)                        (string)
----  ----------  -----------  --------------------------------  ----------  ------------------------------  -------------------
1     1927/1928             1  Actor                                         Richard Barthelmess             The Noose
2     1927/1928             1  Actor                                      1  Emil Jannings                   The Last Command
3     1927/1928             1  Actress                                       Louise Dresser                  A Ship Comes In
4     1927/1928             1  Actress                                    1  Janet Gaynor                    7th Heaven
5     1927/1928             1  Actress                                       Gloria Swanson                  Sadie Thompson
6     1927/1928             1  Art Direction                                 Rochus Gliese                   Sunrise
7     1927/1928             1  Art Direction                              1  William Cameron Menzies         The Dove; Tempest
...

# dataflows create a local package of the data and a reusable processing script which you can tinker with
$ tree
.
├── academy_csv
│   ├── academy.csv
│   └── datapackage.json
└── academy_csv.py

1 directory, 3 files

# Resulting 'Data Package' is super easy to use in Python
[adam] ~/code/budgetkey-apps/budgetkey-app-main-page/tmp (master=) $ python
Python 3.6.1 (default, Mar 27 2017, 00:25:54)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.42.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from datapackage import Package
>>> pkg = Package('academy_csv/datapackage.json')
>>> it = pkg.resources[0].iter(keyed=True)
>>> next(it)
{'Year': '1927/1928', 'Ceremony': 1, 'Award': 'Actor', 'Winner': None, 'Name': 'Richard Barthelmess', 'Film': 'The Noose'}
>>> next(it)
{'Year': '1927/1928', 'Ceremony': 1, 'Award': 'Actor', 'Winner': '1', 'Name': 'Emil Jannings', 'Film': 'The Last Command'}

# You now run `academy_csv.py` to repeat the process
# And obviously modify it to add data modification steps
```

## Features

* Trivial to get started and easy to scale up
* Set up and run from command line in seconds ...
    * `dataflow init` => `flow.py`
    * `python flow.py`
* Validate input (and esp source) quickly (non-zero length, right structure, etc.)
* Supports caching data from source and even between steps
    * so that we can run and test quickly (retrieving is slow)
* Immediate test is run: and look at output ...
    * Log, debug, rerun
* Degrades to simple python
* Conventions over configuration
* Log exceptions and / or terminate
* The input to each stage is a Data Package or Data Resource (not a previous task)
	* Data package based and compatible
* Processors can be a function (or a class) processing row-by-row, resource-by-resource or a full package
* A pre-existing decent contrib library of Readers (Collectors) and Processors and Writers

## Learn more

Dive into the [Tutorial](TUTORIAL.md) to get a deeper glimpse into everything that `dataflows` can do.
Also review this list of [Built-in Processors](PROCESSORS.md), which also includes an API reference for each one of them.
