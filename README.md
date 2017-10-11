foreman_proxy_provisioning
=========
The role configures provisioning proxy via foreman-installer and then delegates creation of foreman subnet to ```hammer_client_host```.

Requirements
------------
```hammer_client_host``` should have hammer-cli installed and configured.

Role Variables
--------------

| Name    | Description    | Required    | Default    | Values | Examples |
|:--|:--|:-:|:-:|:-:|:--|
| foreman_provisioning_dhcp_interface | DHCP listen interface | No | ansible_default_ipv4.interface | - | eth0 |
| foreman_provisioning_dhcp_gateway | DHCP pool gateway | No | ansible_default_ipv4.gateway | - | 192.168.1.2 |
| foreman_provisioning_dhcp_network | Proxy host network  | No | ansible_default_ipv4.network | - | 192.168.1.0 |
| foreman_provisioning_dhcp_netmask | Proxy host netmask  | No | ansible_default_ipv4.network | - | 255.255.255.0 |
| foreman_provisioning_dhcp_start | DHCP pool range first IP address  | Yes | Proxy hosts ipv4 interface IP + 1 (calculated in the role) | - | 192.168.1.1 |
| foreman_provisioning_dhcp_end | DHCP pool range last IP address | Yes | Last IP in the subnet - 2 (calculated in the role) | - | 192.168.1.254 |
| foreman_provisioning_proxy_name | Proxy name | No | ansible_fqdn | - | proxy.example.com |
| foreman_ipam | IP Address Managment | Yes | DHCP | - | Internal DB |
| hammer_client_host | The hostname of the system where hammer command will run | Yes | - | - | master.example.com |
| configure_subnet | Should a new subnet be configured in Foreman | No | True | - | - |

Dependencies
------------

* `genadipost.epel_repositories`
* `genadipost.puppet_repositories`
* `genadipost.foreman_repositories`
* `genadipost.foreman_installer`

Example Playbook
----------------

```yml
---
- name: Configure foreman provisioning proxy
  hosts: proxies
  roles:
  - role: foreman_proxy_provisioning
    foreman_provisioning_dhcp_interface: ens33
    foreman_provisioning_dhcp_gateway: 192.168.1.254
    foreman_provisioning_dhcp_network: 192.168.1.0
    foreman_provisioning_dhcp_netmask: 255.255.255.0
    foreman_provisioning_dhcp_start: 192.168.1.55
    foreman_provisioning_dhcp_end: 192.168.1.200
    foreman_provisioning_proxy_name: proxy1.example.com
    foreman_ipam: 'Internal DB'
    hammer_client_host: foreman-master.example.com
    configure_subnet: yes
```

License
-------

BSD

Author Information
------------------

genadipost@gmail.com
