# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  (1..3).each do |i|
    config.vm.define "redis#{i}" do |o|
      o.vm.box = "centos/6"
      o.vm.hostname = "redis#{i}"
      o.vm.network "private_network", ip: "172.16.18.#{i}", netmask: "255.240.0.0"
      o.vm.provision :shell, inline: "sed 's/127\.0\.0\.1.*redis.*/172\.16\.18\.#{i} redis#{i}/' -i /etc/hosts"
      o.vm.provider "virtualbox" do |v|
        v.name = "redis#{i}"
        v.memory = 1024
        v.gui = false
      end
    end
  end
end
