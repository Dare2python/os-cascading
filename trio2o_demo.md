# Demo for Trio2o

Demonstration fro Trio2o concept is designed as two DevStack nodes tied together with a single API gateway,

## Setup first Node (node-1)

### Setup OpenStack client and the demo cloud credentials
```bash
  python --version
  
  pip install openstackclient
  
  openstack --version
  
  source ../*openrc.sh
  
  nova list
  ```

### Create Network
```bash
  openstack network create 192
  openstack subnet create subnet1 --network 192 --subnet-range 192.168.0.0/24

```
  
### Create first VM
```bash
  openstack server create --flavor a1.2xlarge --image bionic-server-cloudimg-amd64-20190612 --nic net-id=4eedc1ae-1cb8-412c-8f0a-5069a86a7543,v4-fixed-ip=192.168.0.30 --availability-zone nova --key-name cascading node-1
  openstack server add floating ip --fixed-ip-address 192.168.0.30 node-1 172.16.246.27
  openstack server show node-1
```

### Create second VM
```bash
  openstack server create --flavor a1.2xlarge --image bionic-server-cloudimg-amd64-20190612 --nic net-id=4eedc1ae-1cb8-412c-8f0a-5069a86a7543,v4-fixed-ip=192.168.0.40 --availability-zone nova --key-name cascading node-2
  openstack server add floating ip --fixed-ip-address 192.168.0.40 node-2 172.16.245.216
  openstack server show node-2
```
