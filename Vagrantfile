# -*- mode: ruby -*-
# vim: set ft=ruby :


MACHINES = {
  :R1 => {
        :box_name => "centos/8",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '10.0.0.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan12"},
				    {ip: '10.10.0.1', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan13"},
                ]
  },
  :R2=> {
        :box_name => "centos/8",
        :net => [
                    {ip: '10.0.0.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan12"},
				    {ip: '10.20.0.2', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan23"},
                ]
  },
  
  :R3 => {
        :box_name => "centos/8",
        :net => [
                    {ip: '10.10.0.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "vlan13"},
				    {ip: '10.20.0.1', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "vlan23"},
                ]
  },

}


class FixGuestAdditions < VagrantVbguest::Installers::Linux
    def install(opts=nil, &block)
        communicate.sudo("yum update kernel -y; yum install -y gcc binutils make perl bzip2 kernel-devel kernel-headers", opts, &block)
        super
    end
end

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
	config.vbguest.installer = FixGuestAdditions
    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end
		
		 box.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "256"]
          end
		  
		box.vm.provision "ansible" do |ansible|
			ansible.playbook = "playbook.yml"
			ansible.become = "true"
		end

      end

  end
  
  
end