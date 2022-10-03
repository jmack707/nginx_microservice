# Ansible Collection - nginx_microservice.automation

Documentation for the collection.




How to create vxlan tunnel
tmsh create net tunnels vxlan fl-vxlan port 8472 flooding-type none
tmsh create net tunnels tunnel fl-vxlan key 1 profile fl-vxlan local-address 10.1.20.4
tmsh create net self 10.244.20.91 address 10.244.20.91/255.255.0.0 allow-service none vlan fl-vxlan

