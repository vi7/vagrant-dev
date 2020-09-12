# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
require 'logger'

log = Logger.new(STDOUT)

# Local vars
repo_provision_dir = 'provision'
provisioning_path = '/home/vagrant/provision'

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
  config.vm.hostname = "vagrant-#{`hostname`[0..-2]}"

  config.vbguest.auto_update = true

  if dev_config["port_mapping"]
    dev_config["port_mapping"].each do |host, guest|
      config.vm.network "forwarded_port", host: host, guest: guest
    end
  end

  config.vm.network "private_network", ip: "172.16.16.16"

  # Disabling the default /vagrant share
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.synced_folder repo_provision_dir, provisioning_path, create: true
  config.vm.synced_folder dev_config["dev_dir"], "/home/vagrant/devel", create: true
  # create and share dirs between the host and the VM
  dev_config["synced_dir_mapping"].each do |item|
    Dir.mkdir(item['host_path']) if ! Dir.exist?(item['host_path'])
    config.vm.synced_folder item['host_path'], item['guest_path'], create: true
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "6144"
    vb.cpus = 2
  end

  # Some preparations for ansible-galaxy to run successfully
  config.vm.provision "shell" do |s|
    s.inline = <<-SCRIPT
yum -y install https://centos7.iuscommunity.org/ius-release.rpm
yum -y install git2u
SCRIPT
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.install_mode = "pip"
    ansible.version = dev_config["ansible_version"]
    ansible.provisioning_path = provisioning_path
    ansible.galaxy_role_file = "requirements.yml"
    ansible.playbook = "site.yml"
    ansible.verbose = "-vv"
  end

  # Run custom user's provision script if exists
  config.vm.provision "shell", inline: "sudo bash #{provisioning_path}/custom.sh" if File.exist?("#{repo_provision_dir}/custom.sh")

end
