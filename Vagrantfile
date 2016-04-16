# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "project.dev"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 3000, host: 3100

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    echo "--------------------------------"
    echo ">>>   Installing essentials  <<<"
    echo "--------------------------------"
    apt-get -qy update
    apt-get -qy install build-essential git-core unzip nodejs libsqlite3-dev libpq-dev python-software-properties
    echo "--------------------------------"
    echo ">>>  Installing pg client    <<<"
    echo "--------------------------------"
    if which psql > /dev/null; then
      echo "Nothing to do"
    else
      apt-get -qy install postgresql-client-common postgresql-client
    fi
    echo "--------------------------------"
    echo ">>>     Installing vim       <<<"
    echo "--------------------------------"
    if which vim > /dev/null; then
      echo "Nothing to do"
    else
      apt-get -qy update
      apt-get -qy install vim
    fi
    echo "--------------------------------"
    echo ">>> Installing docker engine <<<"
    echo "--------------------------------"
    if which docker > /dev/null; then
      echo "Nothing to do"
    else
      apt-get -qy update
      apt-get -qy install curl
      wget -qO- https://get.docker.com/ | sed 's/lxc-docker/lxc-docker-1.11.0/' | sh
      usermod -aG docker vagrant
    fi
    echo "---------------------------------"
    echo ">>> Installing docker-compose <<<"
    echo "---------------------------------"
    if which docker-compose > /dev/null; then
      echo "Nothing to do"
    else
      curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
      chmod +x /usr/local/bin/docker-compose
    fi
    echo "---------------------------------"
    echo ">>>  Installing Ruby/Bundler  <<<"
    echo "---------------------------------"
    if which bundle > /dev/null; then
      echo "Nothing to do"
    else
      apt-add-repository ppa:brightbox/ruby-ng
      apt-get -qy update
      apt-get install -qy ruby2.3 ruby2.3-dev
      gem install rake bundler --no-rdoc --no-ri
    fi
    echo "---------------------------------"
    echo ">>>    Setting shell color    <<<"
    echo "---------------------------------"
    echo 'PS1="\e[1;31m$(whoami):\e[1;33m\\w\e[00m $ "' >> /home/vagrant/.bashrc
  SHELL
end

Vagrant::Config.run do |config|
  config.vm.network :hostonly, "192.168.99.11"
end
