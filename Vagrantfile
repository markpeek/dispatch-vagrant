# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # config.vm.box = "envimation/ubuntu-xenial-docker"
  config.vm.box = "bento/ubuntu-18.04"
  # config.vm.box_version = "201807.12.0"

  config.vm.synced_folder ".", "/home/vagrant/go/src/github.com/vmware/dispatch"

  if File.file?(File.expand_path('~/.inputrc')) then
    config.vm.provision "file", source: "~/.inputrc", destination: ".inputrc"
    config.vm.provision "shell", inline: <<-SHELL
      chown vagrant .inputrc
    SHELL
  end

  golang_filename = "go1.10.3.linux-amd64.tar.gz"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y autojump docker.io git vim

    systemctl start docker
    systemctl enable docker
    usermod -aG docker vagrant

    echo "Install golang"
    wget --no-verbose https://dl.google.com/go/#{golang_filename}
    sudo tar -C /usr/local -xzf #{golang_filename}
    sudo rm #{golang_filename}

    mkdir -p go/src/github.com/vmware/dispatch
    mkdir -p go/bin
    chown vagrant go
    chown vagrant go/bin

    echo "Other fixups"
    echo "[[ -s /usr/share/autojump/autojump.sh ]] && . /usr/share/autojump/autojump.sh" >> .profile
    echo "export PS1='\\h:\\W \\u\\$ '" >> .profile
    echo "export GOPATH=~/go" >> .profile
    echo "export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin" >> .profile

    echo "Install local registry"
    docker run -d -p 5000:5000 --restart=always --name=registry registry

    echo "Finished provisioning"
  SHELL
end
