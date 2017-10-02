[![Build Status](https://travis-ci.org/marvin-ai/marvin-python-toolbox.svg)](https://travis-ci.org/marvin-ai/marvin-python-toolbox) [![codecov](https://codecov.io/gh/marvin-ai/marvin-python-toolbox/branch/master/graph/badge.svg)](https://codecov.io/gh/marvin-ai/marvin-python-toolbox)

# Marvin Toolbox v0.0.1

## Overview

The toolbox is a set of utilities and CLI commands intended to help the data scientist on the mission of building production-ready models.

## Installation

```
sudo apt-get install libsasl2-dev python-pip graphviz -y

sudo pip install --upgrade pip
sudo pip install virtualenvwrapper

cd /tmp
wget https://d3kbcqa49mib13.cloudfront.net/spark-2.1.1-bin-hadoop2.6.tgz
tar -xf spark-2.1.1-bin-hadoop2.6.tgz
sudo mv spark-2.1.1-bin-hadoop2.6 /opt/spark

echo "export SPARK_HOME=/opt/spark" >> $HOME/.bashrc
echo "export WORKON_HOME=$HOME/.virtualenvs" >> $HOME/.bashrc
echo "export MARVIN_HOME=$HOME/marvin" >> $HOME/.bashrc
echo "export MARVIN_DATA_PATH=$HOME/marvin/data" >> $HOME/.bashrc
echo "source virtualenvwrapper.sh" >> $HOME/.bashrc

source ~/.bashrc

mkdir $MARVIN_HOME
cd $MARVIN_HOME

mkdir $MARVIN_DATA_PATH

git clone https://github.com/marvin-ai/marvin-python-toolbox.git
cd marvin-python-toolbox

mkvirtualenv marvin-python-toolbox-env
setvirtualenvproject

make marvin
marvin test
```


## CLI

### marvin

`marvin` is a command-line utility for development tasks.

#### Usage

```
 $ marvin [OPTIONS] [COMMAND] [ARGS]...
```

`COMMAND` should be one of the commands listed below.

### Builtin Commands

// TODO
 
Some projects can define new commands and blacklist some unuseful ones, but in general the above commands will be available most of the time.

### Configuration

#### configuration file - marvin.ini

To use the `marvin` command your project should have a `marvin.ini` file.

```
[marvin]
# The package name. Mandatory. 
# (the same name used when importing in python)
package = marvin_something              

# The package path. Optional. (default: {inidir}/{package})
packagedir = path                      

# The type of your project. Mandatory.
type = [lib|tool|python-engine] 

# The list of commands to be disabled. Optional.
exclude = list of commands, separated by commas             

# The file containing the project custom commands. Optional.
# (default: {inidir}/commands.py)
commandsfile = path                   
```

#### Globally available substitutions
 
 - `{inidir}`: The directory where marvin.ini is located
 
#### Custom substitutions

All options declared in the configuration file can be used as a substitution, following the schema `{section_option}`. For example:

 - `{marvin_package}`: The package name
 - `{marvin_type}`: The project type

### Creating project specific commands

// TODO


## Module marvin\_toolbox.management

### Creating manage.py using marvin\_toolbox

If you don't want to use directly the `marvin-manage` utility, you can create a `manage.py` file in your project (following the template below), make it executable using `chmod +x manage.py`, and then use all available builtin commands. 

Although it's possible to create new commands modifing the `manage.py` file, it's strongly recommended to use the `commands.py`. Otherwise the new commands will not be available to the `marvin-manage` utility.

#### manage.py example

```
#!/usr/bin/env python
# coding=utf-8

import os.path

from marvin_python_toolbox.management import create_cli

package_name = '[YOUR PACKAGE NAME]'  # Don't forget to replace *[YOUR PACKAGE NAME]* value.
package_type = 'lib'
package_path = os.path.join(os.path.dirname(__file__), package_name)

exclude_commands = ['runserver', 'notebook', 'example']

cli = create_cli(package_name, package_path, exclude=exclude_commands, type_=package_type)

# Create new command 
# (not recommended. use the commands.py file)
@cli.command('ping', help='Display "pong".')
def pong():
    print 'pong'

if __name__ == '__main__':
    cli()
```

## Logging

The default log level is set to _WARNING_. You can change the log level at runtime setting another value to one of the following environment variable: `MARVIN_TOOLBOX_LOG_LEVEL` or `LOG_LEVEL`. The available values are _CRITICAL_, _ERROR_, _WARNING_, _INFO_ and _DEBUG_.

Be careful using `LOG_LEVEL`, it may affect another lib.

> Marvin is a project started at B2W Digital offices and released open source on September 2017.
