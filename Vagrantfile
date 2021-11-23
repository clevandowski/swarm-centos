Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  N = 4
  (0..N-1).each do |node_id|
    config.vm.define "node-#{node_id}" do |node|
      node.vm.hostname = "node-#{node_id}"
      node.vm.network "private_network", ip: "192.168.77.#{20+node_id}"
      node.vm.network "forwarded_port", guest: 8000, host: "#{9000+node_id}"
      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if node_id == N-1
        node.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          # If not disabled, only machine4 will be provisionned
          ansible.limit = "all"
          ansible.playbook = "playbook-only-docker.yml"
          ansible.groups = {
            "manager" => ["node-0", "node-1", "node-2"],
            # TODO Remplacer ça par qque chose de dynamique
            "worker" => ["node-3"]
          }
        end
      end
    end
  end
end
