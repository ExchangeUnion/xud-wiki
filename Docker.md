This page contains `xud` setup instructions for exchange admins and technical users utilizing Docker.

We use `docker-compose` to package `xud`, `lnd`, `btcd`, `ltcd`, `raiden` & `geth` together to make deploying these services as easy as typing a few commands. All configuration between `xud` and other containers are handled automatically by their `docker-compose` config file.

The goal of this is `docker-compose` being able to lauch `xud` along with its dependencies.

## 1. Prerequisites

* docker
* docker-compose

To install these on e.g. Ubuntu, check the [official docker install instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

## 2. Install

```bash
git clone https://github.com/ExchangeUnion/xud
```

## 3. Run

Start Containers:

```bash
cd ~/xud/docker
docker-compose up -d
```

The above command should create and launch both `xud` and other required containers as per `docker-compose.yml`. It will take a moment until everything is downloaded for the first time.

Stop Containers:

```bash
docker-compose stop
```

Remove Containers:

```bash
docker-compose down
```

Check `xud` | `mysql` logs:

```bash
docker-compose logs <xud|db>
```

To change ports & hosts and for docker-specific questions, please refer to the official documentation in [Docker Container Networking](https://docs.docker.com/config/containers/container-networking/).