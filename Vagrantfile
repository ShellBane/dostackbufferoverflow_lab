# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

        	        
	config.vm.define "windev" do |windev|
		windev.vm.box = "ShellBane/windev"
		windev.vm.network :forwarded_port, guest: 22, host: 4444, id: 'ssh'
		windev.vm.network "private_network", ip: "192.168.99.5"
		windev.vm.guest = :windows
		windev.vm.communicator = "winrm" 
		windev.winrm.password = "vagrant"
		windev.winrm.username = "administrator"
		windev.vm.synced_folder ".", "/vagrant"
		windev.vm.network :forwarded_port, guest: 3389, host: 3399, id: "rdp", auto_correct: true
		windev.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true
		windev.vm.provider "virtualbox" do |v, override|
			v.customize ["modifyvm", :id, "--memory", 4096]
			v.customize ["modifyvm", :id, "--cpus", 2]
			v.customize ["modifyvm", :id, "--vram", 99]
			v.customize ["setextradata", "global", "GUI/SuppressMessages", "all" ]
		end

		windev.vm.provision "shell", path: "scripts/configure-firewall.ps1", privileged: true
		windev.vm.provision "shell", path: "scripts/git.ps1", privileged: true
		windev.vm.provision "shell", path: "scripts/configure-mona.ps1", privileged: true
		windev.vm.provision "shell", path: "scripts/configure-windowsdefender.ps1", privileged: true

	end
	
	config.vm.define "kali" do |kali|
		kali.vm.box = "kalilinux/rolling"
		kali.vm.synced_folder ".", "/vagrant", type: "virtualbox"
		kali.vm.provider "virtualbox" do |vb|
			vb.memory = "4096"
			vb.cpus = "4"
		end
		kali.vm.network "private_network", ip: "192.168.99.6"
		kali.vm.provision "shell", inline: "sudo apt-get update -y"
		kali.vm.provision "shell", inline: "sudo /vagrant/scripts/passwd.exp vagrant vagrant"
		kali.vm.provision "shell", inline: "sudo /vagrant/scripts/passwd.exp root vagrant"
		kali.vm.provision "shell", inline: "sudo gem install evil-winrm"
	end
	
	config.vm.provision :shell do |shell|
		shell.inline = 'echo rebooting'
		shell.reboot = true
	end

end
