# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "jtarpley/ubuntu2404_base"

N = 3
(1..N).each do |machine_id|
  config.vm.define "machine#{machine_id}" do |machine|
    machine.vm.hostname = "machine#{machine_id}"
    machine.vm.box_check_update = false
    machine.vm.network "private_network", ip: "192.168.77.#{10+machine_id}"

      machine.vm.provider "libvirt" do |libvirt|
        libvirt.memory = 2144
        libvirt.cpus = 2
        libvirt.nic_model_type = "virtio"
        libvirt.random_hostname = false

        libvirt.storage_pool_name = "default"

        libvirt.storage :file,
          size: "10G",
          type: "qcow2",
          bus: "virtio",
          name: "machine#{machine_id}.qcow2"
      end

      if machine_id == 1
         machine.vm.provision :ansible do |ansible|
           ansible.playbook = "playbook_machine1.yml"
           ansible.limit = "machine1" 
         end
      else
         machine.vm.provision :ansible do |ansible|
           ansible.playbook = "playbook.yml"
           ansible.limit = "machine#{machine_id}" 
         end
      end
    end
  end
end
