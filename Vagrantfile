# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Install dependencies
  $install_dependencies = <<SCRIPT
  if ! which curl &> /dev/null; then
    sudo apt-get install -y curl git postgresql
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
    npm install -loglevel http
  fi
SCRIPT
  config.vm.provision "shell", inline: $clone_repository

  # Running `npm start`
  # Attempting to get `postgres` working
  # So far we have a Vagrant user created but now we are getting a password error from `node` but not `psql`...
  # sudo su - postgres
    # psql
      # CREATE ROLE vagrant WITH SUPERUSER CREATEDB LOGIN;

  # Updated inside of `/etc/postgresql/9.1/main/pg_hba.conf`
  # local   all             all                                     peer
  # to
  # local   all             all                                     md5
  # DEV: We should create another user and configure that over `vagrant`.
  # DEV: Then, we can append a row for `local all nodebugme md5`
  # DEV: Unless we can use `md5` and auto-login via `.pgpass`

  # Restart PostgreSQL server
  # sudo /etc/init.d/postgresql restart

  # Successfully run `npm start`
  # TODO: We should be using `.pgpass` over environment variables
  # PGUSER="vagrant" PGDATABASE="nodebugme" PGPASSWORD="password" npm start
end
