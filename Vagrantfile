# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # TODO: Move all scripts (except for echo) into `bin/bootstrap.sh`

  # TODO: After `bin/bootstrap.sh`, set password to `password`, update pgpass, and see if it was an encoding issue (otherwise it is prob PGPASSWORD no longer existing which is silly)

  # Install dependencies
  $install_dependencies = <<SCRIPT
  if ! which curl &> /dev/null; then
    sudo apt-get update
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
  echo_command="psql --db postgres --command \\"SELECT 'hai';\\""
  if ! sudo su --command "$echo_command" vagrant &> /dev/null; then
    # Set up `vagrant` user in PostgreSQL
    create_user_command="psql --command \\"CREATE ROLE vagrant WITH SUPERUSER CREATEDB LOGIN;\\""
    sudo su --command "$create_user_command" postgres
    psql_password="R0cKeT^TuRtl3."
    set_password_command="psql --command \\"ALTER ROLE vagrant WITH PASSWORD \\\\\\"$psql_password\\\\\\"\\""

    # Require that `vagrant` must provide a password in addition to `peer` authentication
    # DEV: This fixes an issue with `pg.js` where `peer` authentication doesn't seem to work
    # DEV: Technically we should have explicitly list permissions when outputting to `pg_hba.conf` but `sudo` doesn't apply
    #   and I don't know of a short equivalent
    cat >> /etc/postgresql/9.1/main/pg_hba.conf <<EOF

# In addition to \\`peer\\` authentication, require that \\`vagrant\\` provides a password at login
local   all             vagrant                                     md5
EOF

    # Restart PostgreSQL server
    sudo /etc/init.d/postgresql restart

    # Set up pgpass (allows password-less login for `vagrant` via `pg.js` and once logged in)
    echo "localhost:5432:nodebugme:vagrant:$psql_password" > /home/vagrant/.pgpass
    chmod 0600 /home/vagrant/.pgpass
    chown vagrant:vagrant /home/vagrant/.pgpass
  fi
SCRIPT
  config.vm.provision "shell", inline: $setup_postgresql

  # Notify the user with instruction on how to develop
  $notify_user = <<SCRIPT
  cat /vagrant/lib/notify-user.md
SCRIPT
  config.vm.provision "shell", inline: $notify_user  # npm start
end
