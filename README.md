# Flock WebDAV Testing Environment

This project aims to create a webdav environment that can be used for
testing Flock on a self-hosted server. In the future, it may aspire to
help users easily spin up a self-hosted server for their own personal
usage.

## Requirements

- [Install](https://downloads.chef.io/chef-dk/) ChefDK
- [Install](https://docs.vagrantup.com/v2/installation/) Vagrant

## Usage

```
vagrant plugin install vagrant-omnibus
vagrant plugin install vagrant-cachier
berks vendor cookbooks
vagrant up
```

Your VM is now exposed on the internet via [ngrok](ihttps://ngrok.com/)
at:

    http://flock-test-<SYSTEM_USERNAME>.ngrok.com

It is also exposed locally on port `8008`.

You may inspect requests to the VM's WebDAV server via ngrok's web
interface:

    http://127.0.0.1:4040

The system username is taken from the login name of the session you're
using (ie. your home directory name on linux). This will hopefully be
provisioned as part of the VM in the future, so that ngrok exposure will
happen automatically.

## Configuration

Here are locations where configuration should be noted and changed
accordingly:

- https://github.com/patcon/flock-webdav-vagrant/blob/master/Vagrantfile#L89-L90
