# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-6.7"
  config.vm.network "forwarded_port", guest: 8080, host:8080, id:"http"
  config.vm.network "forwarded_port", guest: 8000, host:8000, id:"http"
  config.vm.synced_folder "./", "/vagrant", owner: 'vagrant', group: 'vagrant', mount_options: ['dmode=777', 'fmode=666']
  config.ssh.insert_key = false

  if Vagrant.has_plugin?("vagrant-vbguest") then
    # Guest Additions自動更新の無効化設定
    config.vbguest.auto_update = false
  end

  config.vm.define "gaephp.dev" do |server|
    server.vm.hostname = "gaephp.dev"
    server.vm.network :private_network, ip: "192.168.33.35"
    server.vm.provider "virtualbox" do |vb|
      vb.name = config.vm.box.gsub(/\//, "_") + "_" + server.vm.hostname
      vb.memory = 4096
      vb.cpus = 2
      vb.customize [
        "modifyvm", :id,
        "--hwvirtex", "on",
        "--nestedpaging", "on",
        "--largepages", "on",
        "--ioapic", "on",
        "--pae", "on",
      ]
    end

    server.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.inventory_path = "hosts"
      ansible.limit = "all"
    end

  end
end
