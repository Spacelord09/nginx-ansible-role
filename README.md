Nginx Ansible role
==================

This Ansible role has the goal to install a functional nginx instance with custom configuration on a system of your choice, running debian.

Currently, only Debian bullseye is supported.
Contributions to get more systems compatible are welcome.

ToDo
----

<br/> 

Installation
------------
```
ansible-galaxy install spacelord09.nginx
```

Role Variables
--------------

| Variable                 | Example Value | Default Value                                                                                                            | Description                                                  |
|--------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| nginx_clear_config       | true          | false                                                                                                                    | Delete config files defined in nginx_clear_config_files.     |
| nginx_clear_config_files | -             | - "/etc/nginx/conf.d/",<br>- "/etc/nginx/snippets/"<br>- "/etc/nginx/sites-available/"<br>- "/etc/nginx/sites-enabled/"  | Which files should be deleted if nginx_clear_config is true. |
| nginx_deploy_config      | false         | true                                                                                                                     | Should configuration files be deployed?                      |
| nginx_config_fileglob    | -             | - "{ ansible_hostname }/nginx.conf"<br>- "{ ansible_hostname }/snippets/*"<br>- "{ ansible_hostname }/conf.d/*"          | Defines which configuration files will be deployed.          |

Example Playbook with default variables
---------------------------------------
```
- name: Nginx Deployment.
  gather_facts: true
  hosts: all
  roles:
    - spacelord09.nginx
```

Example Playbook with customizations
------------------------------------
```
- name: Nginx Deployment.
  gather_facts: true
  hosts: all
  tasks:

    - include_role:
        name: spacelord09.nginx
      when: "lookup('filetree', ansible_hostname + '/' ) != []" # Runs only if the config folder is existing and contains files.
      vars:
        nginx_clear_config: false
        nginx_clear_config_files:
          - "/etc/nginx/conf.d/"
          - "/etc/nginx/snippets/"
          - "/etc/nginx/sites-available/"
          - "/etc/nginx/sites-enabled/"

        nginx_deploy_config: true
        nginx_config_fileglob:
          - "{{ ansible_hostname }}/nginx.conf"
          - "{{ ansible_hostname }}/snippets/*"
          - "{{ ansible_hostname }}/conf.d/*"
  
```

Where should I put my configs files?
------------------------------------
Configuration files should be placed in a directory named after the corresponding hostname that is going to be deployed.
<br/> For example:
```
.
├── playbook.yml
└── my-hostname
    ├── conf.d
    │   ├── example.com.conf
    │   ├── default.conf
    └── nginx.conf
```

License
-------

BSD-2-Clause

Author Information
------------------

Written by Spacelord09 / Leon Meier.
