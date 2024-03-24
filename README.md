# Local DNS Server with Docker

This repository contains the necessary configuration to set up a local DNS server using Docker which offers name resolution of a local-only-domain `mich0w0h.house`. The server is composed of two services: a resolver and an authoritative DNS server.

## Services

### Resolver

The resolver service is built from a Dockerfile located in the `resolver` directory. It uses Ubuntu 22.04 as a base image and installs Unbound, a lightweight DNS resolver. The configuration for Unbound is copied from `resolver/unbound.conf` into the container.

### Authoritative

The authoritative service uses the `coredns/coredns` image from Docker Hub. It uses a configuration file located at `authoritative/Corefile` and a zone file located at `authoritative/zone/mich0w0h.house.db`.

## Network

The services are connected through a bridge network named `internal-dns`. The resolver service is assigned the IP address `192.168.1.101` and the authoritative service is assigned the IP address `192.168.1.102`.

## Running the Services

To start the services, use Docker Compose:

```bash
docker-compose -f compose.yml up
```

## Note for Mapping Port 53 in Docker

Before running the services, ensure that port 53 is available on your host machine. On Ubuntu 22.04, `systemd-resolved` runs on port 53 by default. You will need to stop and disable `systemd-resolved`, or change its port before you can use port 53 for your Docker container.

Please note that these steps will change your DNS settings and disable systemd-resolved. Make sure you understand the implications of these changes before proceeding, or have a suitable replacement solution in place.

To stop and disable `systemd-resolved`, run:

```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
```

Then, remove the symlink `/etc/resolv.conf`:

```bash
sudo rm /etc/resolv.conf
```

And create a new `/etc/resolv.conf` with a nameserver of your choice. For example, to use Google's DNS server:

```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

