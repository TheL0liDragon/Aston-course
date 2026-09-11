
# Домашнее задание 4


# Создание самоподписанного сертификата

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/app.local.key \
  -out /etc/ssl/certs/app.local.crt \
  -subj "/CN=app.local"
```

**Созданные файлы:**
- `/etc/ssl/private/app.local.key` — приватный ключ
- `/etc/ssl/certs/app.local.crt` — сертификат


# Конфиг Nginx для HTTPS

Файл `/etc/nginx/sites-available/app.local`:

```
server {
    listen 80;
    server_name app.local;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name app.local;

    ssl_certificate /etc/ssl/certs/app.local.crt;
    ssl_certificate_key /etc/ssl/private/app.local.key;

    root /var/www/html;
    index index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

**Активация:**
```
sudo ln -s /etc/nginx/sites-available/app.local /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

# Код скрипта для проверки доступности ресурса

```
#!/usr/bin/env bash
echo "Добро пожаловать в систему проверки доступности сетевого ресурса!"
read -p "Введите HTTP(S) адресс для проверки доступности:" adress

ponging="${adress#http://}"
ponging="${ponging#https://}"
ponging="${ponging%%/*}"
echo -e "Результат пинга ресурса: \n"
ping -c 1 "$ponging"
ping_exit=$?

echo
echo -e "Код ответа ресурса: \n"
curl -sk -o /dev/null -w "%{http_code}\n" "$adress"
code=$(curl -sk -o /dev/null -w "%{http_code}" "$adress")

if [ "$ping_exit" -eq 0 ] && [ "$code" -ge 200 ] && [ "$code" -lt 400 ]; then
        echo "Установлено соединение с ресурсом"
        exit 0
elif [ "$code" -ge 400 ] && [ "$code" -lt 500 ]; then
        echo "Ошибка на стороне клиента"
        exit 1
elif [ "$code" -ge 500 ] && [ "$code" -lt 600 ]; then
        echo "Ошибка на стороне сервера"
        exit 1
else
        echo "Соединение не установлено"
        exit 1
fi
```