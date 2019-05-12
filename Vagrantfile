# -*- mode: ruby -*-
# vi: set ft=ruby :

def os_cpu_cores
  case RbConfig::CONFIG['host_os']
  when /darwin/
    Integer(`sysctl -n hw.ncpu`)
  when /linux/
    Integer(`cat /proc/cpuinfo | grep processor | wc -l`)
  else
    raise StandardError, "Unsupported platform"
  end
end

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = os_cpu_cores
  end

  config.vm.network :private_network, ip: "10.10.10.20"

  config.vm.provision "shell", inline: "sudo apt install python -y"
  
  config.vm.provision :ansible do |ansible|
    ansible.inventory_path = "inventory.ini"
    ansible.playbook = "main.yml"
    ansible.extra_vars = { user: "vagrant" }
    ansible.limit = 'all'
  end
end
