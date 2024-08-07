# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Чибриков Станислав`

---

### Задание 1. Установите Zabbix Server с веб-интерфейсом

Согласно заданию выбрана последняя версия Zabbix 6.4

1. Установка базы данных PostgreSQL

sudo apt install postgresql


2. Установка и распаковка репозитория

wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb

dpkg -i zabbix-release_6.4-1+debian11_all.deb

apt update

3. Установка заббикс сервера, веб-интерфейса

apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts


4. Создание базы данных и пользователя

sudo -u postgres createuser --pwprompt zabbix

sudo -u postgres createdb -O zabbix zabbix


5. Импорт данных на сервер Zabbix

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix


6. Настройка пароля бызы данных

nano /etc/zabbix/zabbix_server.conf далее в файле напротив DBPassword= указываем наш пароль


7. Рестарт и добавление в автозагрузку веб сервера и Заббикс сервера

systemctl restart zabbix-server apache2

systemctl enable zabbix-server apache2

Скриншот авторизации в админке

![alt text](https://github.com/Chibrik0ff/8-03-hw/blob/main/screen2.1.png)
---

### Задание 2. Установка Zabbix Agent на два хоста

В качестве одного из хостов использована машина с Zabbix серверо, в качестве второго - чистая ВМ.

1. Установка заббикс агента

sudo apt install zabbix-agent


2. Рестарт и добавление в автозагрузку Заббикс агента

systemctl restart zabbix-agent

systemctl enable zabbix-agent


3. Настраиваем параметры подключения заббикс агента к серверу (прописываем адреса хостов)

sed -i 's/Server=127.0.0.1/Server=192.168.1.36/g' /etc/zabbix/zabbix_agent.conf

sed -i 's/Server=127.0.0.1/Server=192.168.1.33/g' /etc/zabbix/zabbix_agent.con

Скриншот раздела configuration

![alt text](https://github.com/Chibrik0ff/8-03-hw/blob/main/screen2.3.png)

Cкриншот лога zabbix agent, где видно, что он работает с сервером

![alt text](https://github.com/Chibrik0ff/8-03-hw/blob/main/screen2.4.png)

Cкриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.

![alt text](https://github.com/Chibrik0ff/8-03-hw/blob/main/screen2.2.png)

![alt text](https://github.com/Chibrik0ff/8-03-hw/blob/main/screen2.5.png)


