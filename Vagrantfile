# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  PYTHON_VERSION = "2.7.11"
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = 'ubuntu-python'
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"
  config.vm.synced_folder ".", "/home/vagrant/data"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end

  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    function install {
      echo installing $1
      shift
      apt-get -y install "$@" >/dev/null 2>&1
    }
    echo updating package information

    apt-get -y update >/dev/null 2>&1

    install 'development tools' build-essential

    install 'required for compile python' git gcc make openssl libssl-dev libbz2-dev libreadline-dev libsqlite3-dev

    # TimeZone

    echo "Asia/Tokyo" | tee /etc/timezone
    dpkg-reconfigure --frontend noninteractive tzdata

    # Needed for docs generation.
    update-locale LANG=en_US.UTF-8 LANGUAGE=en_US.UTF-8 LC_ALL=en_US.UTF-8
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL

    echo installing pyenv

    git clone https://github.com/yyuu/pyenv.git ~/.pyenv
    git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
    echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
    source ~/.bash_profile

    pyenv install #{PYTHON_VERSION}
    pyenv global #{PYTHON_VERSION}
  SHELL
end
