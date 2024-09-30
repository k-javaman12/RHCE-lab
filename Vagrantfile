# Vagrantfile for Rocky Linux 9.4 Environment
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use same SSH key for each machine
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  # Define base box
  BOX_NAME = "generic/rocky9"

  # Repo Machine
  config.vm.define "repo" do |repo|
    repo.vm.box = BOX_NAME
    repo.vm.hostname = "repo.ansi.example.com"
    repo.vm.network "private_network", ip: "192.168.56.199"
    repo.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    repo.vm.provision "shell", inline: <<-SHELL
      sudo dnf install -y epel-release
      sudo dnf install -y sshpass python3-pip python3-devel httpd vsftpd createrepo
      python3 -m pip install --upgrade pip
      python3 -m pip install pexpect ansible
    SHELL
  end

  # Define a method to configure nodes
  def configure_node(config, name, ip, memory)
    config.vm.define name do |node|
      node.vm.box = BOX_NAME
      node.vm.hostname = "#{name}.ansi.example.com"
      node.vm.network "private_network", ip: ip
      node.vm.provider "virtualbox" do |vb|
        vb.memory = memory
      end
      node.vm.provision "shell", inline: <<-SHELL
        echo 'Provisioning #{name}'
        # Additional provisioning steps can go here
      SHELL
    end
  end

  # Configure nodes
  configure_node(config, "control", "192.168.56.200", "2048")
  configure_node(config, "node1", "192.168.56.201", "1024")
  configure_node(config, "node2", "192.168.56.202", "1024")
  configure_node(config, "node3", "192.168.56.203", "512")
  configure_node(config, "node4", "192.168.56.204", "512")
end
