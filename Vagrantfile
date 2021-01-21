# Install vagrant-disksize to allow resizing the vagrant box disk.
unless Vagrant.has_plugin?("vagrant-disksize")
    raise  Vagrant::Errors::VagrantError.new, "vagrant-disksize plugin is missing. Please install it using 'vagrant plugin install vagrant-disksize' and rerun 'vagrant up'"
end

Vagrant.configure("2") do |config|

    #####################
    # Métodos/Funciones #
    #####################
    
    def provisioning(config, shell_arguments)
      # Install Guest Additions
      config.vbguest.auto_update = true
    
      # Trigger reload
      config.vm.provision :reload
    
      # Provision and Update
      config.vm.provision "shell", path: "provision.sh", args: shell_arguments
    
      # Trigger reload
      config.vm.provision :reload
    
      # Install Java and Jenkins
      config.vm.provision "shell", path: "setup.sh", args: shell_arguments
    end
    
    #########################################
    # Defino providers VirtualBox #
    #########################################
    
      config.vm.provider "virtualbox" do |vb|
        #Don't boot with headless mode
        #vb.gui = true
    
        # Use VBoxManage to customize the VM. 
        # For example to change memory
        vb.customize ["modifyvm", :id, "--memory", "4096"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
      end
    
    ################################################
    # Defino entorno de Desarrollo y de Producción #
    ################################################
    
      config.vm.box = "bento/centos-7.4"
      config.vm.hostname = "awx-node-001"
      #config.disksize.size = '40GB'
      
      # Provisiono 
      provisioning(config, ["config", "virtualbox"])
    
      # To enable low ports
      # https://stackoverflow.com/questions/17437137/vagrant-wont-forward-only-port-80
      config.vm.network "forwarded_port", guest: 80, host: 8080, id: "http-awx"
      #config.vm.network "forwarded_port", guest: 443, host: 8443, id: "https-awx"
    
    end
    