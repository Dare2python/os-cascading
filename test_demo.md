# Test demo-magic script

```bash
# ssh-keyscan -H 172.16.245.216 >> ~/.ssh/known_hosts
ssh ubuntu@172.16.245.216
uname -a
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo su - stack
git clone https://opendev.org/openstack/devstack
cd devstack
```
-----

```bash
ping -c 2 192.168.0.30
ip -c a s ens3
```
