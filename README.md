Role Name
=========

A Qpid-Dispatch deployment role.

Requirements
------------

None.

Build/Test Status
------------

[![Build Status](https://travis-ci.org/rh-messaging-qe/ansible-qpid-dispatch.svg?branch=master)](https://travis-ci.org/rh-messaging-qe/ansible-qpid-dispatch)
[![GitHub Issues](https://img.shields.io/github/issues/rh-messaging-qe/ansible-qpid-dispatch.svg)](https://github.com/rh-messaging-qe/ansible-qpid-dispatch/issues)
[![GitHub Issues](https://img.shields.io/github/issues-pr/rh-messaging-qe/ansible-qpid-dispatch.svg)](https://github.com/rh-messaging-qe/ansible-qpid-dispatch/pulls)
![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Role Variables: Installation Variables
--------------

Variables controlling what will be executed by the role.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `dispatch_package_source_epel` | false | Install qpid-dispatch from epel source. |
| `dispatch_package_pip` | false | Install pip for qdrouter-jinja2 generator.. |
| `dispatch_template_generator` | false | Install qdrouter-jinja2 template generator on router nodes. |
| `dispatch_dispatch_generate_congfigs` | false | Generate config files for oruter nodes from templates. |


Role Variables: Daemon Variables.
--------------

Variables controlling router daemon parameters.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `dispatch_daemon` | `qdrouterd` | Service name |
| `dispatch_config_path` | `/etc/qpid-dispatch/qdrouterd.conf` | Path to dispatch config file |
| `dispatch_default_port` | 5672 | Default port |
| `router_daemon_implementation` | qpid-dispatch | Implementation name for path to config file |


Role Variables: Template Variables
--------------

Variables controlling the creation of a broker instance.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `template_generator` | qdrouter-jinja2 | Template generator package name |
| `template_dir` | '/tmp/templates' | Path to dir for templates |
| `template_file_path_remote` | /tmp/templates/{{ template_file }} | Path to template on remote machine |
| `template_file_path_local` | /tmp/templates/{{ template_file }} | Path to template on local machine |
| `template_file` | "qdrouterd-{{ inventory_hostname }}.conf.j2" | Template name |
| `config_file_path` | '/etc/{{ router_daemon_implementation }}/{{ config_file }}' | Path to config file on router-node |
| `config_file` | 'qdrouterd.conf' | Name of config file for router implementation |
| `generated_router_vars` | '/tmp/router_vars.yml' | Variables generated by topology generator |

Dependencies
------------

A `dispatch_package_source_epel` must be set to `true` when you don't have subscription for dispatch repository.
A `dispatch_package_pip` must be set to `true` when you don't have installed pip and you want to install and use [qdrouter-jinja2](https://github.com/rh-messaging-qe/qdrouterd-jinja2) generator..

Inventory and user's variables
------------

You can use two types of config files for generate final config:

- Generated config file (example: `test/vars/router_vars.yml`) is file, gnerated by [iqa-topology-generator](https://github.com/rh-messaging-qe/iqa-topology-generator). This file includes all components generated from network (graph) type as defaults and also all components from graph specification for iqa-topology-generator

- Hostfile in inventory folder (example: `test/inventory/hosts.yml`) contains structure with router nodes and their specific variables for each host or group of hosts. You can more specify router configs by this file, but every record will overwrite generated components of same type (connectors from host.yml will erase conectors from router_vars.yml).

Example of host file:

	- all:
	    hosts:
	      router_name:
	         variables:
	            machine: router_name
	            router:
	             - id: router_name
	               mode: standalone
	            connector:
	             - host: 6.6.6.6
	               port: 4567

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: docker_containers
      vars:
        dispatch_package_source_epel: true
        dispatch_package_pip: true
        dispatch_template_generator: true
        dispatch_test: true
        dispatch_generate_congfigs: true
        router_vars: files/vars/routers_vars.yml
      roles:
        - role: ansible-qpid-dispatch


Testing
----------------

To test this role we use [provision_docker](https://github.com/chrismeyersfsu/provision_docker) role. This role can be tested using the following command:

```make test```

License
-------

Apache 2.0

Author Information
------------------

Messaging QE team @ redhat.com
