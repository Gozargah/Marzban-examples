[English](#en) / [فارسی](#fa)

# [Multi Port](#en)
This setup runs application and proxies on different ports

## Description
| PORT | Description |
| ---- | ----------- |
| 8443 | Application |
| 2083 | Trojan      |
| 2087 | VLESS       |



Dashboard will be on `http://{YOUR_SERVER_IP}:8443/dashboard`

> these are default, you can change any of ports above from `xray_config.json` file.

## Configuration
You can set configuration variables in `env` file.

Do NOT forget to edit `SUDO_USERNAME` & `SUDO_PASSWORD` in production

## Instruction
Install docker on your machine
```bash
curl -fsSL https://get.docker.com | sh
```
Download the files in a directory called *marzban* by following command
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/multi-port.-grpc.tar.gz | tar xz --xform 's/multi-port-grpc/marzban/' && cd marzban
```
Now you're in the directory, run the following command to run the application using docker
```bash
docker compose up -d
```

### How to enable TLS?

You can use either [acme.sh](https://github.com/acmesh-official/acme.sh) or [certbot](https://github.com/certbot/certbot) to generate your certificate files.

In the following you will see how to get this done using acme.sh

First step, you have to install `socat` and `cron` which is required by acme. you can do this by command below if you're on an debian based OS such ubuntu
```bash
sudo apt install -y socat cron
```

Next step, run these commands and fill `YOUR_EMAIL` and `YOUR_DOMAIN` with yours
```bash
curl https://get.acme.sh | sh -s email=YOUR_EMAIL
mkdir -p /var/lib/marzban/certs/
~/.acme.sh/acme.sh --issue --standalone -d YOUR_DOMAIN \
--key-file /var/lib/marzban/certs/key.pem \
--fullchain-file /var/lib/marzban/certs/fullchain.pem
```

Eventually, go edit `xray_config.json` file and find the commented part of tls settings and uncomment it by removing all `//` at start of lines. and do NOT forget to change the `SERVER_NAME` with your domain.


To have dashboard on https too, you must set `UVICORN_SSL_CERTFILE` and `UVICORN_SSL_KEYFILE` in your `env` file
```bash
UVICORN_SSL_CERTFILE = "/var/lib/marzban/certs/fullchain.pem"
UVICORN_SSL_KEYFILE = "/var/lib/marzban/certs/key.pem"
```


Now just restart the marzban with following commands
```bash
cd marzban
docker compose down
docker compose up -d
```

Yay! you've done all needed.
