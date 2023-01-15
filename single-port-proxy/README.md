# Multi Port
This setup runs all proxy protocols on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html) and application on a separate port

## Description
By default, application runs on port 8880 and proxies on port 8443.

You can change them from `docker-compose.yml` ( OPTIONAL: also `xray_config.json` if you wish ).

Configured protocols on port 443:
- Trojan-TCP
- Trojan-Websocket
- VLESS-TCP
- VLESS-Websocket
- Vmess-TCP
- Vmess-Websocket

Dashboard will be on `http://{YOUR_SERVER_IP}:8880/dashboard`

## Configuration
You can set configuration variables in `env` file.

Do NOT forget to edit `SUDO_USERNAME` & `SUDO_PASSWORD` in production

## Instruction
Download the files in a directory called *marzban* by following command
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/download/latest/single-port-proxy.tar.gz | tar xz --xform 's/single-port-proxy/marzban/' && cd marzban
```
Now you're in the directory, run the following command to run the application using docker
```bash
docker compose up -d
```

### TLS
To enable TLS, you must have generated your certificate files. you can use tools such [certbot](https://github.com/certbot/certbot) or [acme.sh](https://github.com/acmesh-official/acme.sh).

Eventually, edit `xray_config.json` file and uncomment the TLS section inside streamSettings of fallback inbound and fill `SERVER_NAME`, `CERT_FILE_PATH` and `KEY_FILE_PATH` with your ones.


| Variable       | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| SERVER_NAME    | Domain name ( e.g. `example.com` )                                               |
| CERT_FILE_PATH | Certificate file path ( e.g. `/etc/letsencrypt/live/example.com/fullchain.pem` ) |
| KEY_FILE_PATH  | Key file path ( e.g. `/etc/letsencrypt/live/example.com/privkey.pem` )           |


