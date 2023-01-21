# Multi Port
This setup runs all proxy protocols on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html) and application on a separate port

## Description
By default, application runs on port 8880 and proxies on port 8443.

To change the port, you have to edit it from `docker-compose.yml` and `xray_config.json`.

Configured protocols on port 8443:
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
Install docker on your machine
```bash
curl -fsSL https://get.docker.com | sh
```
Download the files in a directory called *marzban* by following command
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/single-port-proxy.tar.gz | tar xz --xform 's/single-port-proxy/marzban/' && cd marzban
```
Now you're in the directory, run the following command to run the application using docker
```bash
docker compose up -d
```

### TLS
To enable TLS, you must have generated your certificate files. you can use tools such [certbot](https://github.com/certbot/certbot) or [acme.sh](https://github.com/acmesh-official/acme.sh).

Then you need to mount a volume to the path of certification files in `docker-compose.yml` in order to access it inside docker container.

such this for certbot:
```yaml
volumes:
  - /etc/letsencrypt/:/etc/letsencrypt
```

Eventually, edit `xray_config.json` file and uncomment the TLS section inside streamSettings of fallback inbound and fill `SERVER_NAME`, `CERT_FILE_PATH` and `KEY_FILE_PATH` with your ones.


| Variable       | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| SERVER_NAME    | Domain name ( e.g. `example.com` )                                               |
| CERT_FILE_PATH | Certificate file path ( e.g. `/etc/letsencrypt/live/example.com/fullchain.pem` ) |
| KEY_FILE_PATH  | Key file path ( e.g. `/etc/letsencrypt/live/example.com/privkey.pem` )           |


