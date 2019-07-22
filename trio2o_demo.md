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
  
### Create VM
```bash
  openstack server create --flavor a1.2xlarge --image bionic-server-cloudimg-amd64-20190612 --network 192 --nic v4-fixed-ip=192.168.0.30 --availability-zone nova --key-name cascading node-1
```
