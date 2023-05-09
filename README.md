# IP Proxy Pool Implemented in Golang

> Collect free proxy resources to provide effective IP proxies for crawlers

## System Features

- Automatically crawl free proxy IP on the Internet
- Periodically verify the effectiveness of proxy IP
- Provide HTTP API to obtain available IP

## System Architecture

![architecture image](./docs/images/architecture.png)

## Proxy Pool Design

The proxy pool consists of four parts:
- Fetcher:

Proxy acquisition interface, currently there are several **free proxy sources**, each call will fetch the latest proxies from these websites and put them into the Channel. You can also **add additional proxy acquisition interfaces**.

- Channel:

Temporary storage of collected proxies, use a stable website to verify the validity of the proxy, and store it in the database if it is valid.

- Schedule:

Use scheduled tasks to check the availability of proxy IPs in the database, and delete unavailable proxies. It will also actively get the latest proxies through Fetcher.

- Api:

Access interface of the proxy pool, providing `get` interface output `JSON`, which is convenient for crawlers to use directly.

## Currently Supported Proxies

Proxy acquisition interface, currently fetches **free proxies** from several websites, and of course, supports expanding proxy interfaces on your own;

- [89 Free Proxy](https://www.89ip.cn)
- [66 Free Proxy](http://www.66ip.cn)
- [Cloud Proxy](http://www.ip3366.net)
- [Fast Proxy](http://www.kuaidaili.com)
- [Proxylist+](https://list.proxylistplus.com)

## Installation and Usage

### Source Code Installation

```shell
# Clone the project
git clone https://github.com/wuchunfu/IpProxyPool.git

# Switch to the project directory
cd IpProxyPool

# Modify the database information
vi conf/config.yaml

host: 127.0.0.1
dbName: IpProxyPool
username: IpProxyPool
password: IpProxyPool

# Execute SQL scripts to create database tables
source docs/db/mysql.sql

# Install Go dependencies
go list (go mod tidy)

# Compile
go build IpProxyPool.go

# Grant executable permission
chmod +x IpProxyPool

# Run
./IpProxyPool proxy-pool
```

### Docker Installation

> Please install `Docker` yourself, and after installing `Docker`, check if `docker-compose` is installed
> Run this command to check if `docker-compose` is installed successfully, `docker-compose -version`

```shell
# Clone the project
git clone https://github.com/wuchunfu/IpProxyPool.git

# Enter the project directory
cd IpProxyPool

# Run the following command to start
docker-compose -f docker-compose.yaml up -d

# Run the following command to stop
docker-compose -f docker-compose.yaml down
```

## Access

```shell
# Web access
http://127.0.0.1:3000

# or
# Randomly output available proxies
curl http://127.0.0.1:3000/all

# Randomly output HTTP proxies
curl http://127.0.0.1:3000/http

# Randomly output HTTPS proxies
curl http://127.0.0.1:3000/https
```

## Scheduled Tasks

- [ ] [Xila Free Proxy IP](http://www.xiladaili.com)
- [ ] [Zdaye](https://www.zdaye.com)

## Sincere Thanks

- Firstly, thank you for using it. If you think the program is good and can help you solve practical problems, you can add a
