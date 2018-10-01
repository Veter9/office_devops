Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty32"
  config.vm.hostname = "NMS"
  config.vm.box_check_update = false
  config.vm.boot_timeout = 30
  config.vm.network "public_network", ip: "192.168.1.150", bridge: "wlp3s0"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "NMS_"
    vb.cpus = 1
    vb.memory = "1024"
end
end

