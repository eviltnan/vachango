# -*- mode: ruby -*-
# vi: set ft=ruby :



Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.omnibus.chef_version = "11.12.4"
  config.berkshelf.enabled = true

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network :forwarded_port, guest: 8000, host: 8000
  config.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/","{{project_name}}/media/*", "*coffee/*.js", "*sass/*.css","./idea/*"]

  #config.vm.synced_folder ".", "/vagrant", :nfs => true
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #config.vm.network :private_network, ip: "10.11.12.13"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  # config.vm.provider :virtualbox do |vb|

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:this
  #chef-solo -c solo.rb -j web.json
  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless modeextension
    vb.gui = true
    #vb.cpus = 3
    #Use VBoxManage to customize the VM. For example to change memory:
    #vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end
  #/home/sergey/PycharmProjects/mb_amg_f1/src/mbamgf1/core
  # View the documentation for the provider you're using for mothisre
  # information on available options.

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file precise64.pp in the manifests_path directory.
  #
  # An example Puppet manifest to provision the message of the day:
  #
  # # group { "puppet":
  # #   ensure => "present",
  # # }
  # #
  # # File { owner => 0, group => 0, mode => 0644 }
  # #
  # # file { '/etc/motd':this
  # #   content => "Welcome to your Vagrant-built virtual machine!
  # #               Managed by Puppet.\n"
  # # }
  #
  # config.vm.provision :puppet do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "init.pp"
  # end

  # Enable provisioning with chef solo, specifying a cochecf gettiokbooks path, roles
  # path, and data_bags path (all relativehttp://intro.hellofutu.re/ to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "./chef/cookbooks"
    chef.add_recipe "apt"
    chef.add_recipe "mysql::server"
    chef.add_recipe "database::mysql"
    chef.add_recipe "mysql-databases"
    chef.add_recipe "git"
    chef.add_recipe "python"
    chef.add_recipe "supervisor::vagrant"
    #	   # You may also specify custom JSON attributes:
    chef.json = {
      "mysql" => {
                  "server_root_password" => "password",
                  "server_repl_password" => "password",
                  "server_debian_password" => "password"
                 },
      "databases" => {
                      "create" => ["django"],
                      "grant" => [
                                  {
                                   "user" => "django",
                                   "password" => "password",
                                   "host" => "localhost"
                                  }
                                 ]
                     }
    }

  end
  config.vm.provision :shell, :inline => "sudo initctl emit vagrant-mounted"


  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
  config.vm.provision :shell, :inline => "sudo initctl emit vagrant-mounted"

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "{{project_name}}::vagrant"
  end
end
