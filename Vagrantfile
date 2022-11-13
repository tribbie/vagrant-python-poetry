# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-11.4"
  config.vm.box_check_update = false

  config.vm.hostname = "python-poetry"
  config.vm.network "forwarded_port", guest: 80, host: 8012
  config.vm.network "private_network", ip: "192.168.80.12"

  config.vm.synced_folder "data/", "/home/vagrant/data"
  config.vm.synced_folder "code/", "/home/vagrant/code"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "vagrant-python-poetry"
    vb.memory = "1024"
  end

  # bashrc with fancy prompt
  config.vm.provision "file", source: "data/dot-bashrc.fordebian", destination: ".bashrc"
  config.vm.provision "file", source: "data/dot-tmux.conf.fordebian", destination: ".tmux.conf"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.

  ## Inline shell script - privileged (run by root)
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    #apt-get upgrade -y
    echo "=== Installing python3-distutils"
    apt-get install -y python3-distutils
    echo "=== Installing some handy tools"
    apt-get install -y curl tmux tree jq
  SHELL

  ## Inline shell script - unprivileged (run by user vagrant)
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # python-poetry
    curl -sSL https://install.python-poetry.org | python3 -
    cat /vagrant/code/someinfo.txt
  SHELL

end
