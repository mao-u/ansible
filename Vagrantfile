# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.define "vagrant-development" do |server|
    server.vm.box = "generic/ubuntu2204"
    # config.vm.network "forwarded_port", guest: 80, host: 8080
    server.vm.network "private_network", ip: "192.168.56.10"
    server.vm.network "public_network"
    server.vm.provider "virtualbox" do |vmbox|
      vmbox.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "2", "--ioapic", "on"]
    end

    config.vm.synced_folder ".", "/vagrant"
    config.vm.synced_folder "../data/app", "/home/vagrant/app"
    config.vm.provision :shell, :path => "provision.sh"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    # ansible.raw_arguments = ['--check']
    ansible.groups   = {
      "vagrant-development" => ["vagrant-development"]
    }
  end
end
