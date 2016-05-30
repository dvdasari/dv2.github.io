---
layout: post
title: "Deploy Ruby On Rails 4 Application on Ubuntu 12.04 with Nginx and Passenger"
date: 2014-01-26 21:13:01 -0600
comments: true
keywords: ruby on rails 4, nginx, passenger
description: How to deploy Ruby On Rails 4 Application on Ubuntu 12.04 with Nginx and Passenger
---

This is a step by step guide for deploying a Ruby on Rails 4 application to Ubuntu 12.04 using Nginx & Passenger.

Here we will deploy to Linux Virtual Servers in the Linode Cloud. The same steps can be followed to deploy to other VPS servers, like [DigitalOcean](https://www.digitalocean.com), [Dreamhost](http://www.dreamhost.com) etc.

We will be using
### Ubuntu 12.04
### [Nginx](http://nginx.com)
### [Passenger](https://www.phusionpassenger.com)
### Ruby 2.1.0
### Rails 4.0.2
### MySql

<!-- more -->

## Signup to Linode
First [sign up](https://manager.linode.com/signup) for a Linode account if you do not have an account already.
Then **Add a Linode** and **Select your plan** from one of the plans available

![Select your plan]({{ site.url }}/assets/linode_select_your_plan.png)

you will have to select **Payment Term** and **Location** and **Add this Linode!** and **Complete Order**

### Select a Linux Distribution
There are a lot of operating systems to choose from, but we will go with Ubuntu 12.04 LTS
![Rebuild distribution]({{ site.url }}/assets/linode_rebuild_distribution.png)
Click **Rebuild**

Now you have your Linode, but it is initially in a **Powered Off** state. So Click on **Boot** to boot your linode.
![Boot]({{ site.url }}/assets/linode_boot.png)
Give it a few seconds to boot.

### Connecting to your linode
Click on the **Remote Access** tab in your account and copy the IP address from the SSH Access field. This is the IP address of your linode.
![Remote access]({{ site.url }}/assets/linode_remote_access.png)

We will connect to our linode via SSH.   
Mac users can use Terminal or iTerm2

```
ssh root@IPADDRESS
```

If you get the below warning, type *yes* and press Enter
```
[~]$ ssh root@IPADDRESS
The authenticity of host 'IPADDRESS (IPADDRESS)' can't be established.
RSA key fingerprint is 1b:16:36:09:4b:95:5e:35:53:d2:c9:b1:65:cd:cc:51.
Are you sure you want to continue connecting (yes/no)?
```

It will prompt to enter password, Enter the password you created for the *root* user (while setting up the Linux distribution) and press Enter
```
[~]$ ssh root@IPADDRESS
root@IPADDRESS's password:
```

You are now logged into your linode via SSH.

The first thing we will do on our new server is create a user account to run our applications
```
sudo adduser user_name
sudo adduser user_name sudo
su user_name
```

### Setting up SSH keys to login without having to enter password everytime

If you never setup ssh keys before, google to find out how to setup ssh keys  
Once you have the keys set up, we will copy the public key from your computer to server  

The public key on your computer is located in file *~/.ssh/id_rsa.pub*  
We will copy this on to server file *~/.ssh/authorized_keys*

Most likely you will not have the .ssh folder on server, so lets create that first and then we will create file *authorized_keys* and copy the public key there.

```
cd ~
mkdir .ssh
cd .ssh
touch authorized_keys
vi authorized_keys
```
copy the contents of file ~/.ssh/id_rsa.pub to authorized_keys

Now logout of the server by typing
```
exit
```
and log back in by typing
```
ssh user_name@IPADDRESS
```
it should log you into the server without asking for the password.

**We have successfully** <br>
1. Created a Linode account<br>
2. Installed a Linux distribution<br>
3. Ssh'ed into the server as root user<br>
4. Created a user account<br>
5. Set up authentication via keys<br>

Now we are all set to install the software required to deploy our app.

## Installing Ruby on Server

We will login as a regular user 'user_name' and not as the root user from here on.

```
### installing dependenies for ruby
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev libcurl4-openssl-dev python-software-properties

### installing rvm
sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
curl -L https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm
rvm install 2.1.0
rvm use 2.1.0 --default
ruby -v

### tell Rubygems to not install documentation
echo "gem: --no-ri --no-rdoc" > ~/.gemrc
gem install bundler

### install javascript runtime
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

## Installing Nginx and Passenger on Server
```
# Install Phusion's PGP key to verify packages
gpg --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7
gpg --armor --export 561F9B9CAC40B2F7 | sudo apt-key add -

# Add HTTPS support to APT
sudo apt-get install apt-transport-https

# Add the passenger repository
sudo sh -c "echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger precise main' >> /etc/apt/sources.list.d/passenger.list"
sudo chown root: /etc/apt/sources.list.d/passenger.list
sudo chmod 600 /etc/apt/sources.list.d/passenger.list
sudo apt-get update

# Install nginx and passenger
sudo apt-get install nginx-full passenger
```

Now that Nginx and Passenger are installed, we can manage the Nginx webserver by using the service commands
```
sudo service nginx start
sudo service nginx stop
sudo service nginx restart
```

Next, we need to update the Nginx configuration to update passenger_root, passenger_ruby

To find passenger_root
```
user_name@linode:~$ passenger-config --root
/usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
```

To find passenger_ruby
```
user_name@linode:~$ passenger-config --ruby-command
passenger-config was invoked through the following Ruby interpreter:
  Command: /home/user_name/.rvm/gems/ruby-2.1.0/wrappers/ruby
  Version: ruby 2.1.0p0 (2013-12-25 revision 44422) [x86_64-linux]
  To use in Apache: PassengerRuby /home/user_name/.rvm/gems/ruby-2.1.0/wrappers/ruby
  To use in Nginx : passenger_ruby /home/user_name/.rvm/gems/ruby-2.1.0/wrappers/ruby
  To use with Standalone: /home/user_name/.rvm/gems/ruby-2.1.0/wrappers/ruby /usr/bin/passenger start

```

Open file /etc/nginx/nginx.conf and uncomment and set passenger_root, passenger_ruby
``` ruby /etc/nginx/nginx.conf
##
# Phusion Passenger
##
# Uncomment it if you installed ruby-passenger or ruby-passenger-enterprise
##

passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
passenger_ruby /home/user_name/.rvm/gems/ruby-2.1.0/wrappers/ruby
```

Update the file /etc/nginx/sites-enabled/default to have the below content
``` ruby /etc/nginx/sites-enabled/default
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        server_name your_domain_name.com;
        passenger_enabled on;
        rails_env production;
        root /home/user_name/myapp/current/public;

        error_page 500 502 503 504  /50x.html;
        location = /50.x.html {
            root html;
        }
}
```

## Installing MySql on Server
``` ruby
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

## Create a new Ruby on Rails application on your local box/computer

Install the latest ruby version 2.1.0 via rvm  

``` ruby
rvm install 2.1.0
```

Install the latest rails version 4.0.2
``` ruby
gem install rails
```

Create a new rails app with mysql database
``` ruby
rails new myapp -d=mysql
```

Lets create a controller and setup routes for root path
``` ruby
rails g controller Pages index
```

Update config/routes.rb
``` ruby config/routes.rb
Myapp::Application.routes.draw do
  root to: 'pages#index'
end
```

Update Gemfile to add the gems needed for Capistrano
``` ruby Gemfile
gem 'capistrano'
gem 'capistrano-bundler'
gem 'capistrano-rails'
gem 'capistrano-rvm'
```

Modify config/database.yml to look like below
``` ruby config/database.yml
development:
  adapter: mysql2
  encoding: utf8
  database: myapp_development
  pool: 5
  username: root
  password:
  socket: /tmp/mysql.sock

test:
  adapter: mysql2
  encoding: utf8
  database: myapp_test
  pool: 5
  username: root
  password:
  socket: /tmp/mysql.sock

production:
  adapter: mysql2
  encoding: utf8
  database: myapp_production
  pool: 5
  username: root
  password:
```

Bundle and create files needed for Capistrano
``` ruby On command line
bundle --binstubs
cap install STAGES=production
```

Modify Capfile to look as below
``` ruby Capfile
require 'capistrano/setup'
require 'capistrano/deploy'
require 'capistrano/rvm'
require 'capistrano/bundler'
require 'capistrano/rails'
set :rvm_type, :user
set :rvm_ruby_version, '2.1.0'
Dir.glob('lib/capistrano/tasks/*.cap').each { |r| import r }
```

Modify config/deploy.rb to look as below  
*update repo_url below with the correct github repo url*
``` ruby config/deploy.rb
lock '3.1.0'

set :application, 'myapp'
set :repo_url, 'git@github.com:github_user_name/myapp.git'
set :deploy_to, '/home/user_name/myapp'
# set :linked_files, %w{config/database.yml}
set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}
namespace :deploy do

  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end

  after :finishing, 'deploy:cleanup'
end
```

Modify config/deploy/production.rb to look as below
``` ruby config/deploy/production.rb
role :app, %w{user_name@IPADDRESS}
role :web, %w{user_name@IPADDRESS}
role :db,  %w{user_name@IPADDRESS}

set :stage, :production

server 'IPADDRESS', user: 'user_name', roles: %w{web app}, my_property: :my_value
```

Create database on server
``` ruby
mysql -uroot -p
Enter password: (enter password you entered when you installed mysql)
mysql> CREATE DATABASE myapp_production;
```

Deploying app on server
``` ruby
cap production deploy
```

Now we have completed everything to deploy our Rails app on a Linux Virtual Server.

Open browser and type your domain name or IPADDRESS. You should see the below page

![Deploy home page]({{ site.url }}/assets/linode_deploy_home_page.png)

That is all we have to do to deploy a Ruby on Rails application to run on Ubuntu 12.04.
