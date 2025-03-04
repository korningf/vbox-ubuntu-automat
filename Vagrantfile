# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

# The most common configuration options are documented and commented below.
# For a complete reference, please see the online documentation at
# https://docs.vagrantup.com.
   
# Every Vagrant development environment requires a box. You can search for
# boxes at https://vagrantcloud.com/search.
    
Vagrant.configure("2") do |config|

  #======================================================================================#
  # Guest: Force a specific version for VirtualBox Guest Additions
  #--------------------------------------------------------------------------------------#
  # A version mismatch bug may occur with the Guest Additions (after multiple reinstalls)
  # Even the Vagrant-vbguest plugin may fail to fix it. Best to override the .iso image.
  #--------------------------------------------------------------------------------------#
  # config.vbguest.iso_path = "https://download.virtualbox.org/virtualbox/%{version}/VBoxGuestAdditions_%{version}.iso"
  #======================================================================================#
  config.vbguest.iso_path = "https://download.virtualbox.org/virtualbox/%{version}/VBoxGuestAdditions_%{version}.iso"



  #======================================================================================#
  # Image: select virtual machine image (may be architecture specific)
  #--------------------------------------------------------------------------------------#
  # Mac OSX (Intel):
  # config.vm.box = "ubuntu/jammy64"
  # 
  # Intel x86-64:
  # config.vm.box = "ubuntu/jammy64"
  #======================================================================================#
  config.vm.box = "ubuntu/jammy64"


  #======================================================================================#
  # Timeout: time to initialize machine (in secs, default = 300 = 5 mins)
  #--------------------------------------------------------------------------------------#
  # config.vm.boot_timeout = 300
  #======================================================================================#
  config.vm.boot_timeout = 600
  

  #======================================================================================#
  # Machine: Customizations for machine (memory, etc)
  #--------------------------------------------------------------------------------------#
  # vb.memory = "4096"
  #======================================================================================#
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  #
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "2048"
  # end
  #

  config.vm.provider "virtualbox" do |vb|
     
     # Custom VM name "vbox-ubuntu-automat"
     vb.name = "vbox-ubuntu-automat"
     
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true
  
     # Customize the amount of memory on the VM:
     vb.memory = "4096" 
  end

  

  #======================================================================================#
  # Sharing: vb guest additions are needed for shared folders (synched folders)
  #--------------------------------------------------------------------------------------#
  # config.vbguest.auto_update = true
  #======================================================================================#
  config.vbguest.auto_update = true
  
    
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false



  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  #======================================================================================#
  # NAT forward port mappings 
  #--------------------------------------------------------------------------------------#
  # config.vm.network "forwarded_port", guest: 22, host: 2222
  #======================================================================================#
  #config.vm.network "forwarded_port", guest: 3000, host: 3333


  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"


  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  
  
  

  #======================================================================================#
  # DiskSize: root filesystem disk size (requires vagrant-disksize plugin)
  #--------------------------------------------------------------------------------------#
  # config.disksize.size = "20GB"
  #======================================================================================#
  config.disksize.size = "20GB"


  
  #======================================================================================#
  # Boostrap: configure disks, mountpoints
  #--------------------------------------------------------------------------------------#
  # ResizeFS: resize root filesystem partition
  #--------------------------------------------------------------------------------------#
  # resize2fs /dev/sda1
  #--------------------------------------------------------------------------------------#
  #  
  #--------------------------------------------------------------------------------------#
  # Directories: configure directories for mounting shared folders 
  #--------------------------------------------------------------------------------------#
  # config.vm.provision "shell", "mkdir -p /work/automat"
  #======================================================================================#
  config.vm.provision "shell", inline: <<-SHELL


    #========================================================================#
    # Machine
    #========================================================================#

    echo ""  
    echo "resizing root filesystem"
    resize2fs /dev/sda1


    echo ""  
    echo "creating shared folders"
    mkdir -p /vagrant
    mkdir -p /mnt/work
    


    #------------------------------------------------------------------------#
    # DNS
    #------------------------------------------------------------------------#
    
    # sometimes on vbox windows, the local DNS fails to resolve apt repos
    # the workaround is to ensure we always also check google nameservers
    
    echo ""  
    echo "ensuring DNS nameservers"
    echo "nameserver 8.8.8.8" >> /etc/resolv.conf
    echo "nameserver 8.8.4.4" >> /etc/resolv.conf    



    #------------------------------------------------------------------------#
    # APT keyrings
    #------------------------------------------------------------------------#

    # ensure we have a directory for external apt repsitory keyrings    
    # /usr/share/keyrings/ for official ubuntu distro apt packages
    # /etc/apt/keyrings/ for manually installed external packages
    
    echo ""
    echo "ensuring keyring folders"
    mkdir -p /usr/share/keyrings/
    mkdir -p /etc/apt/keyrings/
    

    #------------------------------------------------------------------------#
    # APT repos
    #------------------------------------------------------------------------#
    
    echo ""  
    echo "registering package repositories"
    echo "deb http://uk.archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse"               > /etc/apt/sources.list
    echo "deb-src http://uk.archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse"           >> /etc/apt/sources.list
    echo "deb http://uk.archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse"      >> /etc/apt/sources.list
    echo "deb-src http://uk.archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse"  >> /etc/apt/sources.list
    echo "deb http://uk.archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse"       >> /etc/apt/sources.list
    echo "deb-src http://uk.archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse"   >> /etc/apt/sources.list
    apt-get update


    #------------------------------------------------------------------------#
    # APT base
    #------------------------------------------------------------------------#

    echo ""  
    echo "adding apt repository tools"
    apt-get -y install apt-transport-https ca-certificates
    apt-get -y install curl gnupg lsb-release
    apt-get -y install software-properties-common python-software-properties
    #apt-get -y upgrade
    apt-get update


    #------------------------------------------------------------------------#
    # base box
    #------------------------------------------------------------------------#

    # at minimum we will need build-essentials as we want to cross-compile.
    
    echo ""
    echo "installing build essentials and headers"
    sudo apt -y install build-essential linux-headers-$(uname -r)
    #apt-get -y install dkms
    apt-get update


    #------------------------------------------------------------------------#
    # VBox guest
    #------------------------------------------------------------------------#

    # ?
    # Check This - not sure it's required since we're forcing the version
    # ?
    
    echo ""
    echo "installing virtualbox extensions"
    #apt-get -y install virtualbox-guest
    #apt-get -y install virtualbox-guest-dkms
    apt-get -y install virtualbox-guest-utils    
    apt-get -y install virtualbox-guest-additions-iso
       
  SHELL
 
 
 
  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder ".", "/mnt/vagrant"

    
  #======================================================================================#
  # Folders: configure mountpoints for synched folders 
  #--------------------------------------------------------------------------------------#
  # config.vm.synced_folder ".", "/mnt/vagrant"
  #======================================================================================#
  #config.vm.synced_folder ".", "/vagrant"
  config.vm.synced_folder "../../", "/mnt/work"


  
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.  
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y nginx
  # SHELL

  #
  # @see https://help.ubuntu.com/community/VirtualBox
  # @see https://askubuntu.com/questions/142549/how-to-install-ubuntu-on-virtualbox
  #
  # @see https://wiki.ubuntulinux.org/wiki/Docker

  config.vm.provision "shell", inline: <<-SHELL
    
    
    #========================================================================#
    # System
    #========================================================================#
  
    echo ""
    echo "configuring basic system"

    # ?
    # Check this - this was to copy stuff from the host into the box
    # ?

    echo ""  
    echo "creating source directories"
    mkdir -p /mnt/work/faktory  
    
    # motd
    cp /vagrant/motd                /etc

    
    # todo certificates and credentials
    # warning: do not overwrite the vagrant credentials (~/.ssh/authorized_keys)
    # if using an ssh-agent, ssh-keys should already be preloaded (ssh-add -L) 
            

    #------------------------------------------------------------------------#
    # Sysutils
    #------------------------------------------------------------------------#

    #apt-get update

    echo ""
    echo "updating core linux utils"
    apt-get -y install syslinux
    apt-get -y install coreutils
    apt-get -y install util-linux
    apt-get -y install locate
    apt-get -y install tree
    #apt-get -y install procps
    #apt-get -y install iotop
        
    echo ""
    echo "updating ubuntu linux utils"
    apt-get -y install systemd.d
    #apt-get -y install init.d
    apt-get -y install rc

    
    #------------------------------------------------------------------------#
    # Archive
    #------------------------------------------------------------------------#

    echo ""
    echo "installing file archive utils"
    #apt-get -y install tar
    apt-get -y install zip
    apt-get -y install unzip
    #apt-get -y install gzip
    #apt-get -y install bzip2
    apt-get -y install sharutils


    #------------------------------------------------------------------------#
    # Network
    #------------------------------------------------------------------------#

    echo ""
    echo "installing basic network utils"
    #apt-get -y install ifupdown
    apt-get -y install net-tools
    apt-get -y install dnsutils    
    apt-get -y install wget
    #apt-get -y install curl


    
    #------------------------------------------------------------------------#
    # Security
    #------------------------------------------------------------------------#

    echo ""
    echo "installing secure network utils"
    apt-get -y install ssh
    apt-get -y install openssl
    apt-get -y install gnutls-bin
    apt-get -y install libssl-dev
    apt-get -y install libtls-dev
    apt-get -y install stunnel

    # gpg is v1. gnupg is v2
    #apt-get -y install gpg
    #apt-get -y install gnupg    


    # password-store vault
    apt-get -y install pass
    apt-get -y install sshpass


    # ?
    # Check this - common libraries for FIPS security compliance
    # ?

    echo ""
    echo "installing common compression libraries"
    #apt-get -y install zlib zlib-dev
    #apt-get -y install zlib1g zlib1g-dev


    echo ""
    echo "installing common crypto libraries"
    apt-get -y install libgcrypt20-dev

     
    #------------------------------------------------------------------------#
    # Utilities
    #------------------------------------------------------------------------#

    echo ""
    echo "installing console tools"
    apt-get -y install libncurses-dev
    apt-get -y install screen
    apt-get -y install vim
    #apt-get -y install nano

        


    #------------------------------------------------------------------------#
    # User profiles
    #------------------------------------------------------------------------#

    # TODO .bash .bashrc .profile, color xterm, ssh, ssh-agent, etc



    echo ""
    echo "Done."
    #reboot

  SHELL


end
