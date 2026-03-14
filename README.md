# Домашние задания по модулю «Мониторинг»

В этом репозитории расположены ваши домашние задания к каждой лекции. 

Обязательны к выполнению задачи без звездочек. Их нужно выполнить, чтобы получить зачёт.

Задачи со звёздочкой (*) — дополнительные задачи или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Любые вопросы по решению задач задавайте в чате учебной группы. Ссылку вы найдёте в письме на вашей электронной почте.



2. [Система мониторинга Zabbix](hw-02.md)
# Система мониторинга Zabbix
## Рыльцева Виктория

---

### Задание 1. Установка Zabbix Server с веб-интерфейсом

#### Текст использованных команд:

```bash
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

Скриншот 1-2 (Вход)

https://github.com/user-attachments/assets/88e8b4c1-d60c-42d0-9a60-404ca12c65f7
https://github.com/user-attachments/assets/1196300d-8e95-4a4c-81ee-b74605e79ceb

3. [Система мониторинга Zabbix. Часть 2](hw-03.md)

