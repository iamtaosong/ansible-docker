Vagrant.configure(2) do |config|
    config.vm.box = "centos/7"
    config.ssh.insert_key = false
    instances_names = ["manager"]
    config.vm.boot_timeout = 600

    instances_names.each do |name|
        config.vm.define name do |config|
            config.vm.network "private_network", type: "dhcp"
        end
    end

    groups = {
        "group-managers" => [
            "manager0",
            "manager1"
        ],
        "group-consuls" => ["consul0"],
        "group-nodes" => [
            "node0",
            "node1",
        ]
    }

    config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        # ansible.groups = groups
        ansible.sudo = true
        ansible.playbook = "ansible/playbook.yml"
    end

    config.vm.provision "docker" do |d|
        d.run "rancher/server:latest",
              args: "--name=rancher-server -l io.rancher.container.system=rancher-agent --restart=always -d -p 8080:8080"
    end
end