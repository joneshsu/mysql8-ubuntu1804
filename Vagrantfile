Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  config.vm.network "private_network", ip: "192.168.168.168"

  config.vm.provider "virtualbox" do |vb|
    vb.name = 'mysql8'
    vb.memory = "2048"
  end
end
