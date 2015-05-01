# -*- mode: ruby -*-
# vi: set ft=ruby :

# Create nodes
nodes = {
  'percona' => [2, 101],
}

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

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
    ansible.groups = {
      "bootstrap" => ["percona-01"],
      "node" => ["percona-02"],
      "all_groups:children" => ["bootstrap", "node"]
     }
  end
end
