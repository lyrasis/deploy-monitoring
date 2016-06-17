# -*- mode: ruby -*-
# vi: set ft=ruby :

PLAYS = [
  { file: "base", restart: {} },
]

Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true

  config.vm.define "monitor" do |monitor|
    monitor.vm.box = "ubuntu/trusty64"
    monitor.vm.hostname = "monitor"
    monitor.vm.network :private_network, ip: "10.11.12.105"

    monitor.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--memory", 4096]
      if not RUBY_PLATFORM.downcase.include?("mswin")
        v.customize ["modifyvm", :id, "--cpus", `awk "/^processor/ {++n} END {print n}" /proc/cpuinfo 2> /dev/null || sh -c 'sysctl hw.logicalcpu 2> /dev/null || echo ": 2"' | awk \'{print \$2}\' `.chomp ]
      end
    end
  end

  PLAYS.each do |play|
    config.vm.provision :ansible do |ansible|
      ansible.playbook = "#{play[:file]}.yml"
      ansible.inventory_path = "inventory/vagrant/hosts"
      if play[:restart].any?
        r = play[:restart]
        i = r[:instance] || false
        a = r[:actions] || ["stop", "start"]
        ansible.extra_vars = {
          target: r[:target],
          instance: i,
          interval: r[:wait],
          actions: a,
          # do not enforce an ansible version with vagrant
          testing: true,
        }
      end
      ansible.limit = "all"
    end
  end
end
