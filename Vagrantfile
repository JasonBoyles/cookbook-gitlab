# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.hostname = "cookbook-gitlab"
  case ENV['VMBOX']
  when 'centos64'
    config.vm.box = "CentOS-6.4-x86_64-minimal"
    config.vm.box_url = "http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130427.box"
  else
    config.vm.box = "opscode-ubuntu-12.04"
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  end

  config.vm.network :private_network, ip: "33.33.33.20"
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "512"]
  end
  config.berkshelf.berksfile_path = "./Berksfile"
  config.berkshelf.enabled = true
  # config.omnibus.chef_version = "11.6.0"
  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.json = {
      :mysql => {
        :server_root_password => 'rootpass1',
        :server_debian_password => 'debpass1',
        :server_repl_password => 'replpass1'
      },
    }

    chef.run_list = [
        "recipe[mysql::server]",
        "recipe[gitlab::mysql]",
        "recipe[gitlab::default]"
    ]
  end
end
