VAGRANTFILE_API_VERSION = "2"
$MEMSIZE=1024

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # The ordering of these 2 lines expresses a preference for a hypervisor
  config.vm.provider "virtualbox"
  config.vm.provider "vmware_fusion"

  config.ssh.forward_agent = false
  config.ssh.insert_key = false
  # Timeouts
  config.vm.boot_timeout = 900
  config.vm.graceful_halt_timeout=30

  # build_master
  config.vm.define :build_master, primary: true, autostart: true do |build_master|
    build_master.vm.box = "redesign/centos7"
    build_master.vm.box_check_update = false
    build_master.vm.synced_folder ".", "/vagrant", id: "vagrant-root"
    build_master.vm.network "private_network", ip: "192.168.10.28", :netmask => "255.255.255.0",  auto_config: true
    build_master.vm.network "forwarded_port", id: 'ssh', guest: 22, host: 2228, auto_correct: false
#    build_master.vm.network "forwarded_port", guest: 443, host: 8443, auto_correct: true
    build_master.vm.provision "shell", inline: "ifup enp0s8", run: "always"
    build_master.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "8192", "--natnet1", "172.16.1/24"]
      vb.gui = false
      vb.name = "build_master"
    end
#      build_master.vm.provision "ansible_local" do |ansible|
#        ansible.playbook = "/vagrant/provision.yml"
#        ansible.compatibility_mode = "2.0"
#        ansible.galaxy_role_file = "requirements.yml"
#        ansible.galaxy_roles_path = "galaxy_roles"
#        ansible.limit = "build_master"
#        ansible.verbose = 'vv'
#      end
  end
end
  