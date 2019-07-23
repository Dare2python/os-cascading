# Demo script for Trio2o

Demonstration for Trio2o concept is designed as two DevStack nodes tied together with a single API gateway,

## Setup OpenStack client and the demo cloud credentials
```bash
python --version
pip install openstackclient
openstack --version
source ../*openrc.sh
nova list
```

## Setup common infra

### Create Network
```bash
openstack network create 192
openstack subnet create subnet1 --network 192 --subnet-range 192.168.0.0/24
```
## Setup first Node (node-1)

### Create first VM
```bash
openstack server create --flavor a1.2xlarge --image bionic-server-cloudimg-amd64-20190612 --nic net-id=4eedc1ae-1cb8-412c-8f0a-5069a86a7543,v4-fixed-ip=192.168.0.30 --availability-zone nova --key-name cascading node-1
openstack server add floating ip --fixed-ip-address 192.168.0.30 node-1 172.16.246.27
openstack server show node-1
```

### Deploy DevStack on first VM
```bash
ssh-keyscan -H 172.16.246.27 >> ~/.ssh/known_hosts
ssh ubuntu@172.16.246.27
uname -a
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo su - stack
git clone https://opendev.org/openstack/devstack
cd devstack

ip -c a s ens3

cat > ./local.conf << 'EOF'
[[local|localrc]]
DATABASE_PASSWORD=password
RABBIT_PASSWORD=password
SERVICE_PASSWORD=password
SERVICE_TOKEN=password
ADMIN_PASSWORD=password

# Enable Trio2o
TRIO2O_START_SERVICES=True
enable_plugin trio2o https://opendev.org/x/trio2o

# Change the HOST_IP address to the host's IP address where
# the Trio2o is running
# HOST_IP=172.16.246.27

disable_service n-net
disable_service tempest
disable_service heat
OPAE_INSTALL_ENABLE=false
# disable_service horizon
EOF

cat ./local.conf
tmux \
  new-session  "./stack.sh ; read" \; \
  split-window "watch -n 0.5 pstree ; read" \; \
  set-option -g mouse on \; \
  select-layout even-horizontal


```

### Test Trio2o on first VM
```bash
source openrc admin admin
unset OS_REGION_NAME
openstack --os-region-name=RegionOne endpoint list
token=$(openstack --os-region-name=RegionOne token issue | awk 'NR==5 {print $4}')
echo $token
curl -X GET http://127.0.0.1:19996/v1.0/pods -H "Content-Type: application/json" \
    -H "X-Auth-Token: $token"
neutron --os-region-name=RegionOne net-create net1
neutron --os-region-name=RegionOne subnet-create net1 10.0.0.0/24
glance --os-region-name=RegionOne image-list
nova --os-region-name=RegionOne flavor-create test 1 1024 10 1
nova --os-region-name=RegionOne flavor-list

# nova --os-region-name=RegionOne boot --flavor 1 --image $image_id --nic net-id=$net_id vm1
# nova --os-region-name=RegionOne boot --flavor 1 --image aa5d3c84-a095-47a1-91e5-479a2dde80ce --nic net-id=d69d3db6-0f93-43b4-bc23-c7a04768ab75 vm1
openstack server create --os-region-name=RegionOne vm1 --net net1 --flavor test --image cirros-0.4.0-x86_64-disk

nova --os-region-name=RegionOne list
nova --os-region-name=Pod1 list
neutron --os-region-name=RegionOne port-list
openstack server show vm1

openstack volume create --os-region-name=RegionOne --availability-zone=az1 1 --size 1 
openstack volume  list 
openstack volume  list --os-region-name=RegionOne
# cinder show fd437a9f-44b4-4e97-a366-4b4c4bbf3695
openstack volume show fd437a9f-44b4-4e97-a366-4b4c4bbf3695

nova --debug --os-region-name=RegionOne list
# cinder --debug --os-region-name=RegionOne list
openstack --debug  volume show fd437a9f-44b4-4e97-a366-4b4c4bbf3695

```


## Setup second Node (node-2)

### Create second VM
```bash
openstack server create --flavor a1.2xlarge --image bionic-server-cloudimg-amd64-20190612 --nic net-id=4eedc1ae-1cb8-412c-8f0a-5069a86a7543,v4-fixed-ip=192.168.0.40 --availability-zone nova --key-name cascading node-2
openstack server add floating ip --fixed-ip-address 192.168.0.40 node-2 172.16.245.216
openstack server show node-2
```

### Deploy DevStack on second VM
```bash
ssh-keyscan -H 172.16.245.216 >> ~/.ssh/known_hosts
ssh ubuntu@172.16.245.216
uname -a
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo su - stack
git clone https://opendev.org/openstack/devstack
cd devstack

ping -c 2 192.168.0.30
ip -c a s ens3

cat > ./local.conf << 'EOF'
[[local|localrc]]

DATABASE_PASSWORD=password
RABBIT_PASSWORD=password
SERVICE_PASSWORD=password
SERVICE_TOKEN=password
ADMIN_PASSWORD=password

NEUTRON_CREATE_INITIAL_NETWORKS=False

# the region name of this OpenStack instance, and it's also
# the pod name in Trio2o
REGION_NAME=Pod2

# Change the HOST_IP, SERVICE_HOST and GLANCE_SERVICE_HOST to
# the host's IP address where the Pod2 is running
# HOST_IP=192.168.0.40
# SERVICE_HOST=192.168.0.40

# Use the KeyStone which is located in RegionOne, where the Trio2o is
# installed, change the KEYSTONE_SERVICE_HOST and KEYSTONE_AUTH_HOST to
# host's IP address where the KeyStone is served.
KEYSTONE_REGION_NAME=RegionOne
KEYSTONE_SERVICE_HOST=192.168.0.30
KEYSTONE_AUTH_HOST=192.168.0.30

# Use the Glance which is located in RegionOne, where the Trio2o is
# installed
GLANCE_SERVICE_HOST=192.168.0.30
disable_service g-api
disable_service g-reg

disable_service tempest
disable_service horizon
EOF

  
cat ./local.conf
tmux \
  new-session  "./stack.sh ; read" \; \
  split-window "watch -n 0.5 pstree ; read" \; \
  set-option -g mouse on \; \
  select-layout even-horizontal

```

## Clean up 
```bash
# openstack server delete node-1
# openstack server delete node-2

```
