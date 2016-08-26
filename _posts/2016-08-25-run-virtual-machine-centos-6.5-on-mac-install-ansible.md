---
layout:      post
title:       Run Virtual Machine (CentOS 6.5) on mac & Install Ansible
---
#### For installing vagrant refer [Vagrant Downloads](https://www.vagrantup.com/downloads.html)

Assuming that Vagrant is installed on your mac, we will run a virtual machine that runs CentOS 6.5 distribution of Linux on the mac.

From a working directory run:

* `$ vagrant init nrel/CentOS-6.5-x86_64` -- This step will take some time the first time. After completion, it creates a file `Vagrantfile` in the current directory. This file has quite a few options to configure. One option I always like to configure is the `synced_folder` option.
I will uncomment and modify it to <br> `config.vm.synced_folder "./ansible", "/home/vagrant/ansible"`. This expects an `ansible` folder in the current directory.
* `$ vagrant up` -- will bring the virtual machine up and setup the folder `ansible` in the root path
* `$ vagrant ssh` -- to ssh into the virtual machine

### Install Ansible on CentOS 6.5

Configure EPEL repository. EPEL (Extra Packages for Enterprise Linux) is a Fedora Special Interest Group that provides additional packages not included in the Red Hat product line.

`$ sudo yum -y update`  -- update the latest packages
<br />`$ sudo yum -y install wget`
<br />`$ wget https://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`
<br />`$ sudo rpm -Uvh epel-release-6-8.noarch.rpm`
<br />`$ sudo yum groupinstall -y 'development tools'`
<br />`$ sudo yum -y install python-pip python-devel`
<br />`$ sudo pip install setuptools --upgrade`
<br />`$ sudo pip install --upgrade pip`
<br />`$ sudo pip install ansible`

After installation has completed successfully, run this command to show Ansible's version number:
<br />`$ ansible --version`
<br />`>> ansible 2.1.1.0`

To upgrade ansible to the latest version available in pip repository:
<br />`$ sudo pip install ansible --upgrade`
