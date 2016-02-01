geoserver-vagrant
=================

A vagrant setup for running the latest geoserver

Getting Started
===============

* `vagrant plugin install vagrant-digitalocean` (see details at https://github.com/smdahlen/vagrant-digitalocean)
* Generate SSH key `ssh-keygen -t rsa` and put public key with "Vagrant" name at https://cloud.digitalocean.com/settings/security 
* Specify your Digital Ocean token from https://cloud.digitalocean.com/settings/api/tokens
* `vagrant up`
* http://digitaloceanhost:8080/geoserver

