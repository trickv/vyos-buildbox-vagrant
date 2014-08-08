# vi: set ft=ruby

$script = <<SCRIPT
#!/usr/bin/env bash

set -u
set -e

# Install Debian & VyOS archive keys
apt-get -y install debian-archive-keyring
wget -O - http://vyos.net/so3group_maintainers.key | sudo apt-key add -

# Install SquashFS tools
# This gets installed from backports because we need a newer version
echo "deb http://backports.debian.org/debian-backports squeeze-backports main" > /etc/apt/sources.list.d/debian-backports.list
apt-get update
apt-get -y -t squeeze-backports install squashfs-tools

# Install dev tools I like - pv
# Install tmux because vagrant only lets me have one ssh session
apt-get -y install vim ack-grep tmux

# Install build dependencies
apt-get -y install git autoconf automake dpkg-dev live-helper syslinux genisoimage

# Set path
set +e
fgrep -q '# VyOS buildbox' /etc/bash.bashrc
if [ $? -ne 0 ]; then
    echo "export PATH=/sbin:/usr/sbin:\\$PATH # VyOS buildbox" >> /etc/bash.bashrc
fi
echo "Now you're ready to proceed with fetching the build-iso source and building."
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "DebianSqueeze64"
  config.vm.box_url = "http://f.willianfernandes.com.br/vagrant-boxes/DebianSqueeze64.box"


  config.vm.provider :virtualbox do |vb|
    #vb.gui = true
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  config.vm.provision "shell", inline: $script
end

