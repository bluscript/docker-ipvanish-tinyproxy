# Docker OpenVPN client for IPVanish
A simple VPN client based on Alpine Linux (forked from [rundqvist/ipvanish-tinyproxy](https://hub.docker.com/r/rundqvist/ipvanish-tinyproxy)).

Find it on Docker Hub: https://hub.docker.com/r/bluscript/ipvanish-tinyproxy

## Features
* Connects to random server (supporting country preference)
* Reconnects if connection breaks down
* HTTP(S) proxy on port 8888
* Killswitch: container stops if OpenVPN connection cannot be established)
* Supports Docker health check

## Components
* Alpine Linux
* OpenVPN
* Tinyproxy

## Configuration
| Variable | Usage |
|----------|-------|
| USERNAME | Your IPVanish username |
| PASSWORD | Your IPVanish password |
| COUNTRY | ISO 3166-1 alpha-2 country code as supported by IPVanish (cf. https://www.ipvanish.com/software/configs) |
| PNET | Add local network like '192.168.0.0' to make container network accessible |
| RANDOMIZE | If true, connects to random server at connect |
| PRIO_REMOTE | Sets specified remote as first connection attempt (does not work with RANDOMIZE=true) |

## Run
```
$ sudo docker run \
    -d \
    --cap-add=NET_ADMIN \
    --device=/dev/net/tun \
    --name=IPVanish \
    --dns 84.200.69.80 \
    --dns 84.200.70.40 \
    -p 8888:8888 \
    -e 'USERNAME=[username]' \
    -e 'PASSWORD=[password]' \
    -e 'COUNTRY=[country code]' \
    -e 'PNET=[local network]' \
    -e 'RANDOMIZE=[true/false]' \
    -e 'PRIO_REMOTE=[first remote to connect to]' \
    bluscript/ipvanish-tinyproxy
```

## Use
Proxy your traffic through [docker server ip]:8888 or use --net container:IPVanish on containers who shall tunnel traffic.
If using --net option, remember to configure PNET or network will not be reachable. Also, the ports you want to reach in the other container must be configured in the vpn container.

## Issues
Please report issues at https://github.com/bluscript/docker-ipvanish-tinyproxy/issues