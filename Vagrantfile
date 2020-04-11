# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.cpus = 1
    v.memory = "2048"
  end

  config.vm.define "v-master" do |v|
    v.vm.hostname = "master"
    v.vm.network "private_network", ip: "192.168.50.10"
  end

  config.vm.define "v-node-1" do |v|
    v.vm.hostname = "node-1"
    v.vm.network "private_network", ip: "192.168.50.20"
  end

  config.vm.define "v-node-2" do |v|
    v.vm.hostname = "node-2"
    v.vm.network "private_network", ip: "192.168.50.21"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/site.yml"
    ansible.compatibility_mode = "2.0"
    ansible.groups = {
      "master" => ["v-master"],
      "node" => ["v-node-1", "v-node-2"],
      "k3s_cluster:children" => ["master", "node"],
      "k3s_cluster:vars" => {
        "k3s_version" => "v1.17.4+k3s1",
        "ansible_python_interpreter" => "auto_silent",
        "systemd_dir" => "/etc/systemd/system",
        "master_ip" => "192.168.50.10",
      }
    }
    ansible.host_vars = {
      "v-master" => {
        "extra_args" => "--no-deploy traefik --no-deploy servicelb --flannel-iface eth1",
        "helm_version" => "v3.1.2"
      },
      "v-node-1" => {
        "node_ip" => "192.168.50.20",
        "extra_args" => "--flannel-iface eth1"
      },
      "v-node-2" => {
        "node_ip" => "192.168.50.21",
        "extra_args" => "--flannel-iface eth1"
      }
    }
  end
end
