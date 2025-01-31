# -*- mode: ruby -*-
# vi: set ft=ruby :

## Insert a shim so that when people use 'vagrant ssh' they will login as them
# VAGRANT_COMMAND = ARGV[0]


# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # ssh shim
#   if VAGRANT_COMMAND == "ssh"
#       config.ssh.username = ENV['USER']
#   end

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  
  ## We'll use an Ubuntu base box for the moment.
  ## Later we'll probably provide our own base box
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # We'll forward ports for the webserver and the database to make local client tools easy to use
  # Automatically correct collisions (look at vagrant up output)
  config.vm.network "forwarded_port", guest: 80, host: 8888, auto_correct: true
  config.vm.network "forwarded_port", guest: 3306, host: 3333, auto_correct: true
  
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline <<-SHELL
  #   sudo apt-get install apache2
  # SHELL
  
  # You can have multiple machines defined in one Vagrantfile.  Here we're creating the
  # "qualitybox" machine.  Later on we could define a "db" machine etc. to more fully 
  # create a full stack locally
  config.vm.define "qualitybox"
  
  # define a pre-task to add the local user to the vagrant machine
#   config.vm.provision "shell" do |s|
#     s.path = 'vagrant/provision.sh'
#   end
  
  # add some bits that give ansible the ability to run
#   config.vm.provision "shell" do |s|
#     ssh_prv_key = ""
#     ssh_pub_key = ""
#     if File.file?("#{Dir.home}/.ssh/id_rsa")
#       ssh_prv_key = File.read("#{Dir.home}/.ssh/id_rsa")
#       ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
#     else
#       puts "No SSH key found. You will need to remedy this before pushing to the repository."
#     end
#     s.inline = <<-SHELL
#       if grep -sq "#{ssh_pub_key}" /home/vagrant/.ssh/authorized_keys; then
#         echo "SSH keys already provisioned."
#         exit 0;
#       fi
#       echo "SSH key provisioning."
#       mkdir -p /home/vagrant/.ssh/
#       touch /home/vagrant/.ssh/authorized_keys
#       echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
#       echo #{ssh_pub_key} > /home/vagrant/.ssh/id_rsa.pub
#       chmod 644 /home/vagrant/.ssh/id_rsa.pub
#       echo "#{ssh_prv_key}" > /home/vagrant/.ssh/id_rsa
#       chmod 600 /home/vagrant/.ssh/id_rsa
#       chown -R vagrant:vagrant /home/vagrant
#       exit 0
#     SHELL
#   end
#   
  # Use Ansible to provision our Vagrant guest
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision.yml"
    ansible.groups = {
      "mw_hosts" => ["qualitybox"]
    }
    ansible.verbose = "vvv"
    ansible.sudo = true
    ansible.sudo_user = "root"
    ansible.raw_ssh_args = ['-o IdentitiesOnly=yes']
# If we experience more issues with 'user', then this is how to specify
#    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
  end
  
end
