# Домашние задания по модулю «Мониторинг»

В этом репозитории расположены ваши домашние задания к каждой лекции. 

Обязательны к выполнению задачи без звездочек. Их нужно выполнить, чтобы получить зачёт.

Задачи со звёздочкой (*) — дополнительные задачи или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Любые вопросы по решению задач задавайте в чате учебной группы. Ссылку вы найдёте в письме на вашей электронной почте.



2. [Система мониторинга Zabbix](hw-02.md)
# Система мониторинга Zabbix
## Рыльцева Виктория Михайловна

---

### Задание 1. Установка Zabbix Server с веб-интерфейсом

#### Текст использованных команд:

```
# 1. Установка PostgreSQL
sudo apt update
sudo apt install -y postgresql

# 2. Создание пользователя и базы данных
sudo -u postgres psql -c "create user zabbix with password 'zabbix';"
sudo -u postgres psql -c "create database zabbix owner zabbix;"

# 3. Добавление репозитория Zabbix 6.0
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
sudo dpkg -i zabbix-release_6.0-4+debian11_all.deb
sudo apt update

# 4. Установка Zabbix сервера и веб-интерфейса
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# 5. Импорт схемы базы данных
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# 6. Настройка пароля в конфиге
sudo sed -i 's/# DBPassword=/DBPassword=zabbix/' /etc/zabbix/zabbix_server.conf

# 7. Настройка часового пояса
echo "php_value date.timezone Europe/Moscow" | sudo tee -a /etc/zabbix/apache.conf

# 8. Запуск сервисов
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2



#### Скриншот авторизации в админке:
<img src="https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_130703.png" width="600">

Задание 2. Установка Zabbix Agent на два хоста


Текст использованных команд:


На первом хосте (Zabbix Server):


sudo apt update
sudo apt install -y zabbix-agent
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent
На втором хосте (Ubuntu-Agent):


# Добавление репозитория Zabbix
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
sudo apt update

# Установка агента
sudo apt install -y zabbix-agent

# Настройка подключения к серверу
sudo sed -i 's/Server=127.0.0.1/Server=192.168.1.44/' /etc/zabbix/zabbix_agentd.conf
sudo sed -i 's/ServerActive=127.0.0.1/ServerActive=192.168.1.44/' /etc/zabbix/zabbix_agentd.conf
sudo sed -i 's/Hostname=Zabbix server/Hostname=Ubuntu-Agent/' /etc/zabbix/zabbix_agentd.conf

# Запуск агента
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent


Скриншот Configuration → Hosts:

https://github.com/user-attachments/assets/a1a0319a-3b7d-4a48-9140-d4552a08b695


Скриншот лога zabbix agent:

https://github.com/user-attachments/assets/da5e2d2b-07eb-43cb-8070-19ccfb6c1855

Скриншот раздела Monitoring > Latest data:

https://github.com/user-attachments/assets/78ff0f2a-f07a-4a6f-b1c7-af4534db0387
https://github.com/user-attachments/assets/e5fbad85-0c8b-473e-aef5-7d217abbacf2





3. [Система мониторинга Zabbix. Часть 2](hw-03.md)

