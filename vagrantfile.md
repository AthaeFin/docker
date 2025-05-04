    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    # Tero Karvisen koodinpätkästä frankensteinin koodina väännetty vagrant-config-file
    
    $minion = <<MINION
    sudo mkdir -p /etc/apt/keyrings
    sudo apt-get update
    sudo apt-get install -qy curl
    sudo curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
    sudo curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
    sudo apt-get update
    sudo apt-get -qy install salt-minion
    echo "master: 10.20.30.100">/etc/salt/minion
    sudo systemctl enable salt-minion
    sudo systemctl restart salt-minion
    sudo apt-get update
    MINION
    
    $master = <<MASTER
    sudo mkdir -p /etc/apt/keyrings
    sudo apt-get update
    sudo apt-get install -qy curl
    sudo curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
    sudo curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
    sudo apt-get update
    sudo apt-get -qy install salt-master
    sudo systemctl enable salt-master
    sudo systemctl start salt-master
    MASTER
    
    Vagrant.configure("2") do |config|
    
      config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 4
      end
    
      config.vm.box = "debian/bookworm64"
    
      config.vm.define "master", primary: true do |master|
        master.vm.provision :shell, inline: $master
        master.vm.network "private_network", ip: "10.20.30.100"
        master.vm.hostname = "master"
      end
    
      config.vm.define "minion" do |minion|
        minion.vm.provision :shell, inline: $minion
        minion.vm.network "private_network", ip: "10.20.30.101"
        minion.vm.hostname = "minion"
      end
    end
