# -*- mode: ruby -*-
# vi: set ft=ruby :
#
#
Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.5"

  #config.vm.network "forwarded_port", guest: 443, host: 9443

  config.vm.network "forwarded_port", guest: 8983, host: 8983
  config.vm.network "forwarded_port", guest: 8984, host: 8984

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
    yum install -y java-11-openjdk

    # setup solr as vagrant user
    su vagrant << EOF
      cd /home/vagrant
 
      # setup 2 Solr nodes

      mkdir -p example/nodes/node1
      mkdir -p example/nodes/node2

      cp /vagrant/solr/server/solr/solr.xml example/nodes/node1/.
      cp /vagrant/solr/server/solr/solr.xml example/nodes/node2/.

      # Start the nodes
      /vagrant/solr/bin/solr start -s example/nodes/node1 -p 8983 -a '-Dsolr.disable.shardsWhitelist=true'
      /vagrant/solr/bin/solr start -s example/nodes/node2 -p 8984 -a '-Dsolr.disable.shardsWhitelist=true'

      # create Solr cores
      /vagrant/solr/bin/solr create_core -c core1 -p 8983 -d sample_techproducts_configs
      /vagrant/solr/bin/solr create_core -c core1 -p 8984 -d sample_techproducts_configs

      # upload a document
      /vagrant/solr/bin/post -c core1 /vagrant/solr/example/exampledocs/monitor.xml -port 8983
      /vagrant/solr/bin/post -c core1 /vagrant/solr/example/exampledocs/monitor2.xml -port 8984
      
      # perform some searches
      curl http://localhost:8983/solr/core1/select?q=*:*
      curl http://localhost:8984/solr/core1/select?q=*:*

      # perform a distributed search
      curl http://localhost:8983/solr/core1/select?q=*:*\&shards=localhost:8983/solr/core1,localhost:8984/solr/core1\&fl=id,name

EOF

  SHELL

end
