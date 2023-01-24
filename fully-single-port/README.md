# Fully Single Port
This setup runs both the application and proxy on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html). this is inspired by [All-in-One-fallbacks-Nginx](https://github.com/XTLS/Xray-examples/tree/main/All-in-One-fallbacks-Nginx)

## Description
‌By using this configuration, Xray serves the entire application on port `443`.

Configured protocols on port 443:
- Trojan-TCP
- Trojan-Websocket
- VLESS-Websocket
- Vmess-Websocket

Dashboard will be on `http://{YOUR_SERVER_IP}:443/dashboard`

> this is the default port, you can change it from `xray_config.json` file.

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
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/fully-single-port.tar.gz | tar xz --xform 's/fully-single-port/marzban/' && cd marzban
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


> In this setup particularly, dashboard will be served on https if you have enabled TLS on proxies. so notice that your dashboard also will be on `https://{YOUR_DOMAIN}/dashboard` with all you did above.

Now just restart the marzban with following commands
```bash
cd marzban
docker compose down
docker compose up -d
```

Yay! you've done all needed.