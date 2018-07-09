# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "ci_server" do |ci_server|
    ci_server.vm.hostname = "ci-server"
    ci_server.vm.network :private_network, ip: "192.168.33.55"
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

  config.vm.define "workstation" do |workstation|
    workstation.vm.synced_folder "./catkin_ws", "/catkin_ws"
    workstation.vm.hostname = "workstation"
    workstation.vm.network :private_network, ip: "192.168.33.58"
    workstation.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      vb.gui = true
      # Customize the amount of memory on the VM:
      vb.memory = "4096"
      vb.cpus = 6
    end
    # Ubuntu 18.04
    workstation.vm.box = "ubuntu/bionic64"
    # Install lubuntu desktop
    workstation.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y lubuntu-desktop
      SHELL
  end

  # Run Ansible from the Vagrant Host
  ENV['ANSIBLE_ROLES_PATH'] = ".vagrant/roles:./ansible/roles"
  ENV['ANISBLE_STDOUT_CALLBACK'] = "debug"
  config.vm.provision "ansible" do |ansible|
    ansible.galaxy_roles_path = '.vagrant/roles'
    ansible.galaxy_command = 'ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}'
    ansible.galaxy_role_file = 'ansible/requirements.yml'
    ansible.playbook = "ansible/playbook.yml"
    ansible.inventory_path = "ansible/production"
  end
end
