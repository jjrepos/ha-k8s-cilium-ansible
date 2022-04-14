IMAGE_NAME = "ubuntu/focal64"

IP_BASE="192.168.56."

LB_IP = "192.168.56.9"
LB_PORT = "6443"
PRIMARY_MASTER_IP = "192.168.56.10"
SECONDARY_MASTER_IP = "192.168.56.11"
K8_VERSION="1.23.3"

MASTERS = 2
NODES = 1



#Loadbalacer 
# must be provisioned and running prior to k8 masters can be provisioned
Vagrant.configure("2") do |config|
  config.vm.box = IMAGE_NAME
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "1"
  end
  config.vm.define "loadbalancer" do |loadbalancer|
    loadbalancer.vm.hostname = "loadbalancer"
    loadbalancer.vm.network "private_network", ip: LB_IP
  end
end

#K8s
Vagrant.configure("2") do |config|
  config.vm.box = IMAGE_NAME
  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end
  #Primary Master
  config.vm.define "k8-master-01" do |primary|
    primary.vm.hostname = "k8-master-01"
    primary.vm.network "private_network", ip: PRIMARY_MASTER_IP
  end
  #Secondary Masters
  config.vm.define "k8-master-02" do |master|
    master.vm.hostname = "k8-master-02"
    master.vm.network "private_network", ip: "#{SECONDARY_MASTER_IP}"
  end
  (1..NODES).each do |i|
    config.vm.define "k8-node-#{i}" do |node|
      node.vm.hostname = "k8-node-#{i}"
      node.vm.network "private_network", ip: "#{IP_BASE}#{i + 10 + MASTERS}"
    end
  end
end

