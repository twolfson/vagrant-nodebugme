# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Install dependencies
  $install_dependencies = <<SCRIPT
  if ! which curl &> /dev/null; then
    sudo apt-get install -y curl git
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_dependencies

  # Install node
  # https://github.com/nodesource/distributions/tree/5be3219f45d58f9356737a0b8a80fcc6f4cf38d0
  $install_node = <<SCRIPT
  if ! which node &> /dev/null; then
    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install -y nodejs
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_node

  # Clone `nodebugme/site` repository
  $clone_repository = <<SCRIPT
  cd /vagrant
  if ! test -d site &> /dev/null; then
    git clone https://github.com/nodebugme/site
    cd site
    npm install
  fi
SCRIPT
  config.vm.provision "shell", inline: $clone_repository
end
