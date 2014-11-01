# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Install node
  # https://github.com/nodesource/distributions/tree/5be3219f45d58f9356737a0b8a80fcc6f4cf38d0
  $install_node = <<SCRIPT
  if ! which node &> /dev/null; then
    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install -y nodejs
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_node
end
