# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network "forwarded_port", guest: 8080, host: 8080

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

  # Set up PostgreSQL configuration
  $setup_postgresql = <<SCRIPT
  # If we can't open `psql` as `vagrant`
  echo_command="psql --db postgres --command \"SELECT 'hai';\""
  if ! sudo su --command "$echo_command" postgres &> /dev/null; then
    # Set up `vagrant` user in PostgreSQL
    create_user_command="psql --command \"CREATE ROLE vagrant WITH SUPERUSER CREATEDB LOGIN;\""
    sudo su --command "$create_user_command" postgres
    psql_password="R0cKeT^TuRtl3."
    set_password_command="psql --command \"ALTER ROLE vagrant WITH PASSWORD \\\"$psql_password\\\"\""

    # Updated inside of `/etc/postgresql/9.1/main/pg_hba.conf`
    # local   all             all                                     peer
    # to
    # local   all             all                                     md5
    # DEV: We should create another user and configure that over `vagrant`.
    # DEV: Then, we can append a row for `local all nodebugme md5`
    # DEV: Unless we can use `md5` and auto-login via `.pgpass`

    # Restart PostgreSQL server
    # sudo /etc/init.d/postgresql restart

    # Set up pgpass
    # echo "localhost:5432:nodebugme:vagrant:password" > /home/vagrant/.pgpass
    # chmod 0600 /home/vagrant/.pgpass
    # chown vagrant:vagrant /home/vagrant/.pgpass
  fi
SCRIPT

  # Successfully run `npm start`
  # npm start
end
