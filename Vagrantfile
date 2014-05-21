# -*- coding: utf-8 -*-
# -*- mode: ruby -*-
# vi: set ft=ruby :
current_dir = File.dirname(__FILE__)
cookbook_testers = {
  "base" => {
    :hostname => "host-base",
    :ipaddress => "172.16.13.5",
  },
 "centos64" => {
    :hostname => "host-centos64",
    :ipaddress => "172.16.13.10",
  },
 "test" => {
    :hostname => "host-base-package-ubuntu",
    :ipaddress => "172.16.13.15",
  },
  "precise64"=>{
    :hostname => "host-base-package-ubuntu-precise",
    :ipaddress => "172.16.13.20",
    :box_url=>"d:/precise-server-cloudimg-amd64-vagrant-disk1.box"
   }
}
HELLO_SCRIPT = <<EOF
echo "=================================================="
echo "I am provisioning..."
echo "current time:"$(date)
echo "=================================================="
EOF

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |global_config|
  #cookbook_testers.each_pair do |name, options|
   cookbook_testers.each_with_index do |(name, options),index|
   global_config.vm.define name do |config|
      puts 'index:'+index.to_s
      
      port_map_base=index*10000
      puts 'prot_map_base:'+port_map_base.to_s
      #vm_name = "box-"+name.to_s
      vm_name = name.to_s
      ipaddress = options[:ipaddress]
      hostname = options[:hostname]

      config.vm.box =vm_name  
      config.vm.host_name = hostname 

	#地址 ,外部可add box
     #config.vm.box_url = "http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.3-x86_64-v20130101.box"
	config.vm.box_url=options[:box_url] if options.has_key? :box_url

      #config.vm.boot_mode = :headless
      #config.vm.share_folder("../data", "/share-data", ".",:disabled => false)
      #config.vm.share_folder("../data", "/share-data", :disabled => false)
      #config.vm.share_folder "../data", "/vagrant-data","../data",:disabled=>false
	config.vm.synced_folder "../data", "/vagrant-data"
      #config.vm.network :hostonly, ipaddress

      #config.vm.network "private_network", ip:ipaddress
	config.vm.network  "public_network",ip:ipaddress

      config.vm.network :forwarded_port, guest: 80, host: (80+port_map_base)

	 # Provider-specific configuration so you can fine-tune various
	  # backing providers for Vagrant. These expose provider-specific options.
	  # Example for VirtualBox:
	  #
	   config.vm.provider :virtualbox do |vb|
	     # Don't boot with headless mode
	     vb.gui = false
	  
	     # Use VBoxManage to customize the VM. For example to change memory:
            vb.customize ["modifyvm", :id, "--memory", "512","--cpus", "2","--cpuexecutioncap", "50"]
	   end


      config.vm.provision "shell", inline:HELLO_SCRIPT 
 # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxMana]
#     config.vm.provision "docker" do |d|
#           d.pull_images "ubuntu"
#           d.pull_images "vagrant"
#           d.run "rabbitmq"
#     end
#     config.vm.provision "puppet" do |puppet|
#           puppet.options = "--verbose --debug"
#     end
    end
  end
end
