# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
require 'yaml'
VAGRANTFILE_API_VERSION = "2"

config_file=File.expand_path(File.join(File.dirname(__FILE__), 'vagrant_variables.yml'))
settings=YAML.load_file(config_file)

MEMORY     = settings['memory']
BOX        = settings['vagrant_box']
VMS        = settings['vms']

ansible_provision = proc do |ansible|
  ansible.playbook = 'vagrant_installation.yml'
  ansible.groups = {
    'nagiosgroup' => (0..VMS-1).map { |i| "nagios#{i}" }
  }
  ansible.extra_vars = {
    vagrant_installation: true,
  }
  ansible.limit = 'all'
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "nagios"
  config.vm.box = BOX
  config.vm.synced_folder "scripts", "/vagrant_data"

  (0..VMS - 1).each do |z|
    config.vm.define "nagios#{z}" do |nag|
      nag.vm.hostname = "nagios#{z}"
      nag.vm.network "forwarded_port", guest: 80, host: "300#{z}"
      nag.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", "#{MEMORY}"]
      end
      nag.vm.provision 'ansible', &ansible_provision if config
    end
  end
end