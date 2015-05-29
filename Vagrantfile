# -*- mode: ruby -*-
# vi: set ft=ruby :

########################################################################################################################
# Configuration
########################################################################################################################

# Github Settings
github_username        = "deviffy"
github_repo            = "Devbox"
github_branch          = "master"
github_url             = "https://raw.githubusercontent.com/#{github_username}/#{github_repo}/#{github_branch}"
github_pat             = ""

# VM Settings
hostname               = "box.dev"
server_ip              = "192.168.100.10"
server_cpus            = "1"
server_memory          = "1024"
server_swap            = "2048"
server_timezone        = "Europe/Bucharest"
public_folder          = "/vagrant"

# PHP Settings
php_version            = "5.6"
hhvm                   = "false"

# Mysql Settings
maria_db               = "true"
mysql_version          = "5.6"
mysql_root_password    = "secret"
mysql_enable_remote    = "false"

# PostgreSQL Settings
pgsql_root_password    = "secret"

# MongoDB Settings
mongo_enable_remote    = "false"

# RabbitMQ Settings
rabbitmq_user          = "dev"
rabbitmq_password      = "secret"

# Blackfire Settings
blackfire_server_id    = ""
blackfire_server_token = ""

# Ruby Settings
ruby_version           = "latest"
ruby_gems              = [
  "sass"
]

# Composer Settings
composer_packages      = [
  "phpunit/phpunit:4.8.*"
]

# NodeJS Settings
nodejs_version         = "latest"
nodejs_packages        = [
  "grunt-cli",
  "gulp",
  "bower",
  "pm2",
  "yo"
]

# Sphinx Search Settings
sphinxsearch_version  = "rel22" # rel20, rel21, rel22, beta, daily, stable


########################################################################################################################
# Provision the VM
########################################################################################################################

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = false
  end

  # Config hostname
  config.vm.hostname = hostname

  # Configure network and port forwarding
  config.vm.network :private_network, ip: server_ip
  config.vm.network :forwarded_port, guest: 22, host: 22220
  config.vm.network :forwarded_port, guest: 80, host: 8000
  config.vm.network :forwarded_port, guest: 443, host: 4430
  config.vm.network :forwarded_port, guest: 1080, host: 1080
  config.vm.network :forwarded_port, guest: 3306, host: 3306
  config.vm.network :forwarded_port, guest: 5432, host: 5432
  config.vm.network :forwarded_port, guest: 5672, host: 5672
  config.vm.network :forwarded_port, guest: 6379, host: 6379
  config.vm.network :forwarded_port, guest: 13000, host: 13000
  config.vm.network :forwarded_port, guest: 27017, host: 27017

  # Config shared folder
  config.vm.synced_folder ".", "/vagrant"
  #  config.vm.synced_folder ".", "/vagrant",
  #          id: "core",
  #          :nfs => false,
  #          :mount_options => ['nolock,vers=3,udp,noatime']

  # If using VirtualBox
  config.vm.provider :virtualbox do |vb|
      vb.name = "devbox"
      vb.customize ["modifyvm", :id, "--cpus", server_cpus]
      vb.customize ["modifyvm", :id, "--memory", server_memory]
      vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
  end

  # If using VMWare Fusion
  config.vm.provider "vmware_fusion" do |vb, override|
    override.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
    vb.vmx["memsize"] = server_memory
  end

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box

#    config.cache.synced_folder_opts = {
#        type: :nfs,
#        mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
#    }
  end

  # Provision Base Packages
  config.vm.provision "shell", path: "#{github_url}/scripts/base.sh", args: [github_url, server_swap, server_timezone]

  # Provision Vim
  config.vm.provision "shell", path: "#{github_url}/scripts/vim.sh", args: github_url

  # Provision Screen
  config.vm.provision "shell", path: "#{github_url}/scripts/screen.sh"

  # Provision Supervisord
  config.vm.provision "shell", path: "#{github_url}/scripts/supervisord.sh"

  # Provision Apache
  config.vm.provision "shell", path: "#{github_url}/scripts/apache.sh", args: [server_ip, public_folder, hostname, github_url]

  # Provision Nginx
  # config.vm.provision "shell", path: "#{github_url}/scripts/nginx.sh", args: [server_ip, public_folder, hostname, github_url]

  # Provision PHP
  config.vm.provision "shell", path: "#{github_url}/scripts/php.sh", args: [server_timezone, hhvm, php_version]

  # Provision MariaDB
  config.vm.provision "shell", path: "#{github_url}/scripts/mariadb.sh", args: [mysql_root_password, mysql_enable_remote]

  # Provision MySQL
  # config.vm.provision "shell", path: "#{github_url}/scripts/mysql.sh", args: [mysql_root_password, mysql_version, mysql_enable_remote]

  # Provision PostgreSQL
  config.vm.provision "shell", path: "#{github_url}/scripts/pgsql.sh", args: pgsql_root_password

  # Provision MongoDB
  config.vm.provision "shell", path: "#{github_url}/scripts/mongodb.sh", args: mongo_enable_remote

  # Provision Couchbase
  # config.vm.provision "shell", path: "#{github_url}/scripts/couchbase.sh"

  # Provision CouchDB
  # config.vm.provision "shell", path: "#{github_url}/scripts/couchdb.sh"

  # Provision RethinkDB
  # config.vm.provision "shell", path: "#{github_url}/scripts/rethinkdb.sh", args: pgsql_root_password

  # Provision SQLite
  config.vm.provision "shell", path: "#{github_url}/scripts/sqlite.sh"

  # Provision Elasticsearch
  config.vm.provision "shell", path: "#{github_url}/scripts/elasticsearch.sh"

  # Provision SphinxSearch
  # config.vm.provision "shell", path: "#{github_url}/scripts/sphinxsearch.sh", args: [sphinxsearch_version]

  # Provision ElasticHQ
  config.vm.provision "shell", path: "#{github_url}/scripts/elastichq.sh"

  # Provision Memcached
  config.vm.provision "shell", path: "#{github_url}/scripts/memcached.sh"

  # Provision Redis
  config.vm.provision "shell", path: "#{github_url}/scripts/redis.sh", args: "persistent"

  # Provision Beanstalkd
  config.vm.provision "shell", path: "#{github_url}/scripts/beanstalkd.sh"

  # Provision RabbitMQ
  config.vm.provision "shell", path: "#{github_url}/scripts/rabbitmq.sh", args: [rabbitmq_user, rabbitmq_password]

  # Provision 0MQ
  #config.vm.provision "shell", path: "#{github_url}/scripts/zeromq.sh"

  # Provision Nodejs
  config.vm.provision "shell", path: "#{github_url}/scripts/nodejs.sh", privileged: false, args: nodejs_packages.unshift(nodejs_version, github_url)

  # Provision Ruby Version Manager (RVM)
  config.vm.provision "shell", path: "#{github_url}/scripts/rvm.sh", privileged: false, args: ruby_gems.unshift(ruby_version)

  # Provision Composer
  config.vm.provision "shell", path: "#{github_url}/scripts/composer.sh", privileged: false, args: composer_packages.join(" ")

  # Provision Mailcatcher
  config.vm.provision "shell", path: "#{github_url}/scripts/mailcatcher.sh"

  # Provision Laravel Envoy
  # config.vm.provision "shell", path: "#{github_url}/scripts/laravelenvoy.sh"

  # config.vm.provision "shell", path: "./local-script.sh"

end
