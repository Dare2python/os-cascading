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
  tmux
  ./stack.sh

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
  # HOST_IP=172.16.245.216

  disable_service n-net
  disable_service tempest
  disable_service heat
  OPAE_INSTALL_ENABLE=false
  # disable_service horizon
  EOF
  cat ./local.conf
  tmux
  ./stack.sh

```
