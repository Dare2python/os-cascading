# OpenStack Cascading report as of July 2019


## Motivation
Research possibility and one particular solution for 
* Reducing OpenStack blast radius
* General OpenStack scalability question

by looking into OpenStack Cascading solution/prototype.

## OpenStack Cascading

This looks like a prototype for aggregating multiple small OpenStack clouds.
There are two versions:
* original [Cascading PoC](https://wiki.openstack.org/wiki/OpenStack_cascading_solution) presented in 2014-2015
* v2 communicated to OpenStack community during 2016-2017

Now it looks stale and abandoned with no recent commits, new contributors, fresh presentation etc.

## OpenStack Cascading solution

By the OpenStack community request initial stateful Cascading solution was split into 3 different efforts:
* [Trio2o](https://opendev.org/x/trio2o) (API gateway)
* [Tricircle](https://opendev.org/openstack/tricircle) (multi-pod networking on top of Neutron)
* [Jacket](https://opendev.org/x/jacket) (Cross cloud deployment, Cloud Burst - non-existent effectively)


## Demo
* [Trio2o](trio2o.md)
* [Tricircle](Tricircle.md)
