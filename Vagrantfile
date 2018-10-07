# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  # CentOS 7 Vagrant box
  config.vm.box = "centos/7"

  # Install Singularity (and Vim!) for local development purposes 
  config.vm.provision "shell", inline: <<-SHELL
    yum install -y epel-release screen vim
    yum install -y singularity
  SHELL

end
