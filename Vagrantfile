# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "private_network", ip: "10.1.2.3"
  # the host path will be created if it does not exist.
  config.vm.synced_folder "./www", "/var/www", create: true, owner: "www-data", group: "www-data"

  # Bug with DNS http://serverfault.com/a/453260
  # Also improves network performance https://github.com/mitchellh/vagrant/issues/1807
  config.vm.provider "virtualbox" do |v|
    v.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    v.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end

  # Read line from file and remove line breaks
  hosts = File.readlines("./vhosts-list").map(&:chomp)
  # Filter out comments starting with "#"
  hosts.grep(/\A[^#]/)

  # Pass the found host names to the hostsupdater plugin so it can perform magic.
  config.hostsupdater.aliases = hosts

  config.vm.provision :shell, :path => "provision.sh"


end
