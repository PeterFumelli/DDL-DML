# Домашнее задание к занятию "`Работа с данными (DDL/DML)`" - `Фумелли Петр`

### Задание 1

# запуск mysql в докере

docker run -d \
--name mysql_netology \
-e MYSQL_ROOT_PASSWORD=Passw0rd! \
-p 3306:3306 \
mysql:8.0

# подключение внутрь контейнера (root)

docker exec -it mysql_netology mysql -u root -p

# создание пользователя

CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
ALTER USER 'sys_temp'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
exit;

# скачивание дампа Sakila

curl -LO <https://downloads.mysql.com/docs/sakila-db.zip>
unzip sakila-db.zip -d ./sakila-db
cd sakila-db

# копирование файлов внутрь контейнера

docker cp sakila-schema.sql mysql_netology:/tmp
docker cp sakila-data.sql mysql_netology:/tmp

# создание базы и загрузка дампа

docker exec -it mysql_netology mysql -u sys_temp -ppassword -e "CREATE DATABASE IF NOT EXISTS sakila;"

docker exec -it mysql_netology bash -c "mysql -u sys_temp -ppassword sakila < /tmp/sakila-schema.sql"
docker exec -it mysql_netology bash -c "mysql -u sys_temp -ppassword sakila < /tmp/sakila-data.sql"

# проверка (таблицы)

docker exec -it mysql_netology mysql -u sys_temp -ppassword -e "USE sakila; SHOW TABLES;"

### Задание 2
