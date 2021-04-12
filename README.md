# riotfp-server

## Purpose

This repository holds some basic configuration to setup a server on a cloud
provider to run as the `riot-fp` demonstrator server.

## Ansible

See [ansible](ansible/README.md). Deploy at least the common role to have
a basic server setup with no ssl.

## Docker

### Deploy [pyaiot](https://github.com/pyaiot/pyaiot) dashboard/broker

The best way is with [docker-machine](https://docs.docker.com/machine/):

- add a [docker-machine](https://docs.docker.com/machine/)

```shell
$ docker-machine create --driver generic --generic-ip-address=<server-ip-address> <some-name>
```

- select the machine

```shell
$ eval $(docker-machine env riofp-test)
```

- copy over some predefined pyaiot keys

```shell
$ scp <path-to-keys> admin@<server-ip-address>:/home/admin/.pyaiot/keys
```

- deploy

```shell
$ cd docker
$ docker-compose pull up -d
```

Congrats your riotfp pyaiot dashboard/broker should be up and running,
verify this by accessing <server-ip-address>:80.

#### Deploy behind reverse proxy

Follow `Deploy SSL certificates` in [ansible/README.md](ansible/README.md).

Redeploy the ssl version of the `docker-compose`

- deploy

```shell
$ cd docker/ssl
$ docker-compose pull up -d
```

## Github Actions Docker Server Deployment

We need some certificates to allow CI to connect to the daemon. If you’re
a TLS certificate wizard, you’ll whip out your openssl wand and make secure
dreams come true. If you’re a mere mortal like me, you can use:
https://github.com/XIThing/generate-docker-client-certs/. Clone that repo
and run:

    ./generate-client-certs.sh ~/.docker/machine/certs/ca.pem ~/.docker/machine/certs/ca-key.pem

Setup the secrets used in the deploy action, and voila.
