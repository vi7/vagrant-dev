Vagrant-powered local dev environment
=====================================

Here you will find [Vagrantfile](./Vagrantfile), related configs and scripts for local development VM.

Provision is done by Ansible. Roles used for the provision and their location is defined here in the [provision/requirements.yml](./provision/requirements.yml)

**Why?**

- *Clean and transparent:* all the tools are in one place, thus easy to resolve all "magical" issues about dependencies, system configs, pathes etc., you host OS is clean and not lagging after the tons of various installed apps and tools
- *Flexible and maintainable:* Easy to re-create and re-provision the machine from scratch at any place and any time
- *Compatible:* Code tested in this VM supposed to be working on any machine with the Linux distro defined in the [Vagrantfile](./Vagrantfile) 

**You're welcome to contribute and add some useful stuff via pull requests!**

## You'll need

- [Vagrant](https://www.vagrantup.com/downloads.html) - `v2.2.19`
- [Vagrant-vbguest plugin](https://github.com/dotless-de/vagrant-vbguest#installation)
- [Virtual Box](https://www.virtualbox.org/wiki/Downloads) - `v6.1.30`
- [Some dev/ops skills](http://bfy.tw/Fy6H)

Versions above are tested and working, you can use any other for your own risk and responsibility

## Launch it

1. Copy the [dev_config.yml.template](./dev_config.yml.template) to the `dev_config.yml`. Define variables inside the created `dev_config.yml` as needed, see the inline comments for help. `dev_config.yml` will be ignored by git
1. Run `vagrant up`

## Use it

1. Run `vagrant ssh`
2. Your git repositories (provided in the `dev_config.yml` above) will be available under `/home/vagrant/gitrepo`
3. Any dirs defined in the `synced_dir_mapping` of the `dev_config.yml` will be mounted into the VM automatically. By default Vagrant will create a `share` dir here in the repo. It can be used for file exchange between the host and the VM and will be available under `/home/vagrant/share/` inside the VM. All the data inside the `share` dir is ignored by git

## Configure it

1. You can easily add your own steps to the machine provision by creating `custom.sh` script inside `provision` dir. This script will be ignored by git

## Hints

1. In addition to the ports forwarded explicitly via the Vagrantfile (if defined in the `dev_config.yml`) any network service launched inside the virtual machine will be available at the `172.16.16.16` from your host

## Kill it

1. `vagrant destroy`
