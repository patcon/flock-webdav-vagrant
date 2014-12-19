# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'etc'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network "forwarded_port", guest: 80, host: 8008 # webdav
  config.vm.network "forwarded_port", guest: 4040, host: 4040 # ngrok admin

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  if Vagrant.has_plugin?("vagrant-omnibus")
    config.omnibus.chef_version = "11.16.4"
  end

  # Will be used to create a unique subdomain using ngrok:
  # Example: https://flock-test-myusername.ngrok.com
  subdomain = "flock-test-#{Etc.getlogin}"

  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "darwin-calendar-server::setup"
    #chef.add_recipe "darwin-calendar-server::patch"
    chef.add_recipe "darwin-calendar-server::configure"
    chef.add_recipe "darwin-calendar-server::install"
    chef.add_recipe "ngrok"
    chef.json = {
      'darwin' => {
        'hostname' => "#{subdomain}.ngrok.com",
        'max_requests' => '10',
        'directory_service' => 'twistedcaldav.directory.xmlfile.XMLDirectoryService',
        'webdav_users' => [
          {
            'username' => 'patcon@custom',
            'password' => 'p4ssw0rd',
          },
        ],
      },
      'memcached' => {
        'host' => '127.0.0.1',
        'port' => '11211',
      },
      'ngrok' => {
        'auth_token' => 'nKgwjggnGnuiIaWTprg9',
        # Make web interface available on host
        'inspect_addr' => '0.0.0.0:4040',
        'tunnels' => {
          "#{subdomain}" => {
            'proto' => {
              'http' => '80',
            },
          },
        },
      },
      'postgres-accounts' => {
        'host' => '',
        'database' => '',
        'darwin_user' => '',
        'darwin_password' => '',
      },
      'postgres-darwin' => {
        'host' => 'localhost',
        'database' => 'darwin',
        'user' => 'darwin',
        'password' => 'password123',
      },
      'postgresql' => {
        'password' => {
          'postgres' => 'passpass',
        }
      },
      's3fs' => {
        'bucket' => '',
      },
      'statsd' => {
        'host' => "",
        'access_key_id' => "",
        'secret_access_key' => "",
      },
    }
  end

end
