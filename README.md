# k3sup_k3s_ha_server_plus_agent_using_vagrant_libvirt

Vagrant setup for using k3sup to install a k3s cluster with three servers and one worker (aka agent), using the libvirt provider 

This setup creates three k3s server VMs and one VM as k3s agent (aka worker) and prepares a k3sup installation script via Ansible.

Default OS is openSUSE Leap 15.2, but that can be changed in the Vagrantfile. Same holds true for the sizing of the machines.

## Vagrant

1. You need vagrant obviously.
2. Fetch the box, per default this is `opensuse/Leap-15.2.x86_64`, using `vagrant box add opensuse/Leap-15.2.x86_64`.
3. Make sure the git submodules are fully working by issuing `git submodule init && git submodule update`
4. Run `vagrant up`
5. Inspect and run the `k3sup_installation.sh` script
6. Run `kubectl --kubeconfig kubeconfig_k3sup_ha.yaml get nodes` and you should see your server and agent nodes.
7. Party!

## Cleaning up

When tearing down the the vagrant VMs using `vagrant destroy`, the files created by Ansible are not being remove.

You can do this (using Ansible...) by running `ansible-playbook ansible/playbook-DELETE_everything_to_start_from_scratch.yml`.

## Creating additional agent nodes

You can modify the Vagrantfile to create additional agent nodes by tweaking two lines.

1. Setting the number of agents (in this example to `2`):

```
  ###################################################################################
  # define number of agents
  W = 2
```

2. Adding the additional agent nodes to the `ansible_groups` line:
```
      ansible.groups = {
        "k3sservers"  => [ "k3sserver1", "k3sserver2", "k3sserver3" ],
        "k3sagents"   => [ "k3sagent1", "k3sagent2" ]
      }
```

## Creating additional server nodes

You can modify the Vagrantfile to create additional server nodes by tweaking two lines similar to adjusting the agent number described above.
