# -*- mode: ruby -*-
# vi: set ft=ruby :

begin
  require_relative "local_config"
rescue LoadError
end

# default settings
Cpus = 2 unless defined?(Cpus)
Memory = 2048 unless defined?(Memory)
Gui = false unless defined?(Gui)
VRAM = 128 unless defined?(VRAM)
Accelerate3D = true unless defined?(Accelerate3D)
SyncedFolders = [] unless defined?(SyncedFolders)

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

  SyncedFolders.each do |path|
    basename = File.basename(path)
    config.vm.synced_folder path, "/" + basename, id: basename
  end

  config.vm.network :forwarded_port,
    id: "docker",
    guest: 2376, host: 2376, host_ip: '127.0.0.1',
    auto_correct: true
  config.vm.network :forwarded_port,
    id: "registry",
    guest: 5000, host: 5000,
    auto_correct: true
  config.vm.network :forwarded_port,
    id: "portainer",
    guest: 9000, host: 9000, host_ip: '127.0.0.1',
    auto_correct: true

  config.vm.network :private_network, type: :dhcp

  config.vm.provision :docker
  config.vm.provision :ansible_local do |ansible|
    ansible.provisioning_path = "/vagrant/provision"
    ansible.playbook = "docker-host.yaml"
    ansible.compatibility_mode = "2.0"
  end
end
