---
layout:      post
title:       Use Ansible for AWS Provisioning and Configuration Management
---

Ansible is a great tool for provisioning AWS environment. You can run Ansible commands from your mac, but I prefer setting up a VM on mac and running Ansible playbooks from there.

We will use CentOS 7 distribution of Linux for the VM. Vagrant should already be installed on the mac. Otherwise, here is the link for [installing vagrant](https://www.vagrantup.com/downloads.html).

### Set Up VM, Install Ansible and boto
cd into your working directory and create the below files. `Vagrantfile` for vm configuration and `ansible` folder for ansible playbooks.

```
$ touch Vagrantfile
$ mkdir ansible
```

Copy the below lines into `Vagrantfile`. This configuration is to

* set up a virtual environment that runs CentOS 7
* create a sync folder between the host machine and the virtual machine
* update packages
* install ansible
* install Python boto - Boto is a Python package that provides interfaces to Amazon Web Services. Read more about Boto [here](https://github.com/boto/boto). It lists all the AWS Services that boto supports.

```
Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.synced_folder "./ansible", "/home/vagrant/ansible"
  config.vm.provision "shell", inline: <<-SHELL
    sudo yum -y update
    sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm
    sudo yum -y install ansible
    sudo yum -y install python-pip
    sudo pip install boto
  SHELL
end
```

Bring the vm box up:

```
$ vagrant up
```

Now we have a virtual machine that is fully configured to create and run Ansible Playbooks for configuring AWS.

Ssh into vm box:

```
$ vagrant ssh
```

Configure `boto` to use AWS access keys. Create `~/.boto` file and add the below

```
[Credentials]
aws_access_key_id = <your_access_key_here>
aws_secret_access_key = <your_secret_key_here>
```

Lets create a playbook to create EC2 key pair

`$ cd ~/ansible`

Create `hosts` file in current directory and update it:

```
[local]
localhost
```

Create playbook file `keypair.yml` in current directory and update with below lines

```
---
- hosts: localhost
  connection: local
  gather_facts: no
  vars:
    region: us-west-2
    keyname: aws-key-pair-usoregon
  tasks:
    - name: create key pair
      ec2_key:
        region: "{{ region }}"
        name: "{{ keyname }}"
      register: mykey

    - debug: var=mykey

    - name: write to file
      copy: content="{{ mykey.key.private_key }}" dest="~/.ssh/{{ keyname }}.pem" mode=0600
      when: mykey.changed
```

Execute the playbook: `$ ansible-playbook -i hosts keypair.yml`

New key pair is created and the private key is saved to `~/.ssh/aws-key-pair-usoregon.pem`
