# Trio2o project

## Description

The Trio2o is to provide APIs gateway for multiple OpenStack clouds, spanning in one site or multiple sites or in hybrid cloud, to act as a single OpenStack cloud.

Trio2o is API gateway (API entrance ) just like Nova API in API Cell, even simpler, for just forwarding the request, and leave the request parameter validation to be done in bottom OpenStacks. No VM/volume/backup/snapshot data will be stored in Trio2o.

Trio2o in a nutshell - use cases and technologies [presentation](https://docs.google.com/presentation/d/16laTyn4ra-446v4p0kwMnpgHqwzMsz1r6QeiSI2Kq2M/edit?pli=1#slide=id.p)

## Architecture

![Architecture diagram](https://wiki.openstack.org/w/images/d/d0/Trio2o_architecture.png "Trio2o Architecture diagram")

### Components
* Shared Keystone
* Nova API-GW
* Cinder API-GW 
* Admin API
* XJob
* Database

### Glance deployment
There are several options:
* Shared Glance (all OpenStack instances are located inside a high bandwidth, low latency site).
* Shared Glance with distributed back-end (OpenStack instances are located in several sites).
* Distributed Glance deployment (multiple site with distributed back-end).
* Separate Glance deployment (each site is installed with separate Glance instance and back-end, no cross site image sharing is needed).

## Demo
[Demo](trio2o_demo.md)
