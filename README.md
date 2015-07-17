SLAMon Python Agent
===================
[![License][license]](http://www.apache.org/licenses/LICENSE-2.0)

[![Latest PyPI Version](https://badge.fury.io/py/slamon-agent.svg)](http://badge.fury.io/py/slamon-agent)
[![Supported Python Versions](https://img.shields.io/pypi/pyversions/slamon-agent.svg)](pypi)
[![Requirements Status][requirements_img]](requirements)

[![Build Status][ci_status]](https://travis-ci.org/SLAMon/slamon-python-agent.svg?branch=master)
[![Coverage Status][coveralls]](https://coveralls.io/github/SLAMon/slamon-python-agent?branch=master)
[![Code Health][codehealth]](https://landscape.io/github/SLAMon/slamon-python-agent/master)

Python implementation of [SLAMon](https://github.com/SLAMon/SLAMon) agent.

Prerequisites
-------------

Required libraries for running the agent:

    $ pip install requests
    $ pip install python-dateutil

Required libraries for running the tests:

    $ pip install responses
    
Installing the agent
--------------------

To easily install the agent with all required dependencies and the *slamon-agent* command line utility, use e.g. pip.

    $ pip install slamon-agent

Running the agent
-----------------

There are two ways for running the agent: a command line script and embedding agent in your own script.

### Running the agent using command line script ###

*slamon-agent* command line utility is the simplest way running the agent. Agent Fleet Manager (AFM) Url, 
concurrent executor count and modules to load handlers from are defined as command line arguments as follows:

    $ slamon-agent --help
    usage: slamon-agent-script.py [-h] -u URL [-l LOAD] [-w DEFAULT_WAIT]
                                  [-x NUM_EXECUTORS]

    optional arguments:
      -h, --help            show this help message and exit
      -u URL, --url URL     Coordinator URL
      -l LOAD, --load LOAD  Load handlers from specified module or package e.g. slamon_agent.handlers
      -w DEFAULT_WAIT, --default-wait DEFAULT_WAIT
                            Seconds to wait before reconnection after connection
                            failure.
      -x NUM_EXECUTORS, --num-executors NUM_EXECUTORS
                            Number of concurrent task executors.
    
Starting an agent with only the wait task handler.

    $ slamon-agent -u http://my_afm.url.com -l slamon_agent.handlers.wait_task_handler

### Running the agent from python script ###

```python
from slamon_agent.handlers import *
from slamon_agent import Agent
agent = Agent('http://localhost:8080')
agent.run()
```

Note that when running agent in a script, you need to explicitly import
the modules you want to load task handlers from:

```python
from my_custom_handler_package import *
import my_custom_handler_module

from slamon_agent import Agent
agent = Agent('http://localhost:8080')
agent.run()
```


Developing task handlers
------------------------

Developing task handler is attempted to make as easy as possible. The agent
core application will take care of the communication between an agent and the AFM, so that
the task handler can be AFM agnostic and purely focus on just
executing the task.
 
Task handlers are simple functions that take input data as dictionary parameter,
do their work synchronously and return response data as a dictionary.

Task handlers are registered using `@TaskHandler` decorator.

```python
from slamon_agent.handlers import TaskHandler

@TaskHandler("wait", 1)
def wait_task_handler(input_params):
   import time, random
   timeout = float(input_params['time']) - 0.5 + random.random()
   return {'time': timeout}
```

[license]: https://img.shields.io/:license-Apache%20License%20v2.0-blue.svg
[ci_status]: https://travis-ci.org/SLAMon/slamon-python-agent.svg?branch=master
[coveralls]: https://coveralls.io/repos/SLAMon/slamon-python-agent/badge.svg?branch=master&service=github
[codehealth]: https://landscape.io/github/SLAMon/slamon-python-agent/master/landscape.svg?style=flat
[latest_version]: https://badge.fury.io/py/slamon-agent.svg
[pypi]: https://pypi.python.org/pypi/slamon-agent/
[requirements_img]: https://requires.io/github/SLAMon/slamon-python-agent/requirements.svg?branch=master
[requirements]: https://requires.io/github/SLAMon/slamon-python-agent/requirements/?branch=master