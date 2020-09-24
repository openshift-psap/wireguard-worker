# wg-scale 

This repository contains [Ansible](https://www.ansible.com/) roles and
playbooks to create a [WireGuard](https://www.wireguard.com/) VPN between machines.
Once this VPN is established we run [openshift-ansible](https://github.com/openshift/openshift-ansible) to
scale up the cluster and add this new node.

## Requirements

- RHEL7.x or RHEL8.x
- Ansible >= 2.9.5
- Passwordless SSH between nodes

# Quickstart

## Create an Ansible Inventory

Create an inventory file with the appropriate groups and variables defined.
An example inventory can be found in [inventory/hosts.example](inventory/hosts.example).

Required variables include:

- `address` - New IP address of the "server" on the VPN subnet
- `vpn_network` - VPN network subnet (three octets only: `192.168.111`)
- `allowed_ips` - Existing CIDR for private baremetal network (CIDR notation: `192.168.222.0/24`)
- `endpoint` - Public IP of new client endpoint (keep port the default: `10.10.10.324:51820`)
- `rhsm_user` - If you need to run subscription manager on the new node, supply the RHSM username
- `rhsm_password` - If you need to run subscription manager on the new node, supply the RHSM password

## Run the install_wg playbook

```bash
cd wg-scale
ansible-playbook -i inventory/hosts playbooks/install_wg.yml
```

NOTE: There is one step that requires user intervention for now and that is the firewall rules
to masquerade between the new wg0 interface on the server and the private NIC and/or the public NIC.

# Further reading

## Contributing

See the [contribution guide](CONTRIBUTING.md).

