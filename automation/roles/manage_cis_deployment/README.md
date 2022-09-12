Role Name
=========

This role creates the CIS container 

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

bigip_name: bigip1.local
bigip_mac: 00:50:56:bb:f7:a7
pods_cidr_net: 10.244.20.0/16
bigip_public_ip: 10.1.20.4
bigip_mgt: 10.1.1.245
bigip_dns_mgt: 10.1.1.245
bigip_tunnel_name: fl-vxlan
bigip_tunnel_cidr: 10.244.20.0/24


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
