---
layout:      post
title:       Use Ansible for AWS Provisioning and Configuration Management
---

We will use CentOS 7 distribution of Linux to run Ansible. To setup VM on mac to run CentOS 7, [read this article](http://dasari.me/2016/08/25/run-virtual-machine-centos-7-on-mac-install-ansible.html)

### Install Python boto
Boto is a Python package that provides interfaces to Amazon Web Services. Read more about Boto [here](https://github.com/boto/boto). It lists all the AWS Services that boto supports.

On CentOS 7

```
$ sudo yum -y update
$ sudo yum -y install python-pip
$ sudo pip install boto
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

Create playbook file `keypair.yml` in current directory

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
