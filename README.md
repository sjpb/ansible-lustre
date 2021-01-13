# ansible-lustre

Provides the following main roles:

- `server`: Setup lustre MGS/MDS/OST server (currently only MGS is optional). This will change the running kernel, if necessary.
- `client`: Setup lustre client.
- `monitoring`: Setup monitoring - WIP: currently just installs/starts the lustre exporter.

The server/client roles only support certain Centos versions (see the vars files).

There are example plays for these in `.yml` files of the same name.

Currently only the default lnet configuration using the `tcp` lnet is supported.

It also provies a `loopdev` role which the server play uses to create a loop device for the MDT.

# Assumptions

Working DNS?

# Roles

See above. For variables currently see the appropriate `role/<name>/defaults/main.yml` file.
