# Nutanix Role to create or delete an AHV subnet (port group)

This Ansible role will create or delete a defined subnet on a Nutanix cluster running AHV.


## Role Variables

| Variable                       | Required | Default  | Choices                                                                         | Comments                                                                                                                                                                                                                          |
|--------------------------------|----------|----------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nutanix_host                   | yes      |          |                                                                                 | The IP address or FQDN for the Prism (Element or Central) to which you want to connect.                                                                                                                                           |
| nutanix_username               | yes      |          |                                                                                 | A valid username with appropriate rights to access the Nutanix API.                                                                                                                                                               |
| nutanix_password               | yes      |          |                                                                                 | A valid password for the supplied username.                                                                                                                                                                                       |
| nutanix_port                   | no       | 9440     |                                                                                 | The Prism TCP port.                                                                                                                                                                                                               |
| validate_certs                 | no       | no       | yes / no                                                                        | Whether to check if Prism UI certificates are valid.                                                                                                                                                                              |
| nutanix_subnets                | no       | []       |                                                                                 |                                                                                                                                                                                                                                   |


### nutanix_subnets dict format

| Variable                       | Required | Default  | Choices                                                                         | Comments                                                                                                                                                                                                                          |
|--------------------------------|----------|----------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                           | yes      |          |                                                                                 | The name of the virtual switch                                                                                                                                                                                                    |
| state                          | yes      |          | [ present , absent ]                                                            | Whether the switch should be created or removed                                                                                                                                                                                   |
| virtual_switch                 | no       | vs0      |                                                                                 | The virtual switch where the new subnet should be created                                                                                                                                                                         |
| vlan_id                        | yes      |          |                                                                                 | The VLAN ID for the new subnet                                                                                                                                                                                                    |
| net_address                    | no       |          |                                                                                 | Required for AHV IPAM. The network address in CIDR format (eg. 192.168.0.0/24)                                                                                                                                                    |
| gateway                        | no       |          |                                                                                 | Required for AHV IPAM. The gateway for the new network                                                                                                                                                                            |
| dns_server_list                | no       |          |                                                                                 | AHV IPAM. List of DNS servers for the new network                                                                                                                                                                                 |
| domain_name                    | no       |          |                                                                                 | AHV IPAM. Domain suffix for all servers on this subnet                                                                                                                                                                            |
| domain_search                  | no       |          |                                                                                 | AHV IPAM. Domain search string for all servers on this subnet                                                                                                                                                                     |
| ipam.start_ip                  | no       |          |                                                                                 | AHV IPAM. Start address for AHV IPAM                                                                                                                                                                                              |
| ipam.end_ip                    | no       |          |                                                                                 | AHV IPAM. End address for AHV IPAM                                                                                                                                                                                                |


## Dependencies

None


## Example Playbooks

This playbook will create a subnet called Primary on the default VLAN.
```
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.nutanix_role_prism_ha
  vars:
    nutanix_host: 10.38.185.37
    nutanix_username: admin
    nutanix_password: nx2Tech165!
    nutanix_subnets:
      - name: Primary
        state: present
        virtual_switch: vs0
        vlan_id: 0
```

This playbook will remove a subnet called Primary.
```
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.nutanix_role_prism_ha
  vars:
    nutanix_host: 10.38.185.37
    nutanix_username: admin
    nutanix_password: nx2Tech165!
    nutanix_subnets:
      - name: Primary
        state: absent
```

This playbook will create a subnet called Primary configured with AHV IPAM.
```
- hosts: localhost
  gather_facts: false
  roles:
    - role: grdavies.nutanix_role_prism_ha
  vars:
    nutanix_host: 10.38.185.37
    nutanix_username: admin
    nutanix_password: nx2Tech165!
    nutanix_subnets:
      - name: Primary
        state: present
        virtual_switch: vs0
        vlan_id: 0
        net_address: 10.38.186.0/25
        gateway: 10.38.186.1
        dns_server_list:
          - 10.42.194.10
        domain_name: ntnxlab.local
        domain_search: ntnxlab.local
        ipam:
          - start_ip: 10.38.185.50
            end_ip: 10.38.185.126
```


## License

See LICENSE.md

## Author Information

Ross Davies
