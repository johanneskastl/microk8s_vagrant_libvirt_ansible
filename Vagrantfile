Vagrant.configure("2") do |config|

  ###################################################################################
  config.vm.define "microk8s" do |node|

    # which image to use
    node.vm.box = "generic/ubuntu2310"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 8196
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "microk8s"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true

    # Ansible
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/microk8s-kubeconfig"
      trigger.run = {inline: "rm -vf ansible/microk8s-kubeconfig"}
    end # node.trigger.after

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/awx.crt"
      trigger.run = {inline: "rm -vf ansible/awx.crt"}
    end # node.trigger.after

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/awx.key"
      trigger.run = {inline: "rm -vf ansible/awx.key"}
    end # node.trigger.after

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/awx.csr"
      trigger.run = {inline: "rm -vf ansible/awx.csr"}
    end # node.trigger.after

  end # config.vm.define

end # Vagrant.configure
