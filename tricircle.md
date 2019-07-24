# Tricircle project

## Description

The [Tricircle](https://wiki.openstack.org/wiki/Tricircle) is dedicated for networking automation across Neutron in multi-region OpenStack deployment.

## Architecture

![Architecture diagram](https://wiki.openstack.org/w/images/c/c5/Tricircle_architecture.png "Tricircle Architecture diagram")

### Components
* Tricircle Local Neutron Plugin
* Tricircle Central Neutron Plugin
* Admin API
* XJob
* Database

### Glance deployment
There are several options:
* Shared Glance (all OpenStack instances are located inside a high bandwidth, low latency site).
* Shared Glance with distributed back-end (OpenStack instances are located in several sites).
* Distributed Glance deployment (multiple site with distributed back-end).
* Separate Glance deployment (each site is installed with separate Glance instance and back-end, no cross site image sharing is needed).

## Demo script
[Script](Tricircle_demo.md)