
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/hirsute64"

  config.vm.hostname = "laravel-docker"
  config.vm.network "private_network", ip: "192.168.56.10"
  config.vm.synced_folder "./", "/home/vagrant/share", type:"virtualbox"

  # Docker Settings
  config.vm.provision :docker
  config.vm.provision :docker_compose, compose_version: "1.24.1"

end
