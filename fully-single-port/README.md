[English](#fully-single-port) / [فارسی](#کاملا-تک-پورت)

# [Fully Single Port](#en)
This setup runs both the application and proxy on a single port using [fallbacks](https://xtls.github.io/config/features/fallback.html). this is inspired by [All-in-One-fallbacks-Nginx](https://github.com/XTLS/Xray-examples/tree/main/All-in-One-fallbacks-Nginx)

## Description
‌By using this configuration, Xray serves the entire application on port `443`.

Configured protocols on port 443:
- Trojan-TCP
- Trojan-Websocket
- VLESS-Websocket
- Vmess-Websocket
- Trojan-gRPC
- VLESS-gRPC
- Vmess-gRPC

Dashboard will be on `http://{YOUR_SERVER_IP}:443/dashboard`

> gRPC inbounds won't work without TLS. check [How to enable TLS?](#how-to-enable-tls)
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


# [کاملا تک پورت](#fa)
این تنظیمات برنامه و پروکسی ها را با استفاده از [fallbacks](https://xtls.github.io/config/features/fallback.html) روی یک پورت اجرا می کند.

## توضیحات
در این تنظیمات، Xray تمام برنامه را روی پورت `۴۴۳` اجرا میکند

پروتکل های تنظیم شده روی پورت ۴۴۳:
- Trojan-TCP
- Trojan-Websocket
- VLESS-Websocket
- Vmess-Websocket
- Trojan-gRPC
- VLESS-gRPC
- Vmess-gRPC

داشبورد بر روی آدرس `http://{YOUR_SERVER_IP}:443/dashboard` در دسترس خواهد بود.

> پروکسی های gRPC بدون تنظیم TLS کار نخواهند کرد. [فعال سازی TLS](#فعال-سازی-tls) را بررسی کنید.

> شما می توانید پورت پیش فرض را از فایل `xray_config.json` تغییر دهید.

## تنظیمات
شما می توانید متغیر های تنظیمات را در فایل `env` تعیین کنید.
لیست متغیر ها را می توانید از [اینجا](https://github.com/Gozargah/Marzban#configuration) مشاهده کنید.

فراموش نکنید که متغیر های `SUDO_USERNAME` و `SUDO_PASSWORD` را تغییر دهید.

## راهنما
با اجرای دستور زیر داکر را نصب کنید
```bash
curl -fsSL https://get.docker.com | sh
```
با دستور زیر فایل ها را در پوشه ای به نام `marzban` دانلود کنید و وارد پوشه شوید
```bash
wget -qO- https://github.com/Gozargah/Marzban-examples/releases/latest/download/fully-single-port.tar.gz | tar xz --xform 's/fully-single-port/marzban/' && cd marzban
```
حالا شما در پوشه هستید، با اجرای دستور زیر برنامه را اجرا کنید
```bash
docker compose up -d
```


### فعال سازی TLS

شما می توانید از [acme.sh](https://github.com/acmesh-official/acme.sh) یا [certbot](https://github.com/certbot/certbot) برای تولید گوهینامه SSL استفاده کنید.

در ادامه، آموزش انجام این کار را با استفاده از acme.sh مشاهده میکنید.

در قدم اول، شما باید پکیج های مورد نیاز acme یعنی `socat` و `cron` را روی ماشین خود نصب کنید. اگر روی ماشین مجازی ای مانند دبیان یا اوبونتو هستید می توانید با دستور زیر این کار را انجام دهید
```bash
sudo apt install -y socat cron
```

در قدم بعد، `YOUR_EMAIL` و `YOUR_DOMAIN` را با ایمیل و دامنه خود جایگزین کنید و دستور را اجرا کنید.
```bash
curl https://get.acme.sh | sh -s email=YOUR_EMAIL
mkdir -p /var/lib/marzban/certs/
~/.acme.sh/acme.sh --issue --standalone -d YOUR_DOMAIN \
--key-file /var/lib/marzban/certs/key.pem \
--fullchain-file /var/lib/marzban/certs/fullchain.pem
```

در آخر، فایل `xray_config.json` باز کنید و بخش های کامنت شده مرتبط با تنظیمات TLS را از حالت کامنت خارج کنید (یعنی تمام ‍`//` های ابتدای خط ها را حذف کنید) و سپس، `SERVER_NAME` را با دامنه خود جایگزین کنید.


> در این تنظیمات به طور استثنا، با تنظیم TLS روی پروکسی، داشبورد هم روی https خواهد بود. پس توجه کنید که با اعمال تغییرات بالا، آدرس داشبورد `https://{YOUR_DOMAIN}/dashboard` می شود.

حالا فقط کافیه با اجرای دستورات زیر سرویس مرزبان را ری استارت کنید
```bash
cd marzban
docker compose down
docker compose up -d
```

عالی! شما همه کار های لازم رو انجام دادید.
