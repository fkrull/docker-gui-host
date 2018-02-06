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
  config.vm.network :forwarded_port, guest: 5000, host: 5000
  config.vm.network :forwarded_port, guest: 9000, host: 9000, host_ip: '127.0.0.1'

  config.vm.provision :ansible_local do |ansible|
    ansible.provisioning_path = "/vagrant/provision"
    ansible.playbook = "docker-host.yaml"
    ansible.galaxy_role_file = "roles.yaml"
    ansible.compatibility_mode = "2.0"
  end
end
