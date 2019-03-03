# nhinds/ubuntu-vagrant-systemd

Ubuntu docker image running systemd/ssh for use with vagrant.

[Docker Hub](https://hub.docker.com/r/nhinds/ubuntu-vagrant-systemd)

## Tags

* `16.04` - Ubuntu xenial

## Usage

### From vagrant

```
  config.vm.provider :docker do |docker|
    docker.image = 'nhinds/ubuntu-vagrant-systemd:16.04'

    docker.create_args = [
      '--privileged', # systemd does cgroup magic and needs to be privileged
      '-v', '/sys/fs/cgroup:/sys/fs/cgroup:ro' # systemd does cgroup magic and needs a read-only view of the host's cgroups
    ]

    docker.has_ssh = true
  end
```

### Manual

This image's systemd likes to run in privileged mode, with the `/sys/fs/cgroup` filesystem mounted in from the host. Run with these flags:

    docker run --rm -d --privileged -v /sys/fs/cgroup:/sys/fs/cgroup:ro nhinds/ubuntu-vagrant-systemd:16.04

Then use `docker exec` to run `bash` or other commands within the container

    docker exec -it <container name> bash

It is not recommended to use this image for non-Vagrant purposes, since it sets up the `vagrant` user with Vagrant's insecure SSH key by default.
