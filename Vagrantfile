# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version '>= 1.5.1'

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.network :private_network, ip: '192.168.50.5'
  config.vm.hostname = 'example.dev'

  # adjust paths relative to Vagrantfile
  config.vm.synced_folder '../example.dev', '/srv/www/example.dev/current', owner: 'vagrant', group: 'www-data', mount_options: ['dmode=776', 'fmode=775']

  config.vm.provision :ansible do |ansible|
    # adjust paths relative to Vagrantfile
    ansible.playbook = './site.yml'
    ansible.groups = {
      'wordpress-server' => ['default']
    }
    ansible.extra_vars = {
      ansible_ssh_user: 'vagrant',
      user: 'vagrant'
    }
    ansible.sudo = true
  end

  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box

    config.cache.synced_folder_opts = {
      type: :nfs,
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
  end
end
