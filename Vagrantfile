# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end
  config.vm.define "portus" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "portus"
    d.vm.network "private_network", ip: "192.168.57.88"
#    d.vm.network "public_network", bridge: "eno4", ip: "192.168.57.88", auto_config: "false", netmask: "255.255.255.0" , gateway: "192.168.57.1"
#    default_router = "192.168.57.1"
#    d.vm.provision :shell, inline: "ip route delete default 2>&1 >/dev/null || true; ip route add default via #{default_router}"     
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/portus.yml -c local"
#    d.vm.provision :shell , inline: "systemctl restart network"
    d.vm.provider "virtualbox" do |v|
      v.memory = 3072
      v.cpus = 2
    end
  end 
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = true
    config.vbguest.no_remote = true
  end
end
