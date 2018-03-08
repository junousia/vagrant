# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.ssh.insert_key = false
  config.ssh.private_key_path = [File.expand_path('~/.vagrant.d/insecure_private_key'), File.expand_path('~/.ssh/id_rsa')]
  config.vm.define "dev64" do |dev64|
    config.vm.network "public_network", type: "dhcp"
  end

  config.vm.hostname = "dev64"
  config.vm.box_check_update = false
  config.ssh.forward_agent = true

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "dev64.yml"
    ansible.extra_vars = {
      target_user: ENV['USER'],
    }
  end

  config.vm.provision :shell do |shell|
      shell.inline = "touch $1 && chmod 0440 $1 && echo $2 > $1"
      shell.args = %q{/etc/sudoers.d/root_ssh_agent "Defaults    env_keep += \"SSH_AUTH_SOCK\""}
  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "dev64"
    vb.memory = 1024
    vb.cpus = 1
  end
end
