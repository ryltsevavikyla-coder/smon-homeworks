# Домашние задания по модулю «Мониторинг»


# Система мониторинга Zabbix
## Рыльцева Виктория Михайловна

---

### Задание 1. Установка Zabbix Server с веб-интерфейсом

#### Текст использованных команд:



## 1. Установка PostgreSQL
sudo apt update
sudo apt install -y postgresql

## 2. Создание пользователя и базы данных
sudo -u postgres psql -c "create user zabbix with password 'zabbix';"
sudo -u postgres psql -c "create database zabbix owner zabbix;"

## 3. Добавление репозитория Zabbix 6.0
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
sudo dpkg -i zabbix-release_6.0-4+debian11_all.deb
sudo apt update

## 4. Установка Zabbix сервера и веб-интерфейса
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

## 5. Импорт схемы базы данных
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

## 6. Настройка пароля в конфиге
sudo sed -i 's/# DBPassword=/DBPassword=zabbix/' /etc/zabbix/zabbix_server.conf

## 7. Настройка часового пояса
echo "php_value date.timezone Europe/Moscow" | sudo tee -a /etc/zabbix/apache.conf

## 8. Запуск сервисов
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2



#### Скриншот авторизации в админке:
   
СКРИНШОТ 1

![Авторизация](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_130703.png)

# Задание 2. Установка Zabbix Agent на два хоста


## Текст использованных команд:


### На первом хосте (Zabbix Server):


## sudo apt update
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


## Скриншот Configuration → Hosts:

СКРИНШОТ 2

![Хосты](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_143420.png)


## Скриншот лога zabbix agent:

СКРИНШОТ 3

![Лог агента](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_162143.png)


## Скриншот раздела Monitoring > Latest data:

СКРИИНШОТ 4

![Latest Data 1](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_163215.png)

СКРИНШОТ 5

![Latest Data 2](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260314_163259.png)




# 3. [Система мониторинга Zabbix. Часть 2](hw-03.md)



# Система мониторинга Zabbix. Часть 2
## Рыльцева Виктория Михайловна

---

### Задание 1. Создание своего шаблона для мониторинга CPU и RAM

#### Скриншот шаблона с элементами данных:
![Template CPU-RAM](https://github.com/ryltsevavikyla-coder/smon-homeworks/blob/main/capture_260317_105232.png)

---

### Задание 2 - 3 . Переименование хостов, Привязка шаблонов к хостам

#### Скриншот Configuration → Hosts с новыми именами и настроек хоста с привязанными шаблонами:

![Хосты с шаблонами](https://github.com/ryltsevavikyla-coder/smon-homeworks/blob/main/capture_260317_105937.png)



### Задание 4. Создание кастомного дашборда

#### Скриншот дашборда:
![Дашборд](https://github.com/ryltsevavikyla-coder/smon-homeworks/raw/main/capture_260315_164935.png)



