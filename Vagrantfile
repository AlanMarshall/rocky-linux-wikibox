# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "rockylinux/8"
  # config.vm.box_version = "4.0.0"
  # NOTE: The 8.5 box for VirtualBox provider is currently broken
  config.vm.box_version = "5.0.0"
  config.vm.hostname = "rockylinux"
  config.vm.network "forwarded_port", guest: 8000, host: 8000

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end

  config.vm.provision "shell", name: "update", privileged: true, inline: <<-SHELL
    dnf -y update
    dnf -y install epel-release
    dnf install -y git python3 screen vim-enhanced
  SHELL

  config.vm.provision "shell", name: "clone_wiki", privileged: false, inline: <<-SHELL
    if [[ ! -d /vagrant/wiki.rockylinux.org/docs ]]; then
      if [[ ! -d /vagrant/wiki.rockylinux.org ]]; then
        pushd /vagrant
        git clone https://github.com/AlanMarshall/wiki.rockylinux.org
        popd
      fi
    fi
  SHELL

  config.vm.provision "shell", name: "install_mkdocs", privileged: true, inline: <<-SHELL
    pip3 install mkdocs
    pushd /vagrant/wiki.rockylinux.org
    pip3 install -r requirements.txt
  SHELL

  config.vm.provision "shell", name: "mkdocs_serve", privileged: false, run: "always", inline: <<-SHELL
    echo -e "\nLogin to the docbox with the command..."
    echo -e "\n    vagrant ssh -- -L 8000:localhost:8000"
    echo -e "\nThen run mkdocs with the command sequence..."
    echo -e "\n    cd /vagrant/wiki.rockylinux.org"
    echo -e "    screen"
    echo -e "    mkdocs serve 2>&1 | tee ./mkdocs.serve.log"
    echo -e "\nThen, optionally detach from screen with the key sequence..."
    echo -e "\n    [CTRL-a] d"
    echo -e "\nFrom the host you'll be able to watch the mkdocs serve log with..."
    echo -e "\n    tail -f wiki.rockylinux.org/mkdocs.serve.log"
    echo -e "\nIf you want to trigger a rebuild of the docs inside the guest you"
    echo -e "_must_ touch a monitored file/directory inside the guest. Filesystem"
    echo -e "events are not passed from the host to the guest even though actual"
    echo -e "content changes will be seen. Do that with the following command..."
    echo -e "\n    touch /vagrant/wiki.rockylinux.org/docs/docs/index.md"
    echo -e "\nEven if that isn't the file you have modified the entire documentation"
    echo -e "tree will be rebuilt. The page you are viewing in the brower in your"
    echo -e "host will update automatically when the docs are rebuilt.\n"
  SHELL

end
