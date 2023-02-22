# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = "centos/7"
  config.vm.box = "generic/centos9s"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
   config.vm.network "public_network", bridge: "en0: Wi-Fi"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "2048"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL, privileged: false
     time sudo yum -yq update |& awk '/^real /{print $NF}; {print}'
     sudo yum -yq install wget unzip
     sudo rm -rf sonar*
     mkdir sonar 
     cd sonar 
     wget -q https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
     sudo rpm -ivh jdk-17_linux-x64_bin.rpm; rm -rf jdk-17_linux-x64_bin.rpm
     java -version
     wget -q https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip 
     unzip -q sonarqube-9.9.0.65466.zip
     sudo chown -R $(whoami):$(whoami) /home/vagrant/sonar/sonarqube-9.9.0.65466/
     /home/vagrant/sonar/sonarqube-9.9.0.65466/bin/linux-x86-64/sonar.sh start
     sudo firewall-cmd --permanent --add-port=9000/tcp
     sudo firewall-cmd --reload
     sleep 10
     /home/vagrant/sonar/sonarqube-9.9.0.65466/bin/linux-x86-64/sonar.sh status
     sleep 10
     curl -I localhost:9000 | grep 200
     echo " Your SonarQube Server is now running and can be access from the URL below"
     echo "Sonar URL is: http://$(ip a | awk '/inet /{print $2}' | tail -1 | awk -F/ '{print $1}'):9000"
   SHELL

end
