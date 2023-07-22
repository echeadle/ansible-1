# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Base VM OS configuration
  config.vm.box = "ubuntu/jammy64"
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # General VirtualBox VM configuration
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 1
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Server 1
  config.vm.define "srv1" do |srv1|
    srv1.vm.hostname = "srv1.test"
    srv1.vm.network :private_network, ip: "192.168.56.40"

#    srv1.vm.provision "shell", 
#      inline: "sudo apt-get update && sudo apt-get upgrade -y"

    srv1.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 1024]
    end

    # Run Ansible provisioner once for all VMs at the end.
    srv1.vm.provision "ansible" do |ansible|
      ansible.playbook = "configure.yml"
      ansible.inventory_path = "inventories/vagrant/inventory"
      ansible.limit = "all"
      ansible.extra_vars = {
      ansible_user: 'vagrant',
      ansible_ssh_private_key_file: \
      "~/.vagrant.d/insecure_private_key"
      }
    end
  end
end
