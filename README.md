# نصب انجینکس بر روی سرور VPS ( ابونتو ) برای جلوگیری از فیلترشدن سرور
https://youtu.be/QI8FH5kWRe8

 <a href="https://t.me/armanibashivpn" target="_blank"> Telegram </a>
 
فرض بر این گرفته شده که شما یک سرور vps برای vpn دارید و میخوایید از فیلترشدن اون جلوگیری کنید . 

حالا این VPN SERVER از هر نوعش میتونه باشه ( پروکسی تلگرام ، سرور V2ray یا ... )

بجز سرور به سایت <a href="https://cloudfale.com" target="_blank"> کلادفلر </a>و یه دامنه که در کلادفلر ثبت شده باشه 

# مرحله اول - آپدیت سرور

```bash
apt update && apt upgrade -y
```

# مرحله دوم - تنظیم فایروال
اگر خواستید این مرحله رو انجام بدید که من پیشنهادم انجام دادنش هست با این کار فقط پورت های که خودتون باز میکنید میتونن به سرور وصل بشن . پس یادتون باشه اگر پورت جدیدی لازم داشتید با دستور `ufw allow port` میتونید پورتهای دیگه رو باز کنید.

پورتهای ضروری را در فایروال باز کنید :

```bash
ufw allow ssh
ufw allow http
ufw allow https
```

میخوام یه پورت دیگه رو برای نمونه باز کنم . پورت مثال 8585
```bash
ufw allow 8585/tcp
ufw allow 8585/udp
```
فعال کردن فایروال

```bash
ufw enable
```
ازتون سوال میپرسه که فعال بشه . پس `y` بزنید و `ENTER`

چک کردن وضعیت فایروال
```bash
ufw status
```
مثل این وضعیت بهتون نمایش میده

<pre>
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443                        ALLOW       Anywhere
8585/tcp                   ALLOW       Anywhere
8585/udp                   ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
8585/tcp (v6)              ALLOW       Anywhere (v6)
8585/udp (v6)              ALLOW       Anywhere (v6)
</pre>
# مرحله سوم - نصب وب سرویس Nginx
```bash
apt install nginx -y
```

نصب core snap 
```bash
apt install snapd
snap install core
```
نصب classic certbot
```bash
snap install --classic certbot
```
انتقال فایل های certbot دریافت شده
```bash
ln -s /snap/bin/certbot /usr/bin/certbot
```
فراخوانی certbot برای دریافت گواهی SSL دامنه وارد شده در nginx 
```bash
certbot --nginx
```
ازتون میخواد یه `Email` وارد کنید و `ENTER` بزنید.

دوتا سوال ازتون میپرسه `y` بزنید.

سوال سوم

این مرحله نیاز به دامنه که در ابتدای مطلب گفتم داریم .

در کلادفلر خودتون یک رکورد A  ایجاد کنید و آیپی سرور را در آنجا قرار دهید . آدرس رکورد را کپی کنید . مثال `Armanbash.online`

دامنه `(DAMIN)` یا ساب دامین وارد کنید و `ENTER`

تست کنید تا مطمعن شوید مراجل را کامل و صحیح انجام داده اید . 

آدرس دامنه خودتون رو توی مروگر باز کنید `https://your_domain` اگر پیام خوش آمدگویی Welcome to nginx را دیدید کار تمام شده و همه چی درست انجام شده 




# دستورات کنترل Nginx

متوقف کردن انجنیکس
```bash
systemctl stop nginx
```
استارت انجنیکس
```bash
systemctl start nginx
```
ریستارت انجنیکس
```bash
systemctl restart nginx
```
اگر دامین تغییر کرد آدرس در فایل زیر است
```
nano /etc/nginx/sites-available/default
```
