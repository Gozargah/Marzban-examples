# Multi Port
This setup runs application and proxies on different ports

## Description
| PORT | Description |
| ---- | ----------- |
| 8880 | Application |
| 2053 | VMess       |
| 2083 | VLESS       |
| 2087 | Trojan      |
| 2096 | Shadowsocks |

You can change any of these ports in `docker-compose.yml` ( OPTIONAL: also `xray_config.json` if you wish ).

Dashboard will be on `http://{YOUR_SERVER_IP}:8880/dashboard`

## Configuration
You can set configuration variables in `env` file.

Do NOT forget to edit `SUDO_USERNAME` & `SUDO_PASSWORD` in production

## Instruction
Download the files in a directory called *marzban* by following command
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/multi-port.tar.gz | tar xz --xform 's/multi-port/marzban/' && cd marzban
```
Now you're in the directory, run the following command to run the application using docker
```bash
docker compose up -d
```

### TLS
To enable TLS, you must have generated your certificate files. you can use tools such [certbot](https://github.com/certbot/certbot) or [acme.sh](https://github.com/acmesh-official/acme.sh).

Eventually you have to edit `xray_config.json` and add TLS configuration inside streamSettings just like the commented ones and fill `SERVER_NAME`, `CERT_FILE_PATH` and `KEY_FILE_PATH`.


| Variable       | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| SERVER_NAME    | Domain name ( e.g. `example.com` )                                               |
| CERT_FILE_PATH | Certificate file path ( e.g. `/etc/letsencrypt/live/example.com/fullchain.pem` ) |
| KEY_FILE_PATH  | Key file path ( e.g. `/etc/letsencrypt/live/example.com/privkey.pem` )           |


