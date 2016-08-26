---
layout:      post
title:       Use Ansible for AWS Provisioning and Configuration Management
---

Ansible is a great tool for provisioning AWS environment. You can run Ansible commands from your mac, but I prefer setting up a VM on mac and running Ansible playbooks from there.

We will use CentOS 7 distribution of Linux for the VM. Vagrant should be installed on the mac. For [installing vagrant click here](https://www.vagrantup.com/downloads.html).

### Set Up VM, Install Ansible and boto
cd into your working directory and create the below files.

* `Vagrantfile` for vm configuration
* `playbook.yml` playbook for provisioning virtual machine
* `ansible` folder for ansible playbooks for provisioning AWS
* `group_vars` folder for variables
* `templates` folder for virtual machine config files

```
$ mkdir ansible group_vars templates
$ touch Vagrantfile playbook.yml templates/boto.j2 group_vars/all
```

Copy the below lines into `Vagrantfile`. This configuration is to

* set up a virtual environment that runs CentOS 7
* create a sync folder between the host machine and the virtual machine
* update packages
* install ansible
* install Python boto - Boto is a Python package that provides interfaces to Amazon Web Services. Read more about Boto [here](https://github.com/boto/boto). It lists all the AWS Services that boto supports. x

{% highlight ruby %}
Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.synced_folder "./ansible", "/home/vagrant/ansible"
  config.vm.provision "ansible" do |ansible|
    # ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
  end
end
{% endhighlight %}

Copy the below into `playbook.yml`

{% highlight ruby %}
---
- hosts: all
  become: true
  tasks:
    - name: Upgrade all packages
      yum: name=* state=latest

    - name: Install EPEL repository
      yum: name=https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm state=present

    - name: Install Ansible
      yum: name=ansible

    - name: Install python-pip
      yum: name=python-pip

    - name: pip install boto
      pip: name=boto

    - name: Setup boto config file
      template: src=boto.j2
                dest=/home/vagrant/.boto
                owner=vagrant
                group=vagrant
{% endhighlight %}


Copy the below into `templates/boto.j2`

{% raw %}
```
[Credentials]
aws_access_key_id = {{ AWS_ACCESS_KEY_ID }}
aws_secret_access_key = {{ AWS_SECRET_ACCESS_KEY }}
```
{% endraw %}

Copy the below into `group_vars/all` and update aws access key, secret access key

{% highlight ruby %}
---
AWS_ACCESS_KEY_ID:      update-aws-access-key-id
AWS_SECRET_ACCESS_KEY:  update-aws-secret-access-key
{% endhighlight %}

Bring the vm box up:

```
$ vagrant up
```

The above statement creates virtual machine and provisions. Later if only provisioning is needed, we can just run

```
$ vagrant provision
```

Now we have a virtual machine that is fully configured to create and run Ansible Playbooks for configuring AWS.

Ssh into vm box:

```
$ vagrant ssh
```

Lets create a playbook to create EC2 key pair

`$ cd ~/ansible`

Create `hosts` file in current directory and update it:

```
[local]
localhost
```

Create playbook file `keypair.yml` in current directory and update with below lines

{% raw %}
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
{% endraw %}

Execute the playbook: `$ ansible-playbook -i hosts keypair.yml`

New key pair is created and the private key is saved to `~/.ssh/aws-key-pair-usoregon.pem`
