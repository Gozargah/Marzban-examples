# Fully Single Port
This setup runs both the application and proxy on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html). this is inspired by [All-in-One-fallbacks-Nginx](https://github.com/XTLS/Xray-examples/tree/main/All-in-One-fallbacks-Nginx)

## Description
‌By using this configuration, Xray serves the entire application on port `8443`.
To change the port, you have to edit it from `docker-compose.yml` and `xray_config.json`.

Configured protocols on port 4843:
- Trojan-TCP
- Trojan-Websocket
- VLESS-Websocket
- Vmess-Websocket

Dashboard will be on `http://{YOUR_SERVER_IP}:8443/dashboard`

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

### TLS
\* **Note that by enabling TLS in this setup, dashboard will serve on https too**, so dashboard URL will be on `https://{YOUR_DOMAIN}:8443/dashboard`

First, you must have generated your certificate files. to do this, you can use [certbot]

apt-get install certbot -y
certbot certonly --standalone --agree-tos --register-unsafely-without-email -d api.yourdomain.com

after you got your certificate we have to copy it to somewhere elese and edit the group and chmod

cp /etc/letsencrypt/live/api.yourdomain.com/fullchain.pem /etc/ssl/private/fullchain.pem
cp /etc/letsencrypt/live/api.yourdomain.com/privkey.pem /etc/ssl/private/privkey.pem
------------
chmod --preserve-root 644 /etc/ssl/private/fullchain.pem
chmod --preserve-docker 644 xray_config.json
chown nobody:nogroup /etc/ssl/private/



Eventually, edit `xray_config.json` file and uncomment the TLS section inside streamSettings of fallback inbound and fill `SERVER_NAME` with your ones.


| Variable       | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| SERVER_NAME    | Domain name ( e.g. `example.com` )                                               |
      |


