# Disable the creation of parallel machines
# ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure("2") do |config|

	NumberNodes = 2

	(1..NumberNodes).each do |i|

		config.vm.define "node#{i}" do |node|
			node.vm.box = "almalinux/9"
			node.vm.hostname = "k8s-node#{i}"
			node.vm.provider :libvirt do |libvirt|
				libvirt.cpus = 8
				libvirt.memory = 12288
			end

			node.vm.provision "shell", 
				inline: " \
					sudo dnf update -y && \
					sudo yum install epel-release -y && \
					sudo yum install epel-release -y && \
					sudo yum install ansible -y && \
					sudo ansible-playbook /vagrant/install-k8s.yml
				"

			# node.vm.provision "ansible" do |ansible|
			# 	ansible.verbose = "v"
			# 	ansible.playbook = "/vagrant/install-k8s.yml"
			# end
		end

	end

end