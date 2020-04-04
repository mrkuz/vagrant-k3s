# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.cpus = 1
    v.memory = "2048"
  end

  config.vm.define "master" do |v|
    v.vm.network "private_network", ip: "192.168.50.11"
  end

  config.vm.define "node-1" do |v|
    v.vm.network "private_network", ip: "192.168.50.21"
  end

  config.vm.define "node-2" do |v|
    v.vm.network "private_network", ip: "192.168.50.22"
  end
end
