# -*- mode: ruby -*-
# vi: set ft=ruby :

begin
  require_relative "local_config"
rescue LoadError
end

# default settings
Cpus = 2 unless defined?(Cpus)
Memory = 2048 unless defined?(Memory)
Gui = true unless defined?(Gui)
VRAM = 128 unless defined?(VRAM)
Accelerate3D = true unless defined?(Accelerate3D)

Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-9"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = Memory
    vb.cpus = Cpus
    vb.gui = Gui
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--vram", VRAM]
    vb.customize ["modifyvm", :id, "--accelerate3d", Accelerate3D ? "on" : "off"]
  end

  config.vm.network :forwarded_port, guest: 2376, host: 2376, host_ip: '127.0.0.1'
  config.vm.network :forwarded_port, guest: 5000, host: 5000, host_ip: '127.0.0.1'
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
    cp /vagrant/portainer-endpoint.json /etc/
    cp /vagrant/portainer.service /etc/systemd/system/
    cp /vagrant/registry.service /etc/systemd/system/
    cp /vagrant/qemu-user-static.service /etc/systemd/system/

    systemctl daemon-reload
    systemctl restart docker.service
    systemctl enable --now portainer.service
    systemctl enable --now registry.service
    systemctl enable --now qemu-user-static.service
  EOF
end
