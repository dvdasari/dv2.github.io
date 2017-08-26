# running blog locally
1. ssh into vm: $ ssh vm
1. cd into app root
1. bundle install
1. bundle exec jekyll serve --host 0.0.0.0 --port 3000

NOTE: make sure other ports are allowed if using a firewall like ufw 
