# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box_check_update = false
  config.vm.synced_folder '.', '/vagrant' , disabled: true

  config.hostmanager.enabled = true
  config.hostmanager.ignore_private_ip = false

  config.vm.define 'oracle-db-vm' do |cfg|
    cfg.vm.box = 'abessifi/centos-7.1-ansible'
    #cfg.vm.box = 'abessifi/debian-jessie-ansible'
    #cfg.vm.box = 'abessifi/ubuntu-trusty-ansible'
    #cfg.vm.box  = 'abessifi/opensuse-13.2-ansible'
    cfg.vm.hostname = 'oracle-db-vm'
    cfg.vm.network 'private_network', ip: '192.168.33.102'
    cfg.vm.provider 'virtualbox' do |vb|
      vb.name = 'oracle-db-vm'
      vb.memory = 2024
      vb.cpus   = 2
    end
    cfg.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'provision.yml'
      ansible.inventory_path = 'vagrant-inventory.ini'
      ansible.limit = 'oracle-db'
      ansible.raw_arguments = [ "--skip-tags=remove-installer,start-pluggable-db,reboot-vm" ]
      ansible.verbose = 'v'
    end
  end

end
