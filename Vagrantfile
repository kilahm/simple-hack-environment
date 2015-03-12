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

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.100.100"

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
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
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
    sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449
    sudo add-apt-repository 'deb http://dl.hhvm.com/ubuntu trusty main'
    sudo apt-get update
    sudo apt-get install -y apache2 git hhvm
    echo "*****"
    echo "Configuring apache to use hhvm to process .php and .hh files."
    echo "*****"
    sudo /usr/share/hhvm/install_fastcgi.sh
    sudo update-rc.d hhvm defaults
    sudo /usr/bin/update-alternatives --install /usr/bin/php php /usr/bin/hhvm 60
    echo "*****"
    echo "Creating sample index.php in /vagrant/hhvm"
    echo "*****"
    mkdir /vagrant/hhvm
    touch /vagrant/hhvm/.hhconfig
    echo "<?hh echo 'Hello SeaPHP!';" > /vagrant/hhvm/index.php
    echo "*****"
    echo "Configuring hhvm to look for files in /vagrant/hhvm"
    echo "*****"
    sudo ln -s /vagrant/hhvm /var/www/hhvm
    sudo echo "ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/hhvm/$1" > /etc/apache2/mods-available/hhvm_proxy_fcgi.conf
    sudo chown -R vagrant:vagrant /vagrant
    echo "*****"
    echo "Increasing the http slow query threshold for command line scripts"
    echo "*****"
    sudo echo "hhvm.http.slow_query_threshold = 60000" >> /etc/hhvm/php.ini
    sudo echo "hhvm.http.default_timeout = 120" >> /etc/hhvm/php.ini
    echo "*****"
    echo "Restarting apache and hhvm services"
    echo "*****"
    sudo service apache2 restart
    sudo service hhvm restart
    echo "*****"
    echo "Installing composer"
    echo "*****"
    curl -sS https://getcomposer.org/installer | sudo php -- --filename=composer --install-dir=/usr/local/bin
  SHELL
end
