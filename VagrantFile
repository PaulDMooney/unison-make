# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.ssh.insert_key = false

  # Ansible Control Machine definition.
  # This example Control Machine uses a base box which comes pre-installed with Ansible, and uses ansible_local provisioning.
  # Vagrant 1.8.4 or later required.
  config.vm.define "control" do |control|
    control.vm.box = "comiq/ansiblebox"

    control.vm.network "private_network", ip: "192.168.56.210"

    control.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "control.yml"
        ansible.limit = "all"
        ansible.inventory_path = "hosts/local"
    end

    control.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
  end

  config.vm.define "unison" do |us|
    us.vm.box = "ubuntu/trusty64"

    us.vm.network "private_network", ip: "192.168.56.211"

    us.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
  end

end
