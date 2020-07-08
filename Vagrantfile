# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.define vm_name = "apple" do |config|
      config.vm.hostname = "apple"
      config.vm.network :private_network, ip: "192.168.20.101"
  end

  config.vm.define vm_name = "banana" do |config|
    config.vm.hostname = "banana"
    config.vm.network :private_network, ip: "192.168.30.101"
  end
end
