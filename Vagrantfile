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

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  #config.vm.network "forwarded_port", guest: 5232, host: 5232 # caldav
  config.vm.network "forwarded_port", guest: 80, host: 8008 # caldav

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  #config.vm.synced_folder "data", "/home/darwin"

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  if Vagrant.has_plugin?("vagrant-omnibus")
    config.omnibus.chef_version = "11.16.4"
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "darwin-calendar-server::setup"
    #chef.add_recipe "darwin-calendar-server::patch"
    chef.add_recipe "darwin-calendar-server::configure"
    chef.add_recipe "darwin-calendar-server::install"
    chef.json = {
      'darwin' => {
        'hostname' => "flock-test-#{Etc.getlogin}.ngrok.com",
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

  #config.vm.provision "shell",
  #  path: "provision-radicale.sh"

end
