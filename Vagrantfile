Vagrant.configure("2") do |config|
	config.vm.define "master" do |master|
		master.vm.box = "almalinux/9"
		master.vm.provider :libvirt do |libvirt|
			libvirt.cpus = 4
    			libvirt.memory = 2048
    		end
	end
	
	config.vm.define "node1" do |node1|
    		node1.vm.box = "almalinux/9"
    		node1.vm.provider :libvirt do |libvirt|
			libvirt.cpus = 4
    			libvirt.memory = 2048
    		end
  	end
end