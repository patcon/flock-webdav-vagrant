# Flock WebDAV Testing Environment

This project aims to create a webdav environment that can be used for
testing Flock on a self-hosted server. In the future, it may aspire to
help users easily spin up a self-hosted server for their own personal
usage.

## Requirements

- [Install](https://downloads.chef.io/chef-dk/) ChefDK
- [Install](https://docs.vagrantup.com/v2/installation/) Vagrant
- `vagrant plugin install vagrant-omnibus`
- `vagrant plugin install vagrant-cachier`

## Usage

```
berks vendor cookbooks
vagrant up
```

You now have a local WebDAV VM running and exposed on port `8008`. To expose this VM on the internet for
testing, I recommend using [ngrok](https://ngrok.com/download):

    ngrok -subdomain=flock-test-<SYSTEM_USERNAME> 8008

The system username is taken from the login name of the session you're
using. This will hopefully be provisioned as part of the VM in the
future, so that ngrok exposure will happen automatically.

## Configuration

Here are locations where configuration should be noted and changed
accordingly:

- https://github.com/patcon/flock-webdav-vagrant/blob/master/Vagrantfile#L26
- https://github.com/patcon/flock-webdav-vagrant/blob/master/Vagrantfile#L79
- https://github.com/patcon/flock-webdav-vagrant/blob/master/Vagrantfile#L84-L85
