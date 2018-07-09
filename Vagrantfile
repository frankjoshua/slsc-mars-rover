# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "ci_server" do |ci_server|
    #Forward Jenkins port
    ci_server.vm.network "forwarded_port", guest: 8080, host: 8080
    ci_server.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false
      # Customize the amount of memory on the VM:
      vb.memory = "512"
      vb.cpus = 1
    end
    #Ubuntu 18.04
    ci_server.vm.box = "ubuntu/bionic64"
  end

  config.vm.define "pi" do |pi|
    pi.vm.box = "bearonis/rpi-emulator"
    pi.vm.box_version = "0.0.1"
  end
  
  #
  # Run Ansible from the Vagrant Host
  #
  config.vm.provision "ansible" do |ansible|
    ansible.galaxy_role_file = 'ansible/requirements.yml'
    ansible.playbook = "ansible/playbook.yml"
  end
end
