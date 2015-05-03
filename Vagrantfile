# -*- mode: ruby -*-
# vi: set ft=ruby :

# Create nodes
nodes = {
  'percona' => [2, 101],
}

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.auto_detect = true
    config.cache.scope = :box
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 1536
    v.cpus = 2
  end

  nodes.each do |prefix, (count, ip_start)|
    count.times do |i|
      
      hostname = "%s-%02d" % [prefix, (i+1)]
      ipaddress = "172.16.0.#{ip_start+i}"
      config.vm.define hostname do |box|
        box.vm.hostname = "#{hostname}"
        box.vm.network :private_network, ip: ipaddress, :netmask => "255.255.255.0"
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    #ansible.inventory_path = "inventory"
    ansible.groups = {
    		"galera_cluster" => ["percona-01","percona-02","percona-03"]
    	}
  end
end
