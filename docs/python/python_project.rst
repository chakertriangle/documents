Python Project
==============

Typical Structure
-----------------
::

    foobar
    |__docs
    |         |__conf.py
    |         |__quickstart.rst
    |         |__index.rst
    |__foobar
    |          |__ __init__.py
    |          |__xxx.py
    |          |__tests
    |                   |__ __init__.py
    |                   |__ test_xxx.py
    |__tools
    |__data
    |__setup.py
    |__ReadMe.rst
    |__requirements.txt   # the dependencies


virtualenv
----------

virtualenv + virtualenvwrapper::

    sudo pip install virtualenv
    sudo pip install virtualenvwrapper

.. see also: [DOC] "Python Flask" => "Flask Beginner"

[MAC OS] pyenv + pyenv-virtualenv::

    brew install pyenv
    brew install pyenvbrew install pyenv

    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="$PYENV_ROOT/bin:$PATH"
    if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi

    pyenv install -l
    pyenv install 3.5.2
    pyenv rehash

    在任意目录， 如果想在这个目录及其子目录中使用不同的python版本
    pyenv local 3.5.2
    python --version  #should be 3.5.2
    pyenv virtualenv 3.5.2 venv3
    pyenv shell venv3
    pyenv activate venv3

    pyenv virtualenv-delete venv3       # delete the virtualenv
    pyenv <tab>                         # to show all sub commands

    不建议使用 pyenv global， 可能对全局其他程序有影响。

    pyenv:  安装切换不同的python版本，但是同一个版本还是共用一个python环境
    virtualenv:   创建不同的python环境 （--python指定基于哪一个python版本，要求系统中先装好了对应版本）
    virtualenvwrapper: 更加方便地创建和管理不同的virtualenv。
    pyenv-virtualenv: 把virtualenv集成到pyenv中， 同时自带virtualenvwrapper 和autoenv 类似高阶管理功能。

    如果系统已经自带多个python版本， 比如ubuntu16.04 自带python2 和python3,
    或许virtualenv + virtualenvwrapper 足够。
    如果还需要自己安装管理多个python版本，python环境, 比如 MacbootPro 2015上面，
    那么 pyenv+pyenv-virtualenv 可能更适合。

Coding Style
------------

- https://www.python.org/dev/peps/pep-0008/
- http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html

PEP8 online: http://www.pep8online.com/


**pylint**:

https://www.pylint.org/

https://pylint.readthedocs.io/en/latest/

sudo apt-get install pylint

There are 5 kind of message types :
* (C) convention, for programming standard violation
* (R) refactor, for bad code smell
* (W) warning, for python specific problems
* (E) error, for much probably bugs in the code
* (F) fatal, if an error occurred which prevented pylint from doing

Examples::

    pylint --generate-rcfile >pylint.conf
    pylint --rcfile=pylint.conf  xxx.py
    pylint --reports=n xxx.py

**pep8(pycodestyle)**:

https://pypi.python.org/pypi/pep8

sudo pip install pep8, and run it like this: ``pep8 <source-file>``.

.. note::
    There is a pyflakes vim plugin: https://github.com/vim-scripts/pyflakes

**flake8**:

https://pypi.python.org/pypi/flake8

豆瓣内部要求使用这个, Include Pyflakes + Pycodestyle + McCabe ... + other plugins。

Code complexity::

    flake8 --max-complexity 10 app.py  (#It's better not greater than 10)



**PEP8 Auto-convert tool**:

- autopep8: https://github.com/hhatto/autopep8
- Google python: https://github.com/google/yapf

 
Configuration
-------------

#. Use a config.py file or the config module
#. Module "configparser" for ini configuration file
#. Module "json" for json configuration file
#. Module "csv" for csv configuration file
#. Module "xml.etree.ElementTree " for xml configuration file

Examples for json::

    with open('comments.json', 'r') as f: # see also json.load()
        comments = json.loads(f.read())

    with open('comments.json', 'w') as f: # see also json.dump()
        f.write(json.dumps(comments, indent=4, separators=(',', ': ')))


Unittest
--------

Module "unittest" for unit tests.

See also nose:
- http://nose.readthedocs.org/en/latest/
- http://pythontesting.net/framework/nose/nose-introduction/


Documentation
-------------

Tips:

#. Use some formal template of document
#. Simple sentences
#. One topic for each paragraph
#. More use of present tenses
#. Use real codes as the examples.
#. Good code is always better than many documents.

HowTO:

1. Document project: sphinx + reStructuredText
2. doctest: https://docs.python.org/2/library/doctest.html
3. pydoc: https://docs.python.org/2/library/pydoc.html
4. epydoc tool: http://epydoc.sourceforge.net/

Epydoc is a tool for generating API documentation for Python modules, 
based on their docstrings. For an example of epydoc's output, see the
`API documentation for epydoc itself`_.

A lightweight markup language called epytext can be used to format
docstrings, and to add information about specific fields, such as
parameters and instance variables. Epydoc also understands docstrings

.. _API documentation for epydoc itself: http://epydoc.sourceforge.net/api/
.. _reStructuredText: http://epydoc.sourceforge.net/othermarkup.html


License
-------

The lice tool: https://github.com/licenses/lice

::

    lice [opts ...]  ('afl3', 'agpl3', 'apache', 'bsd2', 'bsd3', 'cc0',
    'cc_by', 'cc_by_nc', 'cc_by_nc_nd', 'cc_by_nc_sa', 'cc_by_nd',
    'cc_by_sa', 'cddl', 'epl', 'gpl2', 'gpl3', 'isc', 'lgpl', 'mit',
    'mpl', 'wtfpl', 'zlib')


Package
-------

Module 'setuptools'


Continuous Integration
----------------------

buildbot
~~~~~~~~

http://buildbot.net/

Quick start::

    sudo pip install buildbot
    sudo pip install buildbot-www
    sudo pip install buildbot-waterfall_view
    sudo pip install buildbot-console_view
    sudo pip install buildbot-worker

    buildbot create-master /tmp/experimental_buildmaster
    cp /tmp/experimental_buildmaster/master.cfg.sample /tmp/experimental_buildmaster/master.cfg
    buildbot upgrade-master /tmp/experimental_buildmaster   # each time when change the conf file

    # Check the worker name and password from master.cfg
    buildbot-worker create-worker /tmp/experimental_buildslave localhost:9989 example-worker pass
    buildbot-worker start /tmp/experimental_buildslave

    firefox http://localhost:8010/ &
    Builds => runtests => force => Input "Your name"(email) and click "Start Build"

Jenkins
~~~~~~~

https://jenkins.io/index.html


Project Template
----------------

https://github.com/audreyr/cookiecutter

Create new project base on a template::
    
    pip install cookiecutter
    cookiecutter https://github.com/sloria/cookiecutter-flask.git

All templates here:
https://github.com/audreyr/cookiecutter#a-pantry-full-of-cookiecutters


Other code check tool
---------------------
https://wiki.python.org/moin/PythonTestingToolsTaxonomy

**pychecker**:

http://pychecker.sourceforge.net/

Install: ``sudo apt-get install pychecker``

**clonedigger**:

Clone Digger aimed to detect similar code in Python and Java programs.
The synonyms for the term "similar code" are "clone" and "duplicate code". 

http://clonedigger.sourceforge.net/

Install: ``sudo pip install clonedigger``

Examples::

    clonedigger source_file_1 source_file_2 ...
    clonedigger path_to_source_tree

**coverage**:

- https://pypi.python.org/pypi/coverage
- http://coverage.readthedocs.io/en/latest/

Install: ``sudo pip install coverage``

Examples::

    /usr/local/bin/coverage run test.py  # the output is in file "./.coverage" by default
    /usr/local/bin/coverage report -m    # line range show the code which is not covered.
    /usr/local/bin/coverage html         # the outputs are in folder "./htmlcov" by default
    firefox htmlcov/index.html


**profile**:

https://docs.python.org/3/library/profile.html

Examples::

    python -m cProfile test.py


**pymetrics**:
The complexity test tool

http://www.traceback.org/2008/03/31/measuring-cyclomatic-complexity-of-python-code/

/tmp/pymetrics-0.8.1/pymetrics ./send-pull-request


Command-line interfaces
-----------------------

**argparse**:

https://docs.python.org/3/library/argparse.html#module-argparse

**docopt**:

| https://github.com/docopt/docopt
| http://docopt.org/
|
