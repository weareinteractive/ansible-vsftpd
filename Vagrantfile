# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vbguest.no_remote = true
  config.vbguest.auto_update = false
  config.vm.network :private_network, ip: "192.168.33.99"

  config.vm.define 'trusty' do |instance|
    instance.vm.box = 'ubuntu/trusty64'
  end

  config.vm.define 'xenial' do |instance|
    instance.vm.box = 'ubuntu/xenial64'
  end

  config.vm.define 'jessie' do |instance|
    instance.vm.box = 'debian/jessie64'
  end

  config.vm.define 'strecth' do |instance|
    instance.vm.box = 'debian/stretch64'
  end

  # View the documentation for the provider you're using for more
  # information on available options.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "tests/main.yml"
    ansible.verbose = 'vv'
    ansible.become = true
  end
end
