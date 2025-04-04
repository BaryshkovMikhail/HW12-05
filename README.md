# Домашнее задание к занятию "`SQL. Часть 2`" - `Барышков Михаил`

## Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

---

## Решение 1

```sql
SELECT
 ROUND(SUM(index_length) / SUM(data_length + index_length) * 100, 2) AS "процентное отношение размера индексов к общему размеру (данные + индексы)",
 CONCAT(ROUND(SUM(data_length) / (1024 * 1024), 2), ' MB') AS "Общий размер таблиц",
 CONCAT(ROUND(SUM(index_length) / (1024 * 1024), 2), ' MB') AS "Общий размер индексов"
FROM
 information_schema.TABLES
WHERE
 table_schema = 'sakila';
```

<img src = "img/img1.png" width = 80%>

Этот запрос:

1. Считает общий размер данных (data_length) и общий размер индексов (index_length) для всех таблиц в схеме 'sakila'Вычисляет процентное отношение размера индексов к общему размеру (данные + индексы)

2. Также предоставляет информацию об общем размере таблиц и индексов в мегабайтах для наглядности

3. Результат покажет, какую часть от общего размера базы данных занимают индексы, что помогает оценить эффективность их использования.

---

## Задание 2

Выполните explain analyze следующего запроса:

```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```

- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

---

## Решение 2

```sql
EXPLAIN ANALYZE
SELECT
 DISTINCT concat(c.last_name, ' ', c.first_name),
 sum(p.amount) OVER (PARTITION BY c.customer_id,
 f.title)
FROM
 payment p,
 rental r,
 customer c,
 inventory i,
 film f
WHERE
 date(p.payment_date) = '2005-07-30'
 AND p.payment_date = r.rental_date
 AND r.customer_id = c.customer_id
 AND i.inventory_id = r.inventory_id;
```

<img src = "img/img2.png" width = 100%>

## Дополнительные задания (со звёздочкой*)

Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*

Самостоятельно изучите, какие типы индексов используются в PostgreSQL. Перечислите те индексы, которые используются в PostgreSQL, а в MySQL — нет.

*Приведите ответ в свободной форме.*
