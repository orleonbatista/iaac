# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', disabled: false
  config.vm.define "iaac" do |iaac|
	  iaac.vm.box = "ubuntu/focal64"
	  iaac.vm.hostname = "iaac-station"
	  iaac.vm.provision "shell", path: "iaac.sh", run: "once"
          iaac.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa']
          iaac.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
	  iaac.vm.provision "shell", inline: <<-EOC
                sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
                echo "finished"
		EOC
          iaac.vm.network :private_network, ip: "10.0.0.10"
	  iaac.vm.provider "virtualbox" do |v|
	        v.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    		v.cpus = 2
	        v.memory = 4096
          end
  end
  config.ssh.insert_key = false
  config.vm.boot_timeout = 800
  config.vm.synced_folder '.', '/vagrant', disabled: false
  config.vm.define "server" do |server|
     server.vm.box = "ubuntu/focal64"
     server.vm.hostname = "server"
     server.vm.network :private_network, ip: "10.0.0.11"
     server.vm.provision "shell", path: "server.sh", run: "once"
     server.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa']
     server.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
     server.vm.provision "shell", inline: <<-EOC
	sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
        echo "finished"
        EOC
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
