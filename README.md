geoserver-vagrant
=================

A vagrant setup for running GeoServer using DigitalOcean as a provider

Getting Started
===============

* Run `vagrant plugin install vagrant-digitalocean` (see details at https://github.com/smdahlen/vagrant-digitalocean)
* Generate SSH key `ssh-keygen -t rsa` and put public key with "Vagrant" name at https://cloud.digitalocean.com/settings/security 
* Specify in `provider.token` your Digital Ocean token from https://cloud.digitalocean.com/settings/api/tokens
* Change other Digital Ocean properties in `config.vm.provider` if needed
* `vagrant up`
* http://digitaloceanhost:8080/geoserver

