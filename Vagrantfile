# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

# The Jenkins master

  config.vm.define "Jenkins" do |jenkins|

      jenkins.vm.hostname = 'Jenkins'
      # The jenkins master needs to run on an LTS release for the
      # geerlingguy.java role
      jenkins.vm.box = "generic/ubuntu1910"
      jenkins.vm.network :forwarded_port, guest: 8080, host: 8080, host_ip: "0.0.0.0"
      jenkins.vm.network :private_network, ip: "10.10.10.10"
      jenkins.vm.boot_timeout = 600

      jenkins.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true

      jenkins.ssh.forward_agent = true
      jenkins.ssh.forward_x11 = true

      jenkins.vm.provider :libvirt do |v|
	    v.storage_pool_name = "images"
            v.memory = 1024
            v.cpus = 1
	    v.disk_bus = "virtio"
      end
  end


# Jenkins builders start here

  config.vm.define "Ubuntu" do |ubuntu|

      ubuntu.vm.hostname="Ubuntu"
      ubuntu.vm.box = "generic/ubuntu1910"
      ubuntu.vm.network :private_network, ip: "10.10.10.100"
      ubuntu.vm.boot_timeout = 600

      ubuntu.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true

      ubuntu.ssh.forward_agent = true
      ubuntu.ssh.forward_x11 = true

      ubuntu.vm.provider :libvirt do |v|
	    v.storage_pool_name = "images"
            v.memory = 1024
            v.cpus = 1
	    v.disk_bus = "virtio"

      end
  end


  config.vm.define "Debian" do |debian|

      debian.vm.hostname="Debian"
      debian.vm.box = "debian/buster64"
      debian.vm.network :private_network, ip: "10.10.10.101"
      debian.vm.boot_timeout = 600

      debian.vm.synced_folder '.', '/vagrant', type: 'rsync', disabled: true

      debian.ssh.forward_agent = true
      debian.ssh.forward_x11 = true

      debian.vm.provider :libvirt do |v|
	    v.storage_pool_name = "images"
            v.memory = 1024
            v.cpus = 1
	    v.disk_bus = "virtio"
      end
  end

  config.vm.define "FreeBSD" do |freebsd|

      freebsd.vm.hostname="FreeBSD"
      freebsd.vm.box = "generic/freebsd12"
      freebsd.vm.network :private_network, ip: "10.10.10.102"
      freebsd.vm.boot_timeout = 600

      freebsd.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true

      freebsd.ssh.shell = "sh"
      freebsd.ssh.forward_agent = true
      freebsd.ssh.forward_x11 = true

      freebsd.vm.provider :libvirt do |v|
	    v.storage_pool_name = "images"
            v.memory = 1024
            v.cpus = 1
	    #v.disk_bus = "virtio"
            #v.graphics_port = "5090"
	    v.graphics_ip = "0.0.0.0"
            v.cpu_mode = "custom"
            #host-passthrough"
      end
  end

  config.vm.define "OpenBSD" do |openbsd|

      openbsd.vm.hostname="OpenBSD"
      openbsd.vm.box = "generic/openbsd6"
      openbsd.vm.network :private_network, ip: "10.10.10.103"
      openbsd.vm.boot_timeout = 600

      openbsd.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true

      openbsd.ssh.shell = "sh"
      openbsd.ssh.forward_agent = true
      openbsd.ssh.forward_x11 = true

      openbsd.vm.provider :libvirt do |v|
	    v.storage_pool_name = "images"
            v.memory = 1024
            v.cpus = 1
            #v.cpu_mode = "custom"
	    v.disk_bus = "virtio"
      end
  end
#
#  config.vm.define "OpenIndiana" do |openindiana|
#
#      #openindiana.vm.hostname="OpenIndiana"
#      openindiana.vm.box = "openindiana/hipster"
#      #openindiana.vm.network "private_network", ip: "10.10.10.104"
#      openindiana.vm.boot_timeout = 600
#
#      openindiana.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
#
#      openindiana.ssh.shell = "sh"
#      openindiana.ssh.forward_agent = true
#      openindiana.ssh.forward_x11 = true
#
#      openindiana.vm.provider :libvirt do |v|
#            v.name = "OpenIndiana"
#            v.memory = 2048
#            v.cpus = 2
#      end
#  end
#
#  config.vm.define "DragonFlyBSD" do |dragonfly|
#
#      #dragonfly.vm.hostname="DragonFlyBSD"
#      dragonfly.vm.box = "b00ga/dragonfly50"
#      #dragonfly.vm.network "private_network", ip: "10.10.10.105"
#      dragonfly.vm.boot_timeout = 600
#
#      dragonfly.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
#
#      dragonfly.ssh.shell = "sh"
#      dragonfly.ssh.forward_agent = true
#      dragonfly.ssh.forward_x11 = true
#
#      dragonfly.vm.provider :libvirt do |v|
#            v.name = "DragonFlyBSD"
#            v.memory = 1024
#            v.cpus = 2
#      end
#  end


# Ansible configuration here, add new builders to jenkins_builders

  config.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/site.yml"
            ansible.verbose = true
            #ansible.verbose = "vvv"

            ansible.groups = {
                  "jenkins_master" => ["Jenkins"],
                  "jenkins_builders" => ["Ubuntu", "Debian", "OpenBSD", "FreeBSD"],
            }

            ansible.host_vars = {
                  #"DragonFlyBSD" => { "ansible_python_interpreter" => "/usr/local/bin/python" },
                  "FreeBSD" => { "ansible_python_interpreter" => "/usr/local/bin/python" },
                  "OpenBSD" => { "ansible_python_interpreter" => "/usr/local/bin/python2" },
            }
  end
end
