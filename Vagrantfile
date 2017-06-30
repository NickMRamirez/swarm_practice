# -*- mode: ruby -*-
# vi: set ft=ruby :

# 1. Run on host1:
# sudo docker swarm init --advertise-addr 192.168.56.2
# sudo docker swarm join-token manager

# 2. Join host2 and host3 to swarm

# 3. Deploy stack:
# sudo docker stack deploy --compose-file /vagrant/nginx/docker-compose.yml web

Vagrant.configure("2") do |config|

  number_of_hosts = 3

  install_docker = %Q(
    if [ ! -f /usr/bin/docker ]; then
      echo "Installing docker..."
      sudo apt update
      sudo apt install -y apt-transport-https ca-certificates curl software-properties-common rsyslog
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      sudo apt-key fingerprint 0EBFCD88
      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
      sudo apt update
      sudo apt install -y docker-ce
    fi
    
    if [ ! -f /usr/bin/docker-compose ]; then
      echo "Installing docker-compose..."
      curl -L https://github.com/docker/compose/releases/download/1.13.0/docker-compose-`uname -s`-`uname -m` > /usr/bin/docker-compose
      chmod +x /usr/bin/docker-compose
    fi
  )

  (1..number_of_hosts).each do |num|
    config.vm.define "host#{num}" do |host|
      host.vm.box = "envimation/ubuntu-xenial"
      host.vm.hostname = "host#{num}"
      host.vm.network 'private_network', ip: "192.168.56.#{num + 1}" unless (num + 1) >= 255
      host.vm.provision 'shell', inline: install_docker
    end
  end
end
