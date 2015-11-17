# -*- mode: ruby -*-
# vi: set ft=ruby :

# GitHub token file (gitignored)
# NOTICE: Please create this file and write GitHub token only in one line
#         (Only one newline character is acceptable)
GITHUB_TOKEN_FILEPATH = "./github_token"

# IP addresses on host-only network
vagrant_ipaddr_consul_server  = "192.168.33.10"
vagrant_ipaddr_mariadb_server = "192.168.33.20"
vagrant_ipaddr_manager        = "192.168.33.30"
vagrant_ipaddr_cdh_quickstart = "192.168.33.40"

# Basic Consul settings
consul_ipaddr_join = "192.168.33.10"

# MariaDB server settings
# Auto-gerenated root password
mariadb_root_password = (0...8).map{ (65 + rand(26)).chr }.join
# Username, password and database for sample manager application
mariadb_manager_user_name     = "manager"
mariadb_manager_user_password = "manager-password"
mariadb_manager_database      = "sample"
mariadb_manager_host          = vagrant_ipaddr_manager

# Sample manager settings
manager_nginx_server_name = "manager.example.com"
manager_git_version       = "master"

# GitHub settings
if File.exists?(GITHUB_TOKEN_FILEPATH)
  github_token = File.open(GITHUB_TOKEN_FILEPATH).read.chomp
end

Vagrant.configure(2) do |config|
  # Basic settings
  config.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "512"
    vb.cpus = 1
  end

  # Consul Server
  config.vm.define "consul1" do |c|
    c.vm.hostname = "consul1"
    c.vm.network "private_network", ip: vagrant_ipaddr_consul_server

    c.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/consul_servers.yml"
      ansible.extra_vars = {
        consul: {
          is_server: true,
          bootstrap_expect: 1,
          ipaddr_bind: vagrant_ipaddr_consul_server
        }
      }
    end
  end
  
  # MariaDB Server
  config.vm.define "mariadb1" do |c|
    c.vm.hostname = "mariadb1"
    c.vm.network "private_network", ip: vagrant_ipaddr_mariadb_server

    c.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/mariadb_servers.yml"
      ansible.extra_vars = {
        consul: {
          ipaddr_join: consul_ipaddr_join,
          ipaddr_bind: vagrant_ipaddr_mariadb_server
        },
        mariadb: {
          root: {
            password: mariadb_root_password
          },
          normal_users: [
                         {
                           name: mariadb_manager_user_name,
                           password: mariadb_manager_user_password,
                           database: mariadb_manager_database,
                           host: mariadb_manager_host
                         }
                        ],
          databases: [ mariadb_manager_database ]
        }
      }
    end
  end

  # Sample Manager Server
  config.vm.define "manager1" do |c|
    c.vm.hostname = "manager1"
    c.vm.network "private_network", ip: vagrant_ipaddr_manager

    c.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end

    c.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/managers.yml"
      ansible.extra_vars = {
        consul: {
          ipaddr_join: consul_ipaddr_join,
          ipaddr_bind: vagrant_ipaddr_manager
        },
        manager: {
          nginx: {
            server_name: manager_nginx_server_name
          },
          git: {
            version: manager_git_version
          },
          composer: {
            github_token: github_token
          }
        }
      }
    end
  end

  # Cloudera QuickStart VM
  config.vm.define "cdh-quickstart" do |c|
    c.vm.box = "quickstart/cdh"

    c.vm.hostname = "quickstart.cloudera"
    c.vm.network "private_network", ip: vagrant_ipaddr_cdh_quickstart

    c.vm.provider "virtualbox" do |vb|
      vb.cpus = 2
      vb.memory = "8192"
    end

    c.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/cdh_quickstart.yml"
      ansible.extra_vars = {
        consul: {
          ipaddr_join: consul_ipaddr_join,
          ipaddr_bind: vagrant_ipaddr_cdh_quickstart
        }
      }
    end
  end
end
