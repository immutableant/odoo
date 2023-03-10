Odoo
====

Example repository for Dolmen Consulting Group
---

### Install WSL(Windows Subsystem for Linux)

The Windows Subsystem for Linux (WSL) lets developers install a Linux distribution (such as Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc) and use Linux applications, utilities, and Bash command-line tools directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.

The default distribution of Linux for WSL is Ubuntu, this is the one needed.

https://learn.microsoft.com/en-us/windows/wsl/install

Recommended tool: [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us&rtc=1)

### Install PostgreSQL in WSL
Odoo needs a local PostgreSQL server running in order to work locally.

To install one use the following commands.
```Bash
# These two commands just updates and upgrades your WSL distribution.
# This makes sure you get the latest versions of the software we will install.
sudo apt update -y
sudo apt upgrade -y

# This installs the posgresql server.
sudo apt install postgresql postgresql-client

# Creates a user db super user (only do this for your local).
sudo su -c "createuser -s $USER" postgres

# This commands starts the postgresql server, this command needs to be run every time you open the WSL terminal.
sudo service postgresql start

# If you need to check if postgresql is running use this commad.
sudo service postgresql status

# If you want to stop postgresql use this command.
sudo service postgresql stop
```

### Installing Odoo system dependencies in WSL
In order to compile and run Odoo we need to install some system dependencies.

Here is the commands to install them.
```bash
# These two commands just updates and upgrades your WSL distribution.
# This makes sure you get the latest versions of the software we will install.
# If you have run these commands recently from the postgresql installation, you can skip them, but is usually a good idea to run these everytime you open a terminal.
sudo apt update -y
sudo apt upgrade -y

# This will install python, git and all needed libraries.
sudo apt install git python3-dev python3-pip python3-wheel python3-venv build-essential libpq-dev libxslt-dev libzip-dev libldap2-dev libsasl2-dev libssl-dev -y
```

### Fork Odoo repository
We will need to fork the Odoo repository.

*needs more instructions*


### Install Odoo from source
In order to do this install we need to create a folder and then use git to get the source code.

```bash
# This will create a src directory for our source dode in our home directory. ~/ == /home/username
mkdir ~/src
cd ~/src

# This will clone your fork from your github repository.
git clone git@github.com:immutableant/odoo.git -b 15.0 --depth=1
```

### Branching
With the git command we just runned we cloned into our computer the branch `15.0`. 

We need to create a new branch from this branch in order to have a main branch that we can rebase when we need to get the latest updates from this branch.

This branch naming connvention should be something like `company-name-main` or `project-name-main`. For this example I will be using `dolmen-main`.

Use the following commands to create this branch:
```bash
git checkout -b dolmen-main
```
 This will create the branch and also checkout the branch for our next step.

### Setting up Virtual Environment to prep for running Odoo
Virtual environments is a tool that will allow us to install the Python libraries from requirements.txt in a private environment for our local Odoo install.

This enviroments needs to be activated every time we open a new terminal window and want to run Odoo.

To use virtual environments run the following commands:
```bash
# This creates a virtual environment in /home/username/src/venv15
python3 -m venv ~/src/venv15

# Use this command to activate this environment
source ~/src/venv15/bin/activate
```

You should see that your terminal prompt change to display `(venv15)` at the start of it. Like this: `(venv15) username@MACHINE-ID:~/src/odoo$ `.

To deactivate this you can run :
```bash
deactivate
```

Make sure you have your Virtual Environment activated and then run:
```bash
# Updates pip
pip3 install -U pip

# Install setuptools and wheel
pip3 install setuptools wheel

# Install Odoo python requirements
pip3 install -r ~/src/odoo/requirements.txt

# Installs Odoo
pip3 install -e ~/src/odoo
```

### Running Odoo
*Still needs work*

*Original README*
______________

[![Build Status](https://runbot.odoo.com/runbot/badge/flat/1/master.svg)](https://runbot.odoo.com/runbot)
[![Tech Doc](https://img.shields.io/badge/master-docs-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/documentation/15.0)
[![Help](https://img.shields.io/badge/master-help-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/forum/help-1)
[![Nightly Builds](https://img.shields.io/badge/master-nightly-875A7B.svg?style=flat&colorA=8F8F8F)](https://nightly.odoo.com/)

Odoo
----

Odoo is a suite of web based open source business apps.

The main Odoo Apps include an <a href="https://www.odoo.com/page/crm">Open Source CRM</a>,
<a href="https://www.odoo.com/app/website">Website Builder</a>,
<a href="https://www.odoo.com/app/ecommerce">eCommerce</a>,
<a href="https://www.odoo.com/app/inventory">Warehouse Management</a>,
<a href="https://www.odoo.com/app/project">Project Management</a>,
<a href="https://www.odoo.com/app/accounting">Billing &amp; Accounting</a>,
<a href="https://www.odoo.com/app/point-of-sale-shop">Point of Sale</a>,
<a href="https://www.odoo.com/app/employees">Human Resources</a>,
<a href="https://www.odoo.com/app/social-marketing">Marketing</a>,
<a href="https://www.odoo.com/app/manufacturing">Manufacturing</a>,
<a href="https://www.odoo.com/">...</a>

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured <a href="https://www.odoo.com">Open Source ERP</a> when you install several Apps.

Getting started with Odoo
-------------------------

For a standard installation please follow the <a href="https://www.odoo.com/documentation/15.0/administration/install.html">Setup instructions</a>
from the documentation.

To learn the software, we recommend the <a href="https://www.odoo.com/slides">Odoo eLearning</a>, or <a href="https://www.odoo.com/page/scale-up-business-game">Scale-up</a>, the <a href="https://www.odoo.com/page/scale-up-business-game">business game</a>. Developers can start with <a href="https://www.odoo.com/documentation/15.0/developer/howtos.html">the developer tutorials</a>
