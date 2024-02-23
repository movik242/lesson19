# -*- mode: ruby -*-
# vi: set ft=ruby :
# export VAGRANT_EXPERIMENTAL="disks"

Vagrant.configure("2") do |config|

config.vm.define "pxeserver" do |server|
  server.vm.box = 'centos8'
  
  server.vm.host_name = 'pxeserver'
  server.vm.network :private_network, 
                     ip: "10.10.10.20", 
                     virtualbox__intnet: 'pxenet'

  server.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

     server.vm.provision "shell", inline: <<-SHELL
          sudo yum install -y epel-release httpd
          sudo setenforce 0  
          
      SHELL
    
  end


# config used from this
# https://github.com/eoli3n/vagrant-pxe/blob/master/client/Vagrantfile
  config.vm.define "pxeclient" do |pxeclient|
    pxeclient.vm.box = 'centos8'
    pxeclient.vm.host_name = 'pxeclient'
    
    pxeclient.vm.provider :virtualbox do |vb|
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize [
          'modifyvm', :id,
          '--nic1', 'intnet',
          '--intnet1', 'pxenet',
          '--nic2', 'nat',
          '--boot1', 'net',
          '--boot2', 'none',
          '--boot3', 'none',
          '--boot4', 'none'
        ]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
      # ENABLE to fix memory issues
#     end
  end

end