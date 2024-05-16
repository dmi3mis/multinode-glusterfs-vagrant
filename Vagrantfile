Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  glusterfs_version = "7"

  config.vm.provider "virtualbox" do |vb|
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end
  # We setup three nodes to be gluster hosts, and one gluster client to mount the volume
  (1..3).each do |i|
    config.vm.define vm_name = "gluster-server-#{i}" do |config|
      config.vm.hostname = vm_name
      ip = "192.168.56.#{i+10}"
      config.vm.network :private_network, ip: ip
      config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -yq software-properties-common", :privileged => true
      config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:gluster/glusterfs-#{glusterfs_version}", :privileged => true
      config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -yq glusterfs-server", :privileged => true
      config.vm.synced_folder '.', '/vagrant', disabled: true
    end
  end
  config.vm.define vm_name = "gluster-client" do |config|
    config.vm.hostname = vm_name
    ip = "192.168.56.10"
    config.vm.network :private_network, ip: ip
    config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -yq software-properties-common", :privileged => true
    config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:gluster/glusterfs-#{glusterfs_version}", :privileged => true
    config.vm.provision :shell, :inline => "DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -yq glusterfs-client", :privileged => true
    config.vm.synced_folder '.', '/vagrant', disabled: true
  end
end
