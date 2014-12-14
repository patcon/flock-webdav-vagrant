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
