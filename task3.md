# Домашнее задание 3

# Реализация скрипта по добавлению строки из 20 случайных символов в файл
```
#!/usr/bin/env bash

mkdir /opt/app

while true; do
        pass=$(openssl rand -base64 15 | tr -cd 'a-zA-Z0-9@%_+' | head -c 20)
        echo "$pass" >> /opt/app/log.txt
        sleep 17
done
```

#Реализация systemd-юнита для автозапуска скрипта как службы при старте системы 
```
[Unit]
Description=Добавляем скрипт в автозапуск, путем превращения его в службу
After=network.target

[Service]
Type=simple
ExecStart=/home/superuser/tasks/sdv
WorkingDirectory=/home/superuser/tasks
Restart=on-failure
User=root

[Install]
WantedBy=multi-user.target
```

#Реализация ротации скрипта через logrotate
```
#
/opt/app/log.txt {
        daily
        rotate 7
        missingok
        notifempty
        create 644 root root

}
```