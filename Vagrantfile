# -*- mode: ruby -*-
# vi: set ft=ruby :

cidr_prefix = "192.168.33"
worker_node_cnt = 2

os_name = "almalinux"
os_version = "9"
os_version_date = "9.5.20241203"
os_name_version = "#{os_name}/#{os_version}"
os_name_version_date = "#{os_name}/#{os_version_date}"

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.box = "#{os_name}/#{os_version}"
  config.vm.box_version = "#{os_version_date}"
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # Master node
  config.vm.define "master" do |master|
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "#{cidr_prefix}.10"
    master.vm.synced_folder "ansible/", "/data"
    master.vm.provider "vmware_desktop" do |vm|
      vm.cpus = 2
      vm.memory = 1024
    end
    master.vm.provision "shell", inline: <<-SHELL
      dnf update -y && dnf install -y podman
    SHELL
  end

  # Worker nodes
  worker_node_cnt.times do |i|
    config.vm.define "worker#{i + 1}" do |worker|
      worker.vm.hostname = "worker#{i + 1}"
      worker.vm.network "private_network", ip: "#{cidr_prefix}.#{i + 11}"
      worker.vm.provider "vmware_desktop" do |vm|
        vm.cpus = 1
        vm.memory = 512
      end
    end
  end
end
