# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# Read YAML file 
servers = YAML.load_file('servers.yml')

# Define groups
groups = Hash.new

Vagrant.configure(2) do |config|

  # Load Cachier plugin if available
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.auto_detect = true
    config.cache.scope = :box
  end

  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"] do |srv|
      srv.vm.box = servers["box"]
      srv.vm.network "private_network", ip: servers["ip"]
      srv.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.cpus = servers["cpus"]
        vb.memory = servers["ram"]
      end
    end

    # Create groups hash object for Ansible
    if groups.has_key?(servers["group"])
        groups[servers["group"]].push(servers["name"])
    else
        groups[servers["group"]] = [servers["name"]]
    end

  end

  # Setup the Ansible provisioner
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.groups = groups    	
  end
end
