# Multipass

Using multipass to provide independent pre-configured run-time environments.

## Environments

- [nomad](./nomad/README.md)

## Hints

### Cloud Init

- _"Currently multipass only supports YAML cloud-config files."_
- _"If interested, a user can examine the executed user-data and vendor-data by looking at the files in /var/lib/cloud/instances/"_

## References

- [multipass](https://multipass.run)
- [cloud-init](https://ubuntu.com/blog/using-cloud-init-with-multipass)
  - [examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html)
- [ECS Anywhere](https://ubuntu.com/blog/how-to-run-ecs-anywhere-workloads-using-ubuntu)
