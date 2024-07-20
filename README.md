# microk8s_vagrant_libvirt_ansible

This setup creates a VM with Ubuntu 23.10 and installs microk8s.

It enables the ingress and hostpath-storage addons in microk8s and adds the
vagrant user on the VM to the `microk8s` group.

After the installation it fetches the `kubeconfig` file from the VM and saves it
as `microk8s-kubeconfig` in the `ansible` directory. It modifies the file so it
does no longer contain `127.0.0.1` but rather the VM's actual IP address.

Default OS is Ubuntu 23.10. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

There is a branch called `Ubuntu_22.04` that uses Ubuntu 22.04 as base operating
system.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `generic/ubuntu2204`, using
   `vagrant box add generic/ubuntu2204`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Run `kubectl --kubeconfig ansible/microk8s-kubeconfig get nodes` and you
   should see your cluster's only node.
1. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install microk8s (because you want to install
it yourself), just comment out the following lines in the `Vagrantfile`:

```hcl
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```

You also find all of the playbooks in the `ansible` folder.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
