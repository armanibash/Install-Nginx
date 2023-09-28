# نصب انجینکس بر روی سرور VPS ( ابونتو )  
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
ufw allow 8585
```

# مرحله سوم - نصب وب سرویس Nginx
```bash
apt install nginx -y
```

این مرحله نیاز به دامنه که در ابتدای مطلب گفتم داریم .

در کلادفلر خودتون یک رکورد A  ایجاد کنید و آیپی سرور را در آنجا قرار دهید . آدرس رکورد را کپی کنید . مثال `Armanbash.online`


با دستور بعد آدرس دامنه خود را در تنظیمات وارد میکنیم 
```bash
nano /etc/nginx/sites-available/default
```
حالا یک خط وجود داره که نوشته `server_name _;` بجای `_` دامنه خودتون را وارد کنید . مثال بالا رو اینجوری تغییر میدیم .
<pre>server_name Armanbash.online;</pre>
حالا کلید `ctrl+x` بزنید بعد `y` و `ENTER` تا تغییرات ذخیره شود .

راه اندازی مجدد Nginx 
```bash
systemctl restart nginx
```
نصب core snap 
```bash
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
