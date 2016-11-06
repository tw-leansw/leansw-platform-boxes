# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
CLOUD_CONFIG_PATH = File.join(File.dirname(__FILE__), "nocloud.iso")


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
###############################################################################
# Global plugin settings                                                      #
###############################################################################
  #if Vagrant.has_plugin?("vagrant-cachier")
  #  config.cache.scope = :box
  #  config.cache.auto_detect = true
  #  config.cache.synced_folder_opts = {
  #    type: :nfs,
  #    mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
  #  }
  #end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_remote = true
  end
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = false
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
  end

  # box
  config.vm.box              = "file://builds/virtualbox-ubuntu1404.box"
  # ssh
  config.ssh.username         = 'scaleworks'
  config.ssh.insert_key       = false
  config.ssh.forward_agent    = true
  config.ssh.private_key_path = ["#{ENV['HOME']}/.ssh/id_rsa", "#{ENV['HOME']}/.vagrant.d/insecure_private_key"]
  ## synced folders
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :virtualbox do |v|
    v.gui = false
    v.cpus = 1
    v.memory = 1024
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize [
            "storageattach", :id,
            "--storagectl", "IDE Controller",
            "--port", "0",
            "--device", "1",
            "--type", "dvddrive",
            "--medium", CLOUD_CONFIG_PATH
        ]
  end

  config.vm.network :private_network, ip: "10.245.7.2"
end
