# -*- mode: ruby -*-
# vi: set ft=ruby :

## if you change these, change vagrant/hosts too
hosts = [
  {
    :name => "concourse-web",
    :ip => "10.10.10.2",
    :aliases => %w(concourse.dev),
    :ram => 1024,
  },
  {
    :name => "concourse-worker",
    :ip => "10.10.10.3",
    :ram => 1024
  },
]

Vagrant.configure("2") do |config|

  # setup /etc/hosts on each vm
  # - requires the vagrant-hostmanager plugin
  config.hostmanager.enabled = true
  # set to 'false' if you don't wan't to manage _your_ /etc/hosts
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  hosts.each do |host|
    config.vm.define host[:name] do |c|

      c.vm.box = "boxcutter/centos73"

      # stop Vagrant 'helping'
      c.ssh.insert_key = false

      c.vm.hostname = host[:name]
      if host[:aliases]
          c.hostmanager.aliases = host[:aliases]
      end

      c.vm.network :private_network, ip: host[:ip], netmask: "255.255.255.0"

      c.vm.provider("virtualbox") do |vb|
        vb.memory = host[:ram]
      end

      # turn off default shared folder
      c.vm.synced_folder ".", "/vagrant", :disabled => true

      if host[:mount]
        host[:mount].each do |m|
          c.vm.synced_folder m[:local], m[:remote]
        end
      end

    end
  end
end
