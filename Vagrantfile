# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?("vagrant-docker-compose")
  system("vagrant plugin install vagrant-docker-compose")
  puts "Dependencies installed, restarting vagrant again ..."
  exec "vagrant #{ARGV.join(' ')}"
end

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "tensorflow-mnist-tutorial" 
  config.ssh.forward_x11 = true # otherwise use, vagrant ssh -- -X

  #
  config.vm.synced_folder ".", "/vagrant", disabled: true
  #
  config.vm.synced_folder ".", "/home/vagrant/tensorflow-mnist-tutorial", disabled: false

  config.vm.network :forwarded_port, host: 8888, guest: 8888, auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.name = config.vm.hostname.to_s
    # Customize the amount of memory on the VM:
    vb.memory = 2048
    vb.cpus = 2
  end
  #

  #config.vm.provision :docker
  #config.vm.provision :docker_compose, 
  #		       project_name: "tensorflow_tutorial",
  #		       yml: "/vagrant/docker-compose.yml",
  #		       run: "always"
 
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
     sudo -H apt-get update -yq
     sudo -H apt-get install -y git python3 python3-matplotlib python3-pip python3-dev
     sudo -H apt-get install -y sudo libfreetype6-dev pkg-config
     sudo -H pip3 install --upgrade cycler
     sudo -H pip3 install --upgrade matplotlib
    
     # Ubuntu/Linux 64-bit, CPU only, Python 3.4
     export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp34-cp34m-linux_x86_64.whl
     sudo -H pip3 install --upgrade $TF_BINARY_URL
   SHELL
end
