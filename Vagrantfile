# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  NUM_MACHINES = 3

  VAGRANT_VM_PROVIDER = "virtualbox"
  ANSIBLE_RAW_SSH_ARGS = []

  (1..NUM_MACHINES-1).each do |machine_id|
    ANSIBLE_RAW_SSH_ARGS << "-o IdentityFile=.vagrant/machines/vk8s-#{machine_id}/#{VAGRANT_VM_PROVIDER}/private_key"
  end

  (1..NUM_MACHINES).each do |machine_id|
    machine_number = 10 + machine_id - 1
    ip = "10.0.5.#{machine_number}"

    config.vm.define "vk8s-#{machine_id}" do |machine|
      machine.vm.box      = "ubuntu/xenial64"
      machine.vm.hostname = "vk8s-#{machine_id}"

      # Network for communicating with guest from host
      machine.vm.network "private_network", ip: "192.168.5.#{machine_number}"

      # Network for inter-node communication in the cluster
      machine.vm.network "private_network", ip: ip, virtualbox__intnet: true

      machine.vm.provider "virtualbox" do |v|
        v.name   = "vk8s-#{machine_id}"
        v.memory = 4096
        v.cpus   = 2

        # Virtualbox requires a unique mac address be explicitly defined to
        # avoid ip collisions
        v.customize [
          'modifyvm',
          :id,
          '--macaddress1',
          "0800270000#{machine_number}"
        ]
      end

      machine.vm.provision :shell, :inline => "sudo apt-get update && sudo apt-get install -y python"

      if machine_id == 1
        machine.vm.network "forwarded_port", guest: 80,   host: 8080
        machine.vm.network "forwarded_port", guest: 443,  host: 4443
        machine.vm.network "forwarded_port", guest: 8001, host: 8001
        machine.vm.network "forwarded_port", guest: 8443, host: 8443
        machine.vm.network "forwarded_port", guest: 6443, host: 6443
      end

      # Only execute once the Ansible provisioner when all the machines are up
      # and ready
      if machine_id == NUM_MACHINES
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit          = "all"
          ansible.playbook       = "playbook.yml"
          ansible.inventory_path = "ansible_hosts"
          ansible.verbose        = "-v"
          ansible.raw_ssh_args   = ANSIBLE_RAW_SSH_ARGS
        end
      end
    end
  end
end
