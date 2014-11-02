# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Bootstrap our Vagrant environment
  config.vm.provision "shell", path: "bin/bootstrap.sh"

  # TODO: After `bin/bootstrap.sh`, set password to `password`, update pgpass, and see if it was an encoding issue (otherwise it is prob PGPASSWORD no longer existing which is silly)

  # Notify the user with instruction on how to develop
  $notify_user = <<SCRIPT
  cat /vagrant/lib/notify-user.md
SCRIPT
  config.vm.provision "shell", inline: $notify_user
end
