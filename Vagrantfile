# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'
Vagrant.configure("2") do |config|

  inventory = YAML.load_file(File.join(__dir__, 'inventory.yml'))

  inventory['all']['children'].each do |group, properties|
    properties['hosts'].each do |host, host_vars|
      config.vm.define host do |node|
        node.vm.box = "koalephant/debian12"
        #node.vm.network "public_network", ip: host_vars['ansible_host']
        node.vm.network "private_network", ip: host_vars['ansible_host']
        node.vm.hostname = host
        node.vm.provider "virtualbox" do |v|
          v.memory = host_vars['memory']
          v.cpus = host_vars['cpu']
          v.name = host
        end
      end
    end
  end
  
  # Generate Ansible inventory file
  ansible_inventory = { 'all' => { 'children' => inventory['all']['children'] } }
  temp_inventory_path = '/tmp/vagrant_ansible_inventory'
  File.open(temp_inventory_path, 'w') do |file|
    file.write(ansible_inventory.to_yaml)
  end

  if Vagrant.has_plugin?("vagrant-timezone")
    config.timezone.value = :host
  end

  # other provisioning settings...

  config.vm.provision :ansible do |ansible|
    ansible.verbose       = "vv"
    ansible.playbook      = "playbook.yml"
    ansible.compatibility_mode = "2.0"
    ansible.become = "true"
    ansible.inventory_path        = temp_inventory_path 
  end
end
