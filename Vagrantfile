# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.box_version = "201812.27.0"
  config.ssh.insert_key = false

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.2.2"

  parityDisk = './parityDisk.vdi'
  dataDisk1 = './dataDisk1.vdi'
  dataDisk2 = './dataDisk2.vdi'

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"

    # Building disk files if they don't exist
    if not File.exists?(parityDisk)
      vb.customize ['createhd', '--filename', parityDisk, '--size', 5 * 1024]
    end
    if not File.exists?(dataDisk1)
      vb.customize ['createhd', '--filename', dataDisk1, '--size', 5 * 1024]
    end
    if not File.exists?(dataDisk2)
      vb.customize ['createhd', '--filename', dataDisk2, '--size', 5 * 1024]

      # Adding a SATA controller that allows 4 hard drives
      #vb.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 4]
      # Attaching the disks using the SATA controller
      vb.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', parityDisk]
      vb.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', dataDisk1]
      vb.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 3, '--device', 0, '--type', 'hdd', '--medium', dataDisk2]
    end

      # Run Ansible provisioner once for all VMs at the end.
      # ansible.inventory_path = "vagrant/"
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "nas.yml"
        ansible.become = true
        ansible.limit = "all"

    end
  end
end
