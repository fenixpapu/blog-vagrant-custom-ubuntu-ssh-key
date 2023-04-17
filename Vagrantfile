# -*- mode: ruby -*-
# vi: set ft=ruby :


BOX_NAME =  ENV['BOX_NAME'] || "ubuntu/focal64"
BOX_MEM = ENV['BOX_MEM'] || "1024"
BOX_CPUS = ENV['BOX_CPUS'] || "2"
CLUSTER_HOSTS = ENV['CLUSTER_HOSTS'] || "vagrant_hosts"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL


  config.vm.define "node1", primary: true do |node1_config|
    node1_config.vm.box = "ubuntu/focal64"
    node1_config.vm.hostname = "node1"
    node1_config.ssh.shell = "/bin/sh"
    node1_config.ssh.forward_agent = true
    node1_config.vm.network :private_network, ip: "192.168.56.235"
    #node1_config.vm.network "forwarded_port", guest: 22, host: 2252
    node1_config.vm.boot_timeout = 360
    node1_config.vm.provider "virtualbox" do |vb|
      vb.name = "node1"
      vb.memory = BOX_MEM
      vb.cpus = BOX_CPUS
      vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL] # https://bugs.launchpad.net/cloud-images/+bug/1874453
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end
    node1_config.vm.provision :hosts do |provisioner|
      provisioner.sync_hosts = false
      provisioner.add_localhost_hostnames = false
      provisioner.add_host '192.168.56.235', ['node1']
      provisioner.add_host '192.168.56.236', ['node2']
      provisioner.add_host '192.168.56.237', ['node3']
      #provisioner.add_host '192.168.122.27', ['dc2p-mssql-master.citigo.io']
    end
    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
          echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
        SHELL
    end
  end
end
