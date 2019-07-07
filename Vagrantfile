# -*- mode: ruby -*-
# vi: set ft=ruby :
#
#
Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.5"

  #config.vm.network "forwarded_port", guest: 443, host: 9443

  config.vm.network "forwarded_port", guest: 983, host: 8983
  config.vm.network "forwarded_port", guest: 984, host: 8984

  config.vm.synced_folder "./build", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1028"
    vb.cpus= "2"
    vb.customize ["modifyvm", :id, "--usb", "off", "--usbehci", "off"]
    # Necessary for guest to use host's VPN connection
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # common provisioning for all machine types
  config.vm.provision "shell", inline: <<-SHELL
    set -o errexit

    yum install -y net-tools
    yum install -y wget
    yum install -y systemd
    yum install -y openssl
    yum install -y freetype

    mkdir example/nodes 
    mkdir example/nodes/node1

    # Copy solr.xml into this solr.home
    cp solr/server/solr/solr.xml example/nodes/node1/.
    # Repeat the above steps for the second node
    mkdir example/nodes/node2
    cp solr/server/solr/solr.xml example/nodes/node2/.

    # Start first node on port 8983
    solr/bin/solr start -s example/nodes/node1 -p 8983

    # Start second node on port 8984
    solr/bin/solr start -s example/nodes/node2 -p 8984

  SHELL

end
