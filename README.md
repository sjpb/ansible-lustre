# ansible-lustre

This is both an ansible-galaxy Collection (although not on Galaxy, yet), and a demo repo. It currently provides lustre client and server roles, monitoring for both, and related utilities.

The intention is to eventually port the advanced features initially tested [here](https://github.com/stackhpc/ansible-lustre/tree/vss) (lnet routes, filesets, nodemaps, encryption etc) in a more-portable and robust way.

# Use As A Demo

## Installation

    git clone <this repo>
    cd <this repo>
    python3 -m venv venv
    . venv/bin/activate
    pip install -U pip
    pip install -U setuptools
    pip install -r requirements.txt
    #ansible-galaxy install -r requirements.yml

## Use and structure

When used as a demo this repo assumes that the required infrastructure already exists, ie. it does not contain any terraform etc to deploy it. However it is possible to run the same demo on different infrastructures, each of which should have an inventory defined in `environment/<environment-name>/`. The current demo environments are:
- `um6p-dac-h24c5`: For UM6P, uses 2x DAC nodes as servers and 2x physical OpenHPC compute nodes in rack h24c5 as clients.

To make the source of variables clearer, role-specific hostvars are defined in the inventory by hostvar files of the same name as the role, e.g. `hostvars/server0/foo.yml` would set parameters for role `foo` for `server0`.

As well as the inventory, the environment directory also contains an `ansible.cfg` file which configures the inventory path and role search path. The pattern for running a playbook is therefore:

    ANSIBLE_CONFIG=environments/<environment-name>/ansible.cfg ansible-playbook playbooks/<playbook_name>.yml

The following playbooks are provided:

    `monitoring.yml`: Build a Prometheus exporter for lustre metrics on the ansible control host
    `servers.yml`: Install and configure Lustre MGS/MDS/OSS servers and monitoring.
    `clients.yml`: Install Lustre clients and monitoring.

To run the entire suite on the `um6p-dac-h24c5` environment, use:

    export ANSIBLE_CONFIG=environments/um6p-dac-h24c5/ansible.cfg
    ansible-playbook playbooks/monitoring.yml
    ansible-playbook playbooks/servers.yml
    ansible-playbook playbooks/clients.yml
    
## Inventory requirements
TODO: specify here.

# Assumptions

Working DNS?

# Roles

- `server` ([README](roles/server/README.md)): Setup lustre MGS/MDS/OST server (all components are optional). This will change the running kernel, if necessary.
- `client` ([README](roles/client/README.md)): Setup lustre client.
- `lustre_exporter` ([README](roles/lustre_exporter/README.md)): Build and install a Prometheus exporter service for lustre metrics.
- `loopdev`: Used by `playbooks/servers.yml` to create a loop device for the MDT.

Note that:
- Server/client roles currently only support certain Centos versions - see READMEs.
- Only the default lnet configuration using the `tcp` lnet is currently supported.
