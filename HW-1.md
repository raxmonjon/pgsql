Создадим новую ВМ в Hyper-V (Ubuntu 22.04 LTS)
![image](https://github.com/raxmonjon/pgsql/assets/43344039/00be8ac9-bcac-468d-a8ca-2127ab02b365)

Открываем терминал и задаём команды:
sudo apt update
sudo apt upgrade
![image](https://github.com/raxmonjon/pgsql/assets/43344039/ce01234f-4e4d-428c-ad43-77db40167eca)

Будем исползовать MobaXterm что бы соиденитця к серверу по ssh
sudo apt -y install postgresql-14
После установки посмотрим запустилос ли postgresql-14
pg_lsclusters
![image](https://github.com/raxmonjon/pgsql/assets/43344039/89300766-cdb8-4282-abed-45905c97c9f8)

Дадим доступ к порту 5432 
cd /etc/postgresql/14/main/
vi postgresql.conf
изменим listen_addresses = '*'
pg_ctlcluster reload 14 main - делаем релоад наш инстанс (не принимает конфиг)
systemctl restart postgresql@14-main

Дадим доступ для подключение по сети
vi /etc/postgresql/14/main/pg_hba.conf
host    all             all             0.0.0.0/0            scram-sha-256

изменяем парол позователя postgres
sudo passwd postgres

открываем psql с ползователя postgres
sudo -u postgres psql

изменяем парол позователя postgresql
ALTER USER postgres WITH PASSWORD 'new_password';

создадим базу данных:
CREATE DATABASE billing;

посмотрим с какого ползователя на какую базу мы подключыни:
\c
> You are now connected to database "postgres" as user "postgres".

Постмотрим список базы данныйх:
\l
                              List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 billing   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres

Подключаемся на базу данных billing:
\connect billing

Для работы с SQL запросами будем подключатся к нашему базу через DataGrip:
![image](https://github.com/raxmonjon/pgsql/assets/43344039/3e402e4b-5593-447a-a307-0ff35905a2aa)
![image](https://github.com/raxmonjon/pgsql/assets/43344039/ca9aebc3-bddb-4c3d-8ebe-a47554fa0239)

посмотрим текущий уровень изоляции:
show transaction isolation level
>read committed

создадым таблицу:
create table persons(id serial, first_name text, second_name text);




