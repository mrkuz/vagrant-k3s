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
    v.vm.network "private_network", ip: "192.168.50.11"
  end

  config.vm.define "v-node-1" do |v|
    v.vm.network "private_network", ip: "192.168.50.21"
  end

  config.vm.define "v-node-2" do |v|
    v.vm.network "private_network", ip: "192.168.50.22"
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
        "master_ip" => "192.168.50.11",
        "extra_server_args" => ""
      }
    }
  end
end
