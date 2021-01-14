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

When used as a demo this repo assumes that the required infrastructure already exists, ie. it does not contain any terraform etc to deploy it. However it is possible to run the same demo on different infrastructures each, of which should have an inventory defined in `environment/<environment-name>/`. The current demo environments are:
- `um6p-dac-h24c5`: For UM6P, uses 2x DAC nodes as servers and 2x physical OpenHPC compute nodes in rack h24c5 as clients.

To make the source of variables clearer, role-specific hostvars are defined in the inventory by hostvar files of the same name as the role, e.g. `hostvars/server0/foo.yml` would set parameters for role `foo` for `server0`.

As well as the inventory, the environment directory also contains an `ansible.cfg` file which configures the inventory path and role search path. The pattern for running a playbook is therefore:

    ANSIBLE_CONFIG=environments/<environment-name>/ansible.cfg ansible-playbook playbooks/<playbook_name>.yml


To run the entire suite on the `um6p-dac-h24c5` environment, use:

    export ANSIBLE_CONFIG=environments/um6p-dac-h24c5/ansible.cfg
    ansible-playbook playbooks/server.yml
    ansible-playbook playbooks/client.yml
    ansible-playbook playbooks/monitoring.yml # UNDER DEV - currently only partially configures servers!


## Inventory requirements
TODO: specify here.

# Assumptions

Working DNS?

# Roles

- `server`: Setup lustre MGS/MDS/OST server (currently only MGS is optional). This will change the running kernel, if necessary.
- `client`: Setup lustre client.
- `monitoring`: Setup monitoring - WIP: currently just installs/starts the lustre exporter.
- `loopdev`: Used by `playbooks/server.yml` to create a loop device for the MDT.

The server/client roles only support certain Centos versions (see the vars files).

Note that currently only the default lnet configuration using the `tcp` lnet is supported.

For variables currently see the appropriate `role/<name>/defaults/main.yml` file.

# Lustre hints

To reset lnet (defined in /etc/lnet.conf):

    (umount -t lustre --all)
    lctl network down
    lustre_rmmod
    modprobe lnet -v
    lctl network up
