[![License](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/grycap/ansible-role-singularity-registry.svg?branch=master)](https://travis-ci.org/grycap/ansible-role-singularity-registry)

Ansible Role - Singularity Registry
=========

It provides a totally customizable Ansible role for the installation of Singularity Registry and Singularity Registry Client.

Variables for sregistry
--------------

- `install_sregistry`: Boolean. Indicates if the role has to install sregistry. Default: True 
- `sregistry_url`: String. The role will download the sregistry from this GIT repository. Default: https://github.com/singularityhub/sregistry
- `sregistry_branch`: String. Branch of the GIT repository. Default: 'master'
- `sregistry_dir`: String. Path where sregistry will be installed. Default: '/opt/sregistry-{{ sregistry_branch }}'
- `sregistry_started`: Boolean. If false, it will not start the docker-compose.yml
- `sregistry_token`: String. The token of the secrets.py file. If it is '', the role will generate it. Default: ''
- `sregistry_secrets_vars`: List\<dict\>. Users could use this variable to configure the secrets.py. It must be definined as follows:
    ```yml
    sregistry_secrets_vars: 
    - { option: 'VAR_NAME_1', value: VAR_VALUE_1 }
    - { option: 'VAR_NAME_2', value: VAR_VALUE_2 }
    ```
- `sregistry_config_vars`: List\<dict\>. Users could use this variable to configure the config.py.It must be definined as follows:
    ```yml
    sregistry_config_vars: 
    - { option: 'VAR_NAME_1', value: VAR_VALUE_1 }
    - { option: 'VAR_NAME_2', value: VAR_VALUE_2 }
    ```
- `sregistry_plugins_enabled`: List\<String\>. Plugins of sregistry than will be availables in your installation. The allowed plugins are defined in vars/main.yml file:
    ```yml
    sregistry_allowed_plugins:
      - pam_auth
      - google_build
      - globus
      - saml_auth 
    ```

Variables for sregistry-cli
--------------
- `install_sregistry_ci` Boolean. Indicates if the role has to install sregistry-cli. Default: True. 
- `sregistry_cli_url`: String. The role will download the sregistry from this GIT repository. Default: https://github.com/singularityhub/sregistry-cli
- `sregistry_cli_branch`: String. Branch of the GIT repository. Default: 'master'
- `sregistry_cli_dir`: String. Path where sregistry will be installed. Default: '/opt/sregistry-cli-{{ sregistry_cli_branch }}'

- `sregistry_cli_use_docker`: Boolean. It indicates if sregistry-cli will be installed in the host or if the role has to build the docker image. Default: True
- `sregistry_cli_create_alias`: Boolean. It indicates if the user wants to create an alias for sregistry-cli in /root/.bashrc. Default: False

Example Playbook
----------------

Deployment of client and server with Consul enabled (and available at 172.17.0.2):
``` yml
- hosts: singularity-registry
  vars:
    # Variables to configure GITHUB authorization
    sregistry_secrets_vars: 
    - { option: 'SOCIAL_AUTH_GITHUB_KEY', value: "XXXXXXXXXX" }
    - { option: 'SOCIAL_AUTH_GITHUB_SECRET', value: "XXXXXXXXXX" }

    sregistry_config_vars: 
      - { option: 'ENABLE_GITHUB_AUTH', value: True }
      - { option: 'HELP_CONTACT_EMAIL', value: 'serlohu@upv.es' }
      - { option: 'HELP_INSTITUTION_SITE', value: 'https://www.upv.es'}
      - { option: 'REGISTRY_NAME', value: 'My Singularity Registry' }
      - { option: 'REGISTRY_URI', value: 'mysreg' }
      - { option: 'PRIVATE_ONLY', value: True }
    
    # Use PAM authorization
    sregistry_plugins_enabled:
      - pam_auth
    
    # sregistry-cli in Docker
    sregistry_cli_use_docker: true
   
  roles:
    - { role: grycap.singularity_registry } 

- hosts: general-nodes
  vars:
    # Do not install sregistry, only sregistry-cli
    install_sregistry : false  
    # sregistry-cli in Docker
    sregistry_cli_use_docker: true
   
  roles:
    - { role: grycap.singularity_registry } 

License
-------

Apache 2.0
