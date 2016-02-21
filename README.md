## Description

This is an `Ansible` role to configure a CentOS/RHEL/Oracle Linux 7.1 server with Oracle 12c R1 Enterprise Edition Database.

## Role variable

- `oracle_version` - Oracle database version (default: `12.1.0.2`)ignore_errors: true
- `oracle_archives_directory` - Path to temporary directory where to download installer archives (default: `/tmp/oracledb_installer`).
- `oracle_archives_files` - List of downloaded archives files within then `oracledb_archives_directory` temporary forlder.

## Usage

For test purposes with `Vagrant`, you have to install the `vagrant-hostmanager` plugin:

	$ vagrant plugin install vagrant-hostmanager

This plugin will setup, after VM boot, the hosts file of the VM's system. This configuration is required for Oracle server installation. If the host FQDN is not declared, Oracle installer will throw an error.

Next, download from [Oracle website](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html) the installation files:

- linuxamd64_12102_database_1of2.zip
- linuxamd64_12102_database_2of2.zip

and put them into `oracledb_archives_directory` folder and update the `oracledb_archives_files` list items. Now, perform the VM provisioning:

	$ vagrant up

After a few minutes a virtual machine with Oracle Database will be ready for you without any further configuration. You can access the Enterprise Manager Express using `sys` (default sys dba username)  and `oracle` as password:

https://the_vm_fqdn:5500/em

You can shutdown the virtual machine using:

	$ vagrant halt

You can destroy it (delete from disk) using:

	$ vagrant destroy

Password for `oracle` system user is `welcome1`.
