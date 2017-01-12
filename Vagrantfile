# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?('vagrant-hostmanager')
  warn "[\e[1m\e[31mERROR\e[0m]: Please run: vagrant plugin install vagrant-hostmanager"
  exit -1
end

Vagrant.configure('2') do |config|

  config.vm.box_check_update = false

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.define :wbgoat, {:primary => true} do |wbgoat|
    wbgoat.vm.box = 'ubuntu/trusty64'
    wbgoat.vm.hostname = :wbgoat

    # Network
    wbgoat.vm.network :private_network, :ip => '11.11.11.100', :netmask => '255.255.0.0'
    wbgoat.vm.network :forwarded_port, host: 10000, guest: 80, id: "webgoat"

    wbgoat.vm.provider :virtualbox do |vb|
      vb.name = 'Webgoat server'
      vb.gui = false
      vb.customize ['modifyvm', :id, '--memory', '1024', '--nicpromisc2', 'allow-all', '--groups', '/Webgoat']
    end

    wbgoat.vm.provision :docker, type: :docker do |docker|
      docker.pull_images 'webgoat/webgoat-container'
      docker.run 'webgoat', image: 'webgoat/webgoat-container', args: '-p 80:8080'
    end

    wbgoat.vm.provision :hostmanager
  end
end
