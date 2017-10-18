########################################################################################################################
# DEFINITION
########################################################################################################################
Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |v, override|
    override.ssh.username = "vagrant"
    override.vm.box = "bento/ubuntu-16.04"

    v.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end

  config.vm.define "DEVINTRO" do |g|
    g.vm.hostname = "DEVINTRO"

    g.vm.provider "virtualbox" do |vb, override|
      vb.name = "cdev::OSDN2017::Developer::DEVINTRO"
      override.vm.network "private_network", ip: "10.0.5.100"

      vb.memory = 10240
      vb.cpus = 3

    end

    g.vm.provision "ansible" do |ansible|
      #ansible.verbose = "vvvv"
      ansible.limit="all"
      ansible.playbook = "update.yml"
      #ansible.groups = $GROUPS
        #ansible.host_vars = $HOSTS_VARS
    end
  end
end

