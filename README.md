# ansible-lustre

# Assumptions

- have groups `servers` and `mgs` which is single-node subset of `servers`


# Roles

`server` variables:
- `lustre_release`: "2.12.5"
- `lustre_kernel`: "3.10.0-1127.19.1.el7.x86_64"
- `lustre_reformat`: false

NB the release and kernel must match the Centos version, here 7.8.

Must also provide:
- `lustre_lnet`: tcp
- `lustre_fs_name`: "dac"
- `lustre_mgs_addr`: "{{ hostvars[groups['mgs'] | first].ansible_p8p2.ipv4.address }}" # TODO break out interface as variable??
- `lustre_mgs_lnet`: "{{ hostvars[groups['mgs'] | first].lustre_lnet }}"
- `mgt`: /dev/nvme0n1 (on MGS only)
- `mdts`: ['/dev/nvme1n1', ...]
- `osts`: ['/dev/nvme2n1', ...]
