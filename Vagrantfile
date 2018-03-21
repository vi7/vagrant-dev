# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"

  config.vbguest.auto_update = true

  # Netatalk AFP port
  config.vm.network "forwarded_port", guest: 548, host: 9548

  config.vm.network "private_network", ip: "172.16.16.16"

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "/Users/vi7al/Devel", "/home/vagrant/devel", create: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

end
