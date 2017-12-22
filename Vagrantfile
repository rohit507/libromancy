# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  ################
  #     Boxes    #
  ################
  
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  
  # If we're using LXC as a provider change the underlying box, to one that 
  # provides xenial for LXC.
  config.vm.provider "lxc" do |v, override|
    override.vm.box = "developerinlondon/ubuntu_lxc_xenial_x64"
  end
  config.vm.provider :lxd do |v, override|
    override.vm.box = "ghopp/sid"
    v.privileged
  end

  ################
  # Provisioning #
  ################

  # Each of the following provisioning actions are run *in order* by vagrant 
  # as a new box is created. 

  # Run a standard system update and install various standard pieces of
  # software.
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update -y
    sudo apt-get upgrade -y
    sudo apt-get install wget curl fish htop git subversion -y
    sudo apt-get install software-properties-common tree -y 
    sudo apt-get install pkg-config time -y 
    sudo apt-get install imagemagick inkscape -y 
    sudo apt-get install libfftw3-3 libfftw3-bin libfftw3-dev -y 
  SHELL

  # Install Stack, Cabal, and Haskell
  config.vm.provision "shell", inline: <<-SHELL
    curl -sSL https://get.haskellstack.org | sh
  SHELL
   # sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 575159689BEFB442
   # echo 'deb http://download.fpcomplete.com/ubuntu xenial main'|sudo tee /etc/apt/sources.list.d/fpco.list
   # sudo apt-get update && sudo apt-get install stack -y

  # Temporary directories for the install process
  config.vm.provision "shell", inline: <<-SHELL
    sudo mkdir /install
    sudo mkdir /install/texlive
  SHELL

  # Install Texlive 
  config.vm.provision "shell", inline: <<-SHELL
    sudo rm -rf /usr/local/texlive/2017
    sudo rm -rf ~/.texlive2017
    
    cd /install/texlive
    sudo wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
    sudo tar -xvf install-tl-unx.tar.gz 
    cd install-tl-* 
    sudo ./install-tl --profile=/vagrant/bld/vagrant/texlive.profile
  SHELL

  # Add environment setup to bashrc
  config.vm.provision "shell", inline: <<-SHELL
    echo "source /vagrant/bld/vagrant/exports.sh" >> /home/vagrant/.bashrc
  SHELL

#   # Stack initialization process
#   config.vm.provision "shell", inline: <<-SHELL
#       cd /vagrant/
#       chmod +x build.sh 
#       echo "    ## Stack Initialization ##"
#       sudo -u vagrant -i cd /vagrant/ && stack setup
#       sudo -u vagrant -i cd /vagrant/ && stack build
#       sudo -u vagrant -i cd /vagrant/ && ./build.sh test
#   SHELL

  ################
  #  Networking  #
  ################

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  ################
  #  Filesystem  #
  ################

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"


end
