# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "centos/7",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                ]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
                   {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "mgt-net"},
		   {ip: '192.168.255.5', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "of1-cent"},
		   {ip: '192.168.255.9', adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "of2-cent"},
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                ]
  },

  :office1router => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.255.6', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "of1-cent"},
		   {ip: '192.168.2.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "of1-dev"},
		   {ip: '192.168.2.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "of1-test"},
		   {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "of1-managers"},
		   {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "of1-hardware"},
                ]
  },

  :office1Server => {
        :box_name => "centos/7",
        :net => [
		{ip: '192.168.2.194', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "of1-hardware"},
                ]
  },

  :office2router => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "of2-cent"},
                   {ip: '192.168.1.1', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "of2-dev"},
                   {ip: '192.168.1.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "of2-test"},
                   {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "of2-hardware"},
                ]
  },

  :office2Server => {
        :box_name => "centos/7",
        :net => [
                {ip: '192.168.1.194', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "of2-hardware"},
                ]
  },
  
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
       if box.vm.host_name == "office2Server"
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = "play.yml"
            #ansible.inventory_path = "ansible/hosts"
            ansible.host_key_checking = "false"
            ansible.limit = "all"
            ansible.become = "true"
          end
       end

      end

  end
  
end
