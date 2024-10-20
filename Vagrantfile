# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.ssh.insert_key = false

  config.vm.box = "debian/bookworm64"
  config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 dnsutils
    cp -v /vagrant/named /etc/default
    cp -v /vagrant/named.conf.options /etc/bind
    SHELL
   

  config.vm.define "master" do |master|
    master.vm.network "private_network", ip: "192.168.57.102"
    master.vm.provision "shell", inline: <<-SHELL
    cp -v /vagrant/named /etc/default
    cp -v /vagrant/dns.sistema.test /var/lib/bind
    cp -v /vagrant/rev.sistema.test /var/lib/bind
    systemctl restart named 
    SHELL
  end

  config.vm.define "slave" do |slave|
    slave.vm.network "private_network", ip: "192.168.57.102"
    slave.vm.provision "shell", inline: <<-SHELL
    cp -v /vagrant/named.conf.local.slave /etc/bind
    systemctl restart named 
    SHELL
  end
end
