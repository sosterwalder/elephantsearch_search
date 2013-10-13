# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$script = <<'SCRIPT'
  checkForPackage()
  {
    PKG_OK=$(dpkg-query -W --showformat='${Status}\n' $1|grep "install ok installed")
    echo Checking for $1: $PKG_OK
    if [ "" == "$PKG_OK" ]; then
      echo "Package $1 not installed. Setting up $1."
      sudo apt-get --force-yes --yes install $1
    fi
  }


  echo I am provisioning
  sudo apt-get update
  checkForPackage 'maven'
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # config.vm.network :forwarded_port, guest: 80, host: 8080
  # Stanbol
  #config.vm.network :forwarded_port, guest: 8080, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8000

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM.
  # config.vm.synced_folder "../data", "/vagrant_data"
  #

  ## Provisioning
  
  # Shell
  config.vm.provision "shell", inline: $script  
  
  # Chef
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "chef/cookbooks"
    chef.add_recipe "ohai"
    chef.add_recipe "subversion"
    chef.add_recipe "ark"
    chef.add_recipe "java"
  end
  
  # Puppet
  # config.vm.provision :puppet do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file = "base.pp"
  #   puppet.module_path = "modules"
  # end 

  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true
  
    # Use VBoxManage to customize the VM.
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end
end
