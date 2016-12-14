# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "nginx" do |web|
    web.vm.box = "ubuntu/trusty32"
    web.vm.network "forwarded_port", guest:80, host: 8044
    web.vm.network "private_network", ip: "192.168.33.10"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    web.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y unzip git
      apt-get install -y nginx php5-fpm php5 php5-cli php5-curl php5-mysql php5-gd php5-mongo php5-redis php5-mcrypt
      cd /usr/share/nginx/
      rm -Rf *.zip phpMyAdmin* phpmyadmin
      wget --progress=dot:giga https://files.phpmyadmin.net/phpMyAdmin/4.6.5.2/phpMyAdmin-4.6.5.2-all-languages.zip -O phpMyAdmin-4.6.5.2-all-languages.zip
      unzip phpMyAdmin-4.6.5.2-all-languages.zip
      ln -s phpMyAdmin-4.6.5.2-all-languages phpmyadmin
      cp /vagrant/conf/nginx/default /etc/nginx/sites-available/default
      cp /vagrant/conf/phpmyadmin/config.inc.php /usr/share/nginx/phpmyadmin/config.inc.php
      cp /vagrant/conf/php/php.ini /etc/php5/fpm/php.ini
      service nginx restart

      cd /usr/local/bin
      curl -sS https://getcomposer.org/installer | php
      chmod +x composer.phar
      mv composer.phar composer
    SHELL
  end

  config.vm.define "apache" do |web|
    web.vm.box = "ubuntu/trusty32"
    web.vm.network "forwarded_port", guest:80, host: 8043
    web.vm.network "private_network", ip: "192.168.33.20"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    web.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y unzip git
      apt-get install -y apache2 libapache2-mod-php5 php5 php5-cli php5-curl php5-mysql php5-gd php5-mongo php5-redis php5-mcrypt
      service apache2 stop
      a2enmod rewrite
      cd /usr/share/
      rm -Rf *.zip phpMyAdmin* phpmyadmin
      wget --progress=dot:giga https://files.phpmyadmin.net/phpMyAdmin/4.6.5.2/phpMyAdmin-4.6.5.2-all-languages.zip -O phpMyAdmin-4.6.5.2-all-languages.zip
      unzip phpMyAdmin-4.6.5.2-all-languages.zip
      ln -s phpMyAdmin-4.6.5.2-all-languages phpmyadmin
      cp /vagrant/conf/apache2/000-default.conf /etc/apache2/sites-available/000-default.conf
      cp /vagrant/conf/phpmyadmin/config.inc.php /usr/share/phpmyadmin/config.inc.php
      service apache2 start

      cd /usr/local/bin
      curl -sS https://getcomposer.org/installer | php
      chmod +x composer.phar
      mv composer.phar composer
    SHELL
  end

  config.vm.define "mysql" do |db|
    db.vm.box = "ubuntu/trusty32"
    db.vm.network "private_network", ip: "192.168.33.30"
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    db.vm.provision "shell", inline: <<-SHELL
      apt-get update
      locale-gen UTF-8
      echo "mysql-server-5.6 mysql-server/root_password password password" | debconf-set-selections
      echo "mysql-server-5.5 mysql-server/root_password_again password password" | debconf-set-selections
      apt-get -y install mysql-server-5.6
      mysql -u root -ppassword < /vagrant/conf/mysql/000-application.sql
      sed -i 's/bind-address/#bind-address/g' /etc/mysql/my.cnf
      service mysql restart
    SHELL
  end

  config.vm.define "nosql" do |db|
    db.vm.box = "ubuntu/trusty32"
    db.vm.network "private_network", ip: "192.168.33.40"
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    db.vm.provision "shell", inline: <<-SHELL
      apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
      echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/mongodb.list
      apt-get update
      apt-get install -y mongodb-org
      sed -i 's/bind_ip/#bind-ip/g' /etc/mongod.conf
      service mongod restart

      apt-get install -y redis-server
      sed -i 's/bind 127.0.0.1/bind 0.0.0.0/g' /etc/redis/redis.conf
      service redis-server restart
    SHELL
  end
end
