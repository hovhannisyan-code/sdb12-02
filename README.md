# Домашнее задание к занятию «Работа с данными (DDL/DML)»

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

```shell
docker run --name mysql-sakila -e MYSQL_ROOT_PASSWORD=rootpass -p 3306:3306 -d mysql:8.0
```

1.2. Создайте учётную запись sys_temp.

```sql
CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'TempPassword123!';
```

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

```sql
SELECT user, host FROM mysql.user;
```
![Result](https://github.com/hovhannisyan-code/sdb12-02/blob/master/img/screenshot_0.png)

1.4. Дайте все права для пользователя sys_temp.

```sql
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

```sql
SHOW GRANTS FOR 'sys_temp'@'%';
```

![Result](https://github.com/hovhannisyan-code/sdb12-02/blob/master/img/screenshot_1.png)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос:

```sql
ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

```shell
mysql -h 127.0.0.1 -u sys_temp -p
```

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

```shell
wget https://downloads.mysql.com/docs/sakila-db.zip
```
```shell
docker cp sakila-db/sakila-schema.sql mysql-sakila:/sakila-schema.sql
```
```shell
docker cp sakila-db/sakila-data.sql mysql-sakila:/sakila-data.sql
```
1.7. Восстановите дамп в базу данных.
```sql
source sakila-schema.sql;
```
```sql
source sakila-data.sql;
```


1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

![Result](https://github.com/hovhannisyan-code/sdb12-02/blob/master/img/screenshot_2.png)

### Задание 2

Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

```
Название таблицы | Название первичного ключа
customer         | customer_id
```

```sql
SELECT 
    TABLE_NAME AS `Название таблицы`,
    GROUP_CONCAT(COLUMN_NAME ORDER BY ORDINAL_POSITION) AS `Название первичного ключа`
FROM 
    information_schema.KEY_COLUMN_USAGE
WHERE 
    TABLE_SCHEMA = 'sakila'
    AND CONSTRAINT_NAME = 'PRIMARY'
GROUP BY 
    TABLE_NAME
ORDER BY 
    TABLE_NAME;
```
| Название таблицы | Название первичного ключа   |
|------------------|-----------------------------|
| actor            | actor_id                    |
| address          | address_id                  |
| category         | category_id                 |
| city             | city_id                     |
| country          | country_id                  |
| customer         | customer_id                 |
| film             | film_id                     |
| film_actor       | actor_id,film_id            |
| film_category    | film_id,category_id         |
| film_text        | film_id                     |
| inventory        | inventory_id                |
| language         | language_id                 |
| payment          | payment_id                  |
| rental           | rental_id                   |
| staff            | staff_id                    |
| store            | store_id                    |


![Result](https://github.com/hovhannisyan-code/sdb12-02/blob/master/img/screenshot_3.png)

## Дополнительные задания (со звёздочкой\*)

Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3\*

3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

```sql
REVOKE INSERT, UPDATE, DELETE ON sakila.* FROM 'sys_temp'@'%';
```

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

_Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами._

![Result](https://github.com/hovhannisyan-code/sdb12-02/blob/master/img/screenshot_3.png)