# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # configure the virtual machine

  config.vm.box = "bento/ubuntu-16.04"
  config.vm.hostname="local"
  config.vm.synced_folder ".", "/var/www/local", owner: "www-data", group: "www-data"
  config.vm.synced_folder "./util", "/vagrant", mount_options: ["dmode=775,fmode=664"]
  config.vm.network "public_network"
  config.vm.network :private_network, ip: "192.168.73.13"

  # configure the virtualbox app

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus   = 2
    # virtualbox ioapic required (?) for multiple vcpu's
    v.customize ["modifyvm", :id, "--ioapic", "on"]
    # use the host's resolver instead of the ones set inside the vm (fixes slow dns resolconfigion)
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Provision the virtual machine

  config.vm.provision "shell", inline: <<-SHELL
     echo "Updating apt..."
     # update the system
     apt-get update >/dev/null 2>&1
     echo "Upgrading all components.. Will take a few minutes."
     DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" upgrade >/dev/null 2>&1
     # install nginx and php
     echo "Installing nginx and PHP"
     apt-get -y install nginx >/dev/null 2>&1
     apt-get -y install php-fpm >/dev/null 2>&1
     apt-get -y install php-xdebug >/dev/null 2>&1
     apt-get -y install php-mbstring >/dev/null 2>&1
     apt-get -y install php-memcached  >/dev/null 2>&1
     # configure php
     echo "Configuring PHP and nginx"
     cp /vagrant/configs/php.ini /etc/php/7.0/fpm/php.ini >/dev/null 2>&1
     # install memcache
     apt-get -y install memcached >/dev/null 2>&1
     # restart php
     service php7.0-fpm restart >/dev/null 2>&1
     # copy nginx config into place
     cp /vagrant/configs/kirby.nginx /etc/nginx/sites-available/kirby.conf >/dev/null 2>&1
     rm /etc/nginx/sites-enabled/* >/dev/null 2>&1
     ln -s /etc/nginx/sites-available/kirby.conf /etc/nginx/sites-enabled/kirby.conf
     service nginx reload >/dev/null
     # this is for SSL installation, to implement read: https://github.com/Neilpang/acme.sh
     # and https://github.com/Neilpang/acme.sh/wiki/How-to-use-Amazon-Route53-API
     # install acme.sh
     #curl https://get.acme.sh | sh
     echo "you can now load the test site"
  SHELL
end
