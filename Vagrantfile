['vagrant-reload'].each do |plugin|
  unless Vagrant.has_plugin?(plugin)
    raise "Vagrant plugin #{plugin} is not installed!"
  end
end

Vagrant.configure('2') do |config|
  config.vm.box = "ubuntu/trusty64" # Ubuntu 14.04
  config.vm.network "private_network", ip: "192.168.50.4"

  # fix issues with slow dns https://www.virtualbox.org/ticket/13002
  config.vm.provider :libvirt do |libvirt|
    libvirt.connect_via_ssh = false
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.nic_model_type = "e1000"
  end

  config.vm.provider :virtualbox do |vb|
    vb.memory = 2048
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["setextradata", :id, "VBoxInternal/CPUM/SSE4.1", "1"]
    vb.customize ["setextradata", :id, "VBoxInternal/CPUM/SSE4.2", "1"]
    config.vm.synced_folder ".", "/vagrant", type: "rsync"
  end

  config.vm.provision :shell, :privileged => true, :path => "setup-apt.sh"
  config.vm.provision :shell, :privileged => true, :path => "setup-kernel.sh"
  config.vm.provision :reload
  config.vm.provision :shell, :privileged => true, :path => "setup-bcc.sh"
  config.vm.provision :shell, :privileged => true, :path => "setup-xdp-script.sh"
end

