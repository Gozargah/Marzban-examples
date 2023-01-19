# Fully Single Port
This setup runs both the application and proxy on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html). this is inspired by [All-in-One-fallbacks-Nginx](https://github.com/XTLS/Xray-examples/tree/main/All-in-One-fallbacks-Nginx)

## Description
‌By using this configuration, Xray serves the entire application on port `443`.
You can change the port from `docker-compose.yml` ( OPTIONAL: also `xray_config.json` if you wish ).

Configured protocols on port 443:
- Trojan-TCP
- Trojan-Websocket
- VLESS-Websocket
- Vmess-Websocket
- Trojan-gRPC
- VLESS-gRPC
- Vmess-gRPC

Dashboard will be on `http://{YOUR_SERVER_IP}:443/dashboard`

## Configuration
You can set configuration variables in `env` file.

Do NOT forget to edit `SUDO_USERNAME` & `SUDO_PASSWORD` in production

## Instruction
Download the files in a directory called *marzban* by following command
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/fully-single-port.tar.gz | tar xz --xform 's/fully-single-port/marzban/' && cd marzban
```
Now you're in the directory, run the following command to run the application using docker
```bash
docker compose up -d
```

### TLS
\* **Note that by enabling TLS in this setup, dashboard will serve on https too**, so dashboard URL will be on `https://{YOUR_DOMAIN}:443/dashboard`

First, you must have generated your certificate files. to do this, you can use tools such [certbot](https://github.com/certbot/certbot) or [acme.sh](https://github.com/acmesh-official/acme.sh).

Eventually, edit `xray_config.json` file and uncomment the TLS section inside streamSettings of fallback inbound and fill `SERVER_NAME`, `CERT_FILE_PATH` and `KEY_FILE_PATH` with your ones.


| Variable       | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| SERVER_NAME    | Domain name ( e.g. `example.com` )                                               |
| CERT_FILE_PATH | Certificate file path ( e.g. `/etc/letsencrypt/live/example.com/fullchain.pem` ) |
| KEY_FILE_PATH  | Key file path ( e.g. `/etc/letsencrypt/live/example.com/privkey.pem` )           |


