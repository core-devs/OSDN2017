########################################################################################################################
# DEFINITION
########################################################################################################################
(1..$DEPLOYMENT_NUMBER_OF_HOST).each do |i|
  Vagrant.configure("2") do |config|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.box_check_update = false

    config.vm.provider "virtualbox" do |v, override|
      override.ssh.username = $VIRTUALBOX_USERNAME
      override.vm.box = $VBOX_DEFAULT_BOXTYPE

      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
    end

    config.vm.define "#{$DEPLOYER_HOSTNAME}#{i}" do |g|
      g.vm.hostname = "#{$DEPLOYER_HOSTNAME}#{i}"

      g.vm.provider "virtualbox" do |vb, override|
        vb.name = "cdev::OSDN2017::Developer::#{$DEPLOYER_HOSTNAME}#{i}"
        override.vm.network "private_network", ip: "#{$NETWORK_INTERNAL_IP}#{i+$DEPLOYER_NETWORK_IPOFFSET}"

        vb.memory = 10240
        vb.cpus = 3

      end

      if i == $DEPLOYMENT_NUMBER_OF_HOST
        g.vm.provision "ansible" do |ansible|
          #ansible.verbose = "vvvv"
          ansible.limit="all"
          ansible.playbook = "update.yml"
          #ansible.groups = $GROUPS
          #ansible.host_vars = $HOSTS_VARS
        end
      end
    end
  end
end