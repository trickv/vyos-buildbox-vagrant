# vi: set ft=ruby

$script = <<SCRIPT
#!/usr/bin/env bash

set -u
set -e

# Install VyOS archive key
apt-get -y install debian-archive-keyring
wget -O - http://vyos.net/so3group_maintainers.key | sudo apt-key add -

# Install SquashFS tools
# This gets installed from backports because we need a newer version
echo "deb http://backports.debian.org/debian-backports squeeze-backports main" > /etc/apt/sources.list.d/debian-backports.list
apt-get update
apt-get -y -t squeeze-backports install squashfs-tools

# Install vim (because I can't live without it)
apt-get -y install vim

# Install tmux because vagrant only lets me have one ssh session
apt-get -y install tmux

# Install build dependencies
apt-get -y install debian-archive-keyring git autoconf automake dpkg-dev live-helper syslinux genisoimage

echo "Now you're ready to proceed with fetching the build-iso source and building."
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "puppetlabs-debian-607-x64-vbox4210-nocm"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/debian-607-x64-vbox4210-nocm.box"

  config.vm.provider :virtualbox do |vb|
    #vb.gui = true
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  config.vm.provision "shell", inline: $script
end

