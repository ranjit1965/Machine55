# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

VAGRANT_BOX         = "generic/ubuntu2204"
VAGRANT_BOX_VERSION = "4.2.10"
CPUS_MASTER_NODE    = 4
CPUS_WORKER_NODE    = 1
MEMORY_MASTER_NODE  = 4096
MEMORY_WORKER_NODE  = 2048
WORKER_NODES_COUNT  = 3


Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "kloud-bytes-bootstrap.sh"

  # Kubernetes Master Server
  config.vm.define "k8s55-master" do |node|
  
    node.vm.box               = VAGRANT_BOX
    node.vm.box_check_update  = false
    node.vm.box_version       = VAGRANT_BOX_VERSION
    node.vm.hostname          = "k8s55-master.example.com"

    node.vm.network "private_network", ip: "192.168.55.100"
  
    node.vm.provider :virtualbox do |v|
      v.name    = "k8s55-master"
      v.memory  = MEMORY_MASTER_NODE
      v.cpus    = CPUS_MASTER_NODE
    end
  
    node.vm.provider :libvirt do |v|
      v.memory  = MEMORY_MASTER_NODE
      v.nested  = true
      v.cpus    = CPUS_MASTER_NODE
    end
  
    node.vm.provision "shell", path: "bootstrap_k8s55-master.sh"
  
  end


  # Kubernetes Worker Nodes
  (1..WORKER_NODES_COUNT).each do |i|

    config.vm.define "k8s55-worker#{i}" do |node|

      node.vm.box               = VAGRANT_BOX
      node.vm.box_check_update  = false
      node.vm.box_version       = VAGRANT_BOX_VERSION
      node.vm.hostname          = "k8s55-worker#{i}.example.com"

      node.vm.network "private_network", ip: "192.168.55.10#{i}"

      node.vm.provider :virtualbox do |v|
        v.name    = "k8s55-worker#{i}"
        v.memory  = MEMORY_WORKER_NODE
        v.cpus    = CPUS_WORKER_NODE
      end

      node.vm.provider :libvirt do |v|
        v.memory  = MEMORY_WORKER_NODE
        v.nested  = true
        v.cpus    = CPUS_WORKER_NODE
      end

      node.vm.provision "shell", path: "bootstrap_k8s55-worker.sh"

    end

  end

end
