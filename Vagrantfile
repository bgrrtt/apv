# -*- mode: ruby -*-
# vi: set ft=ruby :

DOMAIN_NAME = "project"
LOCAL_IP = "10.10.10.10"

Vagrant.configure("2") do |config|

  # Box Image
  config.vm.box = "bento/ubuntu-14.04"

  # Don't grab my ssh!
  config.ssh.insert_key = false

  # Virtualbox Configuartion
  config.vm.provider :virtualbox do |v|
    v.name = DOMAIN_NAME + ".vm"
    v.memory = 2048
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--nictype1", "virtio" ]
    v.customize ["modifyvm", :id, "--nictype2", "virtio" ]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
    v.customize ["storagectl", :id, "--name", "SATA Controller", "--hostiocache", "on"]
  end

  # Set hostname and IP
  config.vm.hostname = DOMAIN_NAME + ".dev"
  config.vm.network :private_network, ip: LOCAL_IP

  # Sync local folders for dev
  config.vm.synced_folder ".", "/nfs", type: "nfs", mount_options: ['rw', 'vers=3', 'tcp', 'fsc' ,'actimeo=2']
  config.bindfs.bind_folder "/nfs", "/site"

  # Set name of the VM
  config.vm.define DOMAIN_NAME do |yo_thing|
  end

  # Provison with Ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.provisioning_path = "/site/ops"
    ansible.inventory_path = "/site/ops/inventory/hosts"
    ansible.limit = "all"
    ansible.version = "latest"
    ansible.verbose = false
  end

end
