# Tricircle project

## Description

The [Tricircle](https://wiki.openstack.org/wiki/Tricircle) is dedicated for networking automation across Neutron in multi-region OpenStack deployment.
The mission is to provide networking automation across Neutron in multi-region OpenStack deployments


[OPNFV Multisite](https://wiki.opnfv.org/display/multisite/Multisite) is NFV related incarnation of Tricircle project. It looks stale as well. The last meeting was held on May 18th, 2017. Last [commit](https://github.com/opnfv/multisite) is on 30 Mar 2017.

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
[Script](tricircle_demo.md)
