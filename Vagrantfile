# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "envimation/ubuntu-xenial-docker"

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
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder ".", "/home/vagrant/go/src/github.com/vmware/dispatch"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  if File.file?(File.expand_path('~/.inputrc')) then
    config.vm.provision "file", source: "~/.inputrc", destination: ".inputrc"
    config.vm.provision "shell", inline: <<-SHELL
      chown vagrant .inputrc
    SHELL
  end

  golang_filename = "go1.10.3.linux-amd64.tar.gz"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y autojump git vim

    wget --no-verbose https://dl.google.com/go/#{golang_filename}
    sudo tar -C /usr/local -xzf #{golang_filename}
    sudo rm #{golang_filename}

    mkdir -p go/src/github.com/vmware/dispatch
    mkdir -p go/bin
    chown vagrant go
    chown vagrant go/bin
    # find go -type d -exec chmod 655 -- {} +

    echo "[[ -s /usr/share/autojump/autojump.sh ]] && . /usr/share/autojump/autojump.sh" >> .profile
    echo "export PS1='\\h:\\W \\u\\$ '" >> .profile
    echo "export GOPATH=~/go" >> .profile
    echo "export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin" >> .profile
    docker run -d -p 5000:5000 --restart=always --name=registry registry
  SHELL
end
