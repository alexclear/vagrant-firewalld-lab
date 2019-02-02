# -*- mode: ruby -*-
# vi: set ft=ruby :

$FW1_IP = "172.16.137.182"
$FW2_IP = "172.16.137.183"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/fw1/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/fw2/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "fw1" do |fw1|
    fw1.vm.box = "ubuntu/bionic64"
    fw1.vm.hostname = "fw1"
    fw1.vm.network "private_network", ip: $FW1_IP

    fw1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 1]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    fw1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "fw2" do |fw2|
    fw2.vm.box = "ubuntu/bionic64"
    fw2.vm.hostname = "fw2"
    fw2.vm.network "private_network", ip: $FW2_IP

    fw2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 1]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    fw2.vm.provision "shell", inline: "apt-get install -y python"
    fw2.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
