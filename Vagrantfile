# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
    end
  
  
    config.vm.define "inetRouter" do |inetRouter|
        inetRouter.vm.box = "centos/7"
        #inetRouter.vm.box_version = "20210210.0"  
        inetRouter.vm.hostname = "inetRouter"
        inetRouter.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "net_router"
        inetRouter.vm.network "private_network", adapter: 3, auto_config: false, virtualbox__intnet: "net_router"
        inetRouter.vm.network "private_network", ip: "192.168.56.10", adapter: 8
    end
  
    config.vm.define "centralRouter" do |centralRouter|
        centralRouter.vm.box = "centos/7"
        #centralRouter.vm.box_version = "20210210.0"      
        centralRouter.vm.hostname = "centralRouter"
        centralRouter.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "router-net"
        centralRouter.vm.network "private_network", adapter: 3, auto_config: false, virtualbox__intnet: "router-net"
        centralRouter.vm.network "private_network", ip: '192.168.255.9', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"
        centralRouter.vm.network "private_network", ip: '192.168.56.11', adapter: 8
    end

    config.vm.define "office1Router" do |office1Router|
        office1Router.vm.box = "centos/7"
        #office1Router.vm.box_version = "20210210.0"      
        office1Router.vm.hostname = "office1Router"
        office1Router.vm.network "private_network", ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"
        office1Router.vm.network "private_network", adapter: 3, auto_config: false, virtualbox__intnet: "vlan1"
        office1Router.vm.network "private_network", adapter: 4, auto_config: false, virtualbox__intnet: "vlan1"
        office1Router.vm.network "private_network", adapter: 5, auto_config: false, virtualbox__intnet: "vlan2"
        office1Router.vm.network "private_network", adapter: 6, auto_config: false, virtualbox__intnet: "vlan2"
        office1Router.vm.network "private_network", ip: '192.168.56.20', adapter: 8
    end

    config.vm.define "testClient1" do |testClient1|
        testClient1.vm.box = "centos/7"
        #testClient1.vm.box_version = "20210210.0"      
        testClient1.vm.hostname = "testClient1"
        testClient1.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"
        testClient1.vm.network "private_network", ip: '192.168.56.21', adapter: 8
    end
  
    config.vm.define "testServer1" do |testServer1|
        testServer1.vm.box = "centos/7"
        #testServer1.vm.box_version = "20210210.0"      
        testServer1.vm.hostname = "testServer1"
        testServer1.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"
        testServer1.vm.network "private_network", ip: '192.168.56.22', adapter: 8
    end
  
    config.vm.define "testClient2" do |testClient2|
        testClient2.vm.box = "ubuntu/focal64"
        testClient2.vm.box_version = "20220411.2.0"        
        testClient2.vm.hostname = "testClient2"       
        testClient2.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"
        testClient2.vm.network "private_network", ip: '192.168.56.31', adapter: 8
    end
  
    config.vm.define "testServer2" do |testServer2|
        testServer2.vm.box = "ubuntu/focal64"
        testServer2.vm.box_version = "20220411.2.0" 
        testServer2.vm.hostname = "testServer2"
        testServer2.vm.network "private_network", adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"
        testServer2.vm.network "private_network", ip: '192.168.56.32', adapter: 8

  
        testServer2.vm.provision "ansible" do |ansible|
          #ansible.verbose = "vvv"
          ansible.playbook = "ansible/provision.yml"
          ansible.inventory_path = "ansible/hosts"
          ansible.host_key_checking = "false"
          ansible.become = "true"
          ansible.limit = "all"
        end
    end
    
    config.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh
      cp ~vagrant/.ssh/auth* ~root/.ssh
    SHELL
    
end