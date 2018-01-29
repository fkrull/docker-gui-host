# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-9"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
    vb.gui = true
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--vram", 128]
    vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
  end

  config.vm.network :forwarded_port, guest: 2376, host: 2376, host_ip: '127.0.0.1'
  config.vm.network :forwarded_port, guest: 9000, host: 9000, host_ip: '127.0.0.1'

  config.vm.provision :shell, inline: <<-EOF
    apt-get update
    apt-get install -y python-pip vim
    pip install docker-compose

    if ! which docker; then
      curl -fsSL get.docker.com | bash -
    fi
    usermod -aG docker vagrant

    mkdir -p /etc/systemd/system/docker.service.d
    cp /vagrant/docker.conf /etc/systemd/system/docker.service.d/
    cp /vagrant/daemon.json /etc/docker/
    cp /vagrant/portainer.service /etc/systemd/system/
    cp /vagrant/qemu-user-static.service /etc/systemd/system/

    systemctl daemon-reload
    systemctl restart docker.service
    systemctl enable --now portainer.service
    systemctl enable --now qemu-user-static.service
  EOF
end
