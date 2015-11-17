# hashicorp-sample

Sample configuration of HashiCorp tools

## Overview

The goal of this sample configuration is to evaluate the usefulness of HashiCorp tools in practical situations. The current version uses only a part of HashiCorp tools:

* Vagrant
* Consul
* Atlas

By using this configuration, four or more VMs are deployed automatically on local machine by Ansible. Consul agents run on these VMs and join one Consul cluster through the Consul server or Atlas.

* manager1, manager2, ...
    * Sample web application based on FuelPHP
    * Composer requires "github_token" file which contains GitHub token
* mariadb1
    * MariaDB server
    * This server allows connection from manager[1..] only
* consul1
    * Consul server
    * When Atlas-integration is enabled, the server requires "atlas_token" file which contains Atlas token
* cdh-quickstart
    * Cloudera QuickStart VM
    * This configuration includes Impala service definition for Consul

The IP address of each VM, the number of sample web applications and some software configurations are customizable. Please read the beginning of Vagrantfile for more information about the configurations.

## System Requirements

The following software must be installed for deploying VMs by this configuration:

* VirtualBox
* Vagrant
* Ansible

This configuration was tested in the following environment:

* MacBook Pro (Retina, 15-inch, Mid 2014)
    * CPU: 2.2 GHz Intel Core i7
    * Memory: 16 GB 1600 MHz DDR3
* OS X Yosemite 10.10.5
* VirtualBox 5.0.10
* Vagrant 1.7.4
* Ansible 1.9.4

## References

* "consulのinitスクリプトとsystemdユニットファイルを書いてみた - Qiita":http://qiita.com/yunano/items/7ef5fa5670721de55627 (in Japanese)
    * Scripts for starting Consul agent on CentOS 6 and 7
* "Vagrant box quickstart/cdh | Atlas by HashiCorp":https://atlas.hashicorp.com/quickstart/boxes/cdh
    * Box file of Cloudera QuickStart VM 5.4.2

## License

The MIT License (MIT)
