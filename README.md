[![License](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/grycap/ansible-role-singularity-registry.svg?branch=master)](https://travis-ci.org/grycap/ansible-role-singularity-registry)

Ansible Role - Singularity Registry
=========

It provides a totally customizable Ansible role for the installation of Nomad. If the variable ```create_nomad_service``` is ```true```, this role creates a Linux service.  

Role Variables
--------------

The variables used for the installation and configuration are described in defaults/main file. 

Example Playbook
----------------

Deployment of client and server with Consul enabled (and available at 172.17.0.2):
``` yml
    - hosts: servers
      vars:
        name: server 
        nomad_user: nomad
        nomad_group: nomad
        bind_address: "172.17.0.3"
        server_enabled: true
        client_enabled: false
        use_consul: true
        consul_address: "172.17.0.2:8500"
        create_nomad_service: true
      roles:
         - { role: grycap.nomad }

    - hosts: clients
      vars:
        name: server 
        nomad_user: nomad
        nomad_group: nomad
        bind_address: "172.17.0.4"
        server_enabled: false
        client_enabled: true
        use_consul: true
        consul_address: "172.17.0.2:8500"
        create_nomad_service: true
      roles:
         - { role: grycap.nomad }
```

License
-------

Apache 2.0
