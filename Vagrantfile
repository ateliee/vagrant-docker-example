
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/hirsute64"

  config.vm.hostname = "laravel-docker"
  config.vm.network "private_network", ip: "192.168.56.10"
  config.vm.synced_folder "./", "/home/vagrant/share", type:"virtualbox"

  # Docker Settings
  # config.vm.provision :docker, run: "always"
  # config.vm.provision :docker_compose, yml: "/home/vagrant/share/docker-compose.yml", run: "always"

end
