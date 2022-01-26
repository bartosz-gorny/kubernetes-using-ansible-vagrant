IMAGE_NAME = ENV["IMAGE_NAME"] || "bento/ubuntu-18.04"
MASTER_MEMORY = ENV["MASTER_CPUS"] || 4096
MASTER_CPUS = ENV["MASTER_CPUS"] || 2
WORKER_NODES = ENV["WORKER_NODES"] || 1
WORKER_MEMORY = ENV["WORKER_MEMORY"] || 2048
WORKER_CPUS = ENV["WORKER_CPUS"] || 2
BASE_IP = ENV["BASE_IP"] || "192.168.50"
KUBERNETES_VERSION= ENV["KUBERNETES_VERSION"] || "1.20.6-00"

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.define "k8s-master" do |master|
      master.vm.provider "virtualbox" do |v|
        v.memory = MASTER_MEMORY
        v.cpus = MASTER_CPUS
      end
      master.vm.box = IMAGE_NAME
      master.vm.network "public_network", ip: "#{BASE_IP}.#{10}", bridge: "wlp3s0"
      master.vm.hostname = "k8s-master"
      master.vm.provision "ansible" do |ansible|
          ansible.playbook = "kubernetes-setup/master-playbook.yaml"
          ansible.verbose = true
          ansible.extra_vars = {
            node_ip: "#{BASE_IP}.#{10}",
            kubernetes_version: KUBERNETES_VERSION,
          }
      end
  end

  (1..WORKER_NODES).each do |i|
      config.vm.define "node-#{i}" do |node|
          node.vm.provider "virtualbox" do |v|
            v.memory = WORKER_MEMORY
            v.cpus = WORKER_CPUS
          end
          node.vm.box = IMAGE_NAME
          node.vm.network "public_network", ip: "#{BASE_IP}.#{i + 10}", bridge: "wlp3s0"
          node.vm.hostname = "node-#{i}"
          node.vm.provision "ansible" do |ansible|
              ansible.playbook = "kubernetes-setup/node-playbook.yaml"
              ansible.verbose = true
              ansible.extra_vars = {
                  node_ip: "#{BASE_IP}.#{i + 10}",
                  kubernetes_version: KUBERNETES_VERSION,
              }
          end
      end
  end
end