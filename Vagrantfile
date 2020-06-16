IMAGE_NAME = "ubuntu/bionic64"
IP_MASTER = "192.168.50.10"
IP_SLAVES = [ "192.168.50.11"]
N = 1

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
  end
    
  config.vm.define "hadoop-master" do |master|
      master.vm.box = IMAGE_NAME
      master.vm.network "private_network", ip: IP_MASTER
      master.vm.hostname = "hadoop-master"
      master.vm.provision "ansible" do |ansible|
        ansible.playbook = "./ansible/playbook.yml"
        ansible.extra_vars = {
          ip_master: IP_MASTER,
          ip_slaves: IP_SLAVES
        }
      end
    end
    
    (1..N).each do |i|
      config.vm.define "node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.network "private_network", ip: IP_SLAVES[i-1]
      node.vm.hostname = "node-#{i}"
      node.vm.provision "ansible" do |ansible|
        ansible.playbook ="./ansible/playbook.yml"
        ansible.extra_vars = {
            ip_master: IP_MASTER,
            ip_slaves: IP_SLAVES
          }
      end
    end
  end
end