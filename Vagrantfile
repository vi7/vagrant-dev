# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
require 'logger'

log = Logger.new(STDOUT)

# Load dev env configuration
if File.exist?('dev_config.yml')
  dev_config  = YAML.load_file('dev_config.yml')
else
  log.fatal("Configuration file \"dev_config.yml\" wasn't found! Please create it based on the template.")
  exit 1
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"

  config.vbguest.auto_update = true

  if dev_config["port_mapping"]
    dev_config["port_mapping"].each do |guest, host|
      config.vm.network "forwarded_port", guest: guest, host: host
    end
  end

  config.vm.network "private_network", ip: "172.16.16.16"

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "scripts", "/home/vagrant/provision_scripts", create: true
  config.vm.synced_folder dev_config["dev_dir"], "/home/vagrant/devel", create: true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.config_file = "ansible/ansible.cfg"
    ansible.playbook = "ansible/dev_machine.yml"
  end

  # Run custom user's provision script if exists
  config.vm.provision "shell", inline: 'sudo bash /home/vagrant/provision_scripts/custom.sh' if File.exist?('scripts/custom.sh')

end
