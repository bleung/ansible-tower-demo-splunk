# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |cluster|

  cluster.vm.define "splunk" do |config|
    config.vm.box = "centos/7"
    config.ssh.insert_key = false
    config.vm.provider :libvirt do |vb|
      vb.memory = 8192
      vb.cpus = 4
   end
    config.vm.hostname = "splunk"
  end

  cluster.vm.define "tower" do |config|
    config.vm.box = "centos/7"
    config.ssh.insert_key = false
    config.vm.provider :libvirt do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end
    config.vm.hostname = "tower"

    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/site.yml"
      ansible.limit = "all"
      ansible.become = true
      ansible.groups = {
        "splunkvm" => ["splunk"],
        "towervm" => ["tower"],
        "demovm" => ["demovm1", "demovm2", "demovm3", "demovm4"]
      }
      ansible.host_vars = {
        "splunk" => {"splunk_http_token" => "ansibletowertoken"}
      }
    end
  end

#  (1..1).each do |i|
#    cluster.vm.define "demovm#{i}" do |config|
#      config.vm.box = "centos/7"
#      config.ssh.insert_key = false
#      config.vm.provider :libvirt do |v|
#        v.memory = 512
#     end
#      config.vm.hostname = "demovm#{i}"
#    end
#  end

end
