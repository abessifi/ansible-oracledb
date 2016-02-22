## Description

This is an `Ansible` role to configure a CentOS/RHEL/Oracle Linux 7.1 server with Oracle 12c R1 Enterprise Edition Database.

## Usage

For test purposes with `Vagrant`, you have to install the `vagrant-hostmanager` plugin:

	$ vagrant plugin install vagrant-hostmanager

This plugin will setup, after VM boot, the '/etc/hosts' file of the VM's system. This configuration is required for Oracle server installation. If the host FQDN is not declared, Oracle installer will throw an error.

Update the host's file `/etc/hosts` and add an entry for the Vagrant VM `oracle-db-vm` as follow:

	192.168.33.102  oracle-db-vm

> **PS:** If you update the VM ip address in the Vagrantfile, you have to do the same thing in the `/etc/hosts` file in order to keep the `oracle-db-vm` reachable.

Download the Oracle 12c installer files from [Oracle website](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html):

- linuxamd64_12102_database_1of2.zip
- linuxamd64_12102_database_2of2.zip

and put them into `/tmp/oracledb_installer` (default directory).

Update your inventory variables, when needed, to override the role default variables. For instance, if you want to:

- download oracle installer files 'foobar_1of2.zip' and 'foobar_2of2.zip'
- put them into '/foo/bar' directory
- install them under '/baz/quax' folder (VM side)
- set the `oracle` system user password to 'changeme' (use the command `mkpasswd --method=SHA-512` to generate an encrypted password)
- set the database global name to 'db.foo.bar'
- run the DB listener on port '2115' (instead of the default 1521)

add the following variables to the `host_vars/oracle-db` inventory file:

	# Oracle Installation params
	oracle_archives_directory: '/foo/bar'
	oracle_archives_files:
	  - foobar_1of2.zip
	  - foobar_2of2.zip
	oracle_base_dir: '/baz/quax'
	oracle_system_user_password: '$6$AbIiQcyxOZA$yXm3R2qLAx....'
	oracle_install:
	  db_listener_port: 2115
      db_globalname: 'db.foo.bar'

> To secure the content of your inventory variables files, [`ansible-vault`](http://docs.ansible.com/ansible/playbooks_vault.html) is your friend !

Now, provision the VM:

	$ vagrant up

After a few minutes a virtual machine with Oracle Database will be ready for you without any further configuration. You can access the Enterprise Manager Express using `sys` (default sys dba username)  and `oracle` as password:

https://the_vm_fqdn:5500/em

To access the `sqlplus` console as `sysdba`:

- first, connect to the virtual machine with `vagrant ssh` command
- open a shell for `oracle` user (default password is `welcome1`)
- type: `sqlplus / as sysdba`

You can shutdown the virtual machine:

	$ vagrant halt

or completely destroy it (delete from disk):

	$ vagrant destroy

## Role variables

- `oracle_version` - Oracle database version (default: `12.1.0.2`)
- `oracle_edition` - The Oracle database edition (default: 'EE')
- `oracle_archives_directory` - Path to temporary directory where to download installer archives (default: `/tmp/oracledb_installer`)
- `oracle_archives_files` - List of downloaded archives files within then `oracledb_archives_directory` temporary forlder.
- `oracle_system_user_password` - The digest of the encrypted `oracle` user password (default: 'welcome1' encrypted)
- `oracle_base_dir` - Oracle base directory (default: '/u01/app')
- `oracle_installation_dir` -  Oracle installer directory (default: '/u01/app/installation')
- `oracle_server_hostname` -  FQDN of oracle server machine (default: 'oracle-db-vm')
- `oracle_db_name` - Oracle database name (default: 'oradb')
- `oracle_home_dir` -  Database home directory (default: '/u01/app/oracle/product/12.1.0.2/oradb')
- `oracle_install.mode` - The installation mode option  (default: 'INSTALL_DB_SWONLY')
- `oracle_install.db_type` - The type of database to create (default: 'GENERAL_PURPOSE')
- `oracle_install.db_globalname` - The starter global Database name (default: 'starter.oradb.private')
- `oracle_install.db_sid` - The starter database SID (default: 'ora')
- `oracle_install.db_listener_port` - The database listener port (default: 1521)
- `oracle_install.em_express_port` - Enterprise Manager access port (default: 5500)
- `oracle_install.instance_name` - Oracle instance name (default: 'orainst')
- `oracle_install.create_cdb` - Flag to create database as container database (default: 'true')
- `oracle_install.config_as_container_db` - Specify whether the database should be configured as a Container database (default: 'true')
- `oracle_install.pdb_name` - Pluggable Database name for the pluggable database in Container Database (default: 'pdb_01')
- `oracle_install.total_pdbs` - The number of pdb to be created (default: '1')
- `oracle_install.pdb_prefix` - The prefix name to use if one or more pdb need to be created (default: 'pdb')
- `oracle_install.sys_dba_username` - Username with DBA role (default: 'sys')
- `oracle_install.starter_db_common_password` - The password that is to be used for all schemas in the starter database (default: 'oracle')
- `oracle_install.management_option` - The management option to use for managing the database (default: 'DEFAULT')
- `oracle_install.enable_recovery` - This variable is to be set to 'true' if database recovery is required (default: 'true')
- `oracle_install.storage_type` - The type of storage to use for the database (default: 'FILE_SYSTEM_STORAGE')
- `oracle_install.inventory_location` -  The location which holds the inventory files (default: '/u01/app/inventory')
- `oracle_install.data_location` - The database file location which is a directory for datafiles, control files, redo logs (default: /u01/app/oradata')
- `oracle_install.recovery_location` - The recovery location (default: '/u01/app/recovery_area')
- `oracle_install.decline_security_updates` - Specify whether user doesn't want to configure security updates (default: 'no')
- `oracle_install.install_samples` - This variable controls whether to load Example Schemas onto the starter database (default: 'false')
- `oracle_install.memory_option` - Put this parameter to 'false' if automatic memory management is not desired (default: 'false')
- `oracle_install.memory_mb` - The total memory allocation for the database, value in MB (default: 1536)
- `oracle_install.db_charset` - Specify the Starter Database character set (default: 'AL32UTF8')

