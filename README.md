
# Odoo - Example repository for Dolmen Consulting Group


## Install WSL(Windows Subsystem for Linux)

The Windows Subsystem for Linux (WSL) lets developers install a Linux distribution (such as Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc) and use Linux applications, utilities, and Bash command-line tools directly on Windows, unmodified, without the overhead of a traditional virtual machine or dualboot setup.

The default distribution of Linux for WSL is Ubuntu, this is the one needed.

https://learn.microsoft.com/en-us/windows/wsl/install

Recommended tool: [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us&rtc=1)
</br>
</br>
## Install PostgreSQL in WSL
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
</br>

## Installing Odoo system dependencies in WSL
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
</br>

## Fork Odoo repository
We will need to fork the Odoo repository.

*needs more instructions*

</br>

## Install Odoo from source
In order to do this install we need to create a folder and then use git to get the source code.

```bash
# This will create a src directory for our source dode in our home directory. ~/ == /home/username
mkdir ~/src
cd ~/src

# This will clone your fork from your github repository.
git clone git@github.com:immutableant/odoo.git -b 15.0 --depth=1
```
</br>

## Branching
With the git command we just runned we cloned into our computer the branch `15.0`. 

We need to create a new branch from this branch in order to have a main branch that we can rebase when we need to get the latest updates from this branch.

This branch naming connvention should be something like `company-name-main` or `project-name-main`. For this example I will be using `dolmen-main`.

Use the following commands to create this branch:
```bash
git checkout -b dolmen-main
```
 This will create the branch and also checkout the branch.

</br>

## Setting up Virtual Environment to prep for running Odoo
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
</br>

## Running Odoo
First lets make sure Odoo is available by checking it's version wiht the following commnad, *make sure the virtual environment is activated first*
```bash
odoo --version
```
This should return: `Odoo Server 15.0`

We can then move to the next step which is to create the database in Odoo. 

There are two ways of doing of creating the database:
- Via the web client
- Via Command line

For this guide we are gonna use the Command line way of creating the database, but I will also explain how to do so via the web client after.

</br>

### **Command line database creation**
As a developer it is more convenient to create a database from the command line.

To create it this way run the following command:
```bash
odoo -d odoo-15-local-db --stop-after-init
```
This command might take a couple of minutes while to finish. This will create a database named `odoo-15-local-db`

We can ommit the option `--stop-after-init` if we want to have the instance run after database creation.

This data base will start with demo data by default, this can be avoided by adding this option to the end `--without-demo=all`

Once the database creation has finished we can then run Odoo with the following command:
```bash
odoo
```
After running this go to your browser and go to this url: `http://localhost:8069`. You should see the login screen of your local instance. The default administrator username and password is `admin`.

With this we now have a local developer environment for Odoo 15.

If you want to terminate the instance go to the terminal and press `Ctrl + C`

</br>


### **Web client database creation**
This method creates the database when you run Odoo and access it for the first time. **This only works if there is no Odoo database already created.**
Run the following command:
```bash
odoo
```
This should run your Odoo instance, you'll see the instance startup logs scrolling in the terminal.

If you need to stop the install press `Ctrl + C` in the terminal and this will terminate it

Next, you want to access this url `http://localhost:8069`

It should load the create database wizard. In it you'll be given a random password for your local database or you can replace it with one of your choice, *make sure to remember this password as you'll needed to access your database*

You'll also need to:

- Enter a database name, ex: localOdooDB

- Enter an Email address for the for the defaul administrator. Does not have to be an actual email. The default value is `admin`

- Enter a password for the Admin, this is different than the passsword for the database and needs you also need to remember this value.

- Select a language for the the database.

- Select the country for the company defaults settings.

- The Demo Data checkbox allows you install demo data in the instance so you can have some preloaded data on it instead of starting from scratch.

Click the create database button to create the database. This can take a couple of minutes to complete. Once completed you'll get redirected to the login screen.

</br>

## Other handy information

### Database manager
You can access the database manager via this url: `http://localhost:8069/web/database/manager`

### Database listing
Use this commmand to list databases:
```bash
psql -l
```

### Create a copy of your database
Run this command:
```bash
createDB --template=odoo-15-local-db odoo-15-local-db-copy
```

### Database deletion(or recreation)
Use the following command to delete your database:
```bash
#### REMEMBER THIS CANNOT BE REVERTED SO ONLY RUN IF YOU ARE SURE YOU WANT TO DELETE IT ###
dropdb odoo-15-local-db
```

If you want to recreate the database you could run this to delete and recreate:
```bash
dropdb odoo-15-local-db && odoo -d odoo-15-local-db --stop-after-init
```

</br>

### Recommended software
- VS Code. A light weight, modular, and free IDE. [link](https://code.visualstudio.com/)

</br>

### Recommended reading material and source for this mini guide
- Odoo 15 Development Essentials. [link](https://www.packtpub.com/product/odoo-15-development-essentials-fifth-edition/9781800200067)


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
