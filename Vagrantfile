# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: false
  config.vm.define "iaac" do |iaac|
	  iaac.vm.box = "ubuntu/focal64"
	  iaac.vm.hostname = "iaac-station"
	  iaac.vm.provision "shell", path: "iaac.sh", run: "once"
          iaac.vm.network :private_network, ip: "10.0.0.10"
	  iaac.vm.provider "virtualbox" do |v|
	        v.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    		v.cpus = 2
	        v.memory = 4096
          end
  end
  config.vm.synced_folder '.', '/vagrant', disabled: false
  config.vm.define "server" do |server|
     server.vm.box = "ubuntu/focal64"
     server.vm.hostname = "server"
     server.vm.network :private_network, ip: "10.0.0.11"
     server.vm.provision "shell", path: "server.sh", run: "once"
     server.vm.network "forwarded_port", guest: 8000, host: 8000
     server.vm.network "forwarded_port", guest: 8081, host: 8081
     server.vm.network "forwarded_port", guest: 80, host: 8080
     server.vm.provider "virtualbox" do |vb|
	        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
                vb.cpus = 2
                vb.memory = 4096
	end
  end
end
