# Домашнее задание к занятию "Реляционные базы данных: SQL. Часть 2" - Юлия Гриб

### Задание 1.

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина,
- город нахождения магазина,
- количество пользователей, закрепленных в этом магазине.

```
SELECT s.store_id, CONCAT(st.last_name, ' ', st.first_name) AS staff, ci.city, COUNT(customer_id) AS cnt
FROM store s
JOIN staff st ON s.manager_staff_id = st.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city ci ON a.city_id = ci.city_id
JOIN customer c ON c.store_id = s.store_id
GROUP BY s.store_id
HAVING  cnt > 300;
```

![image](https://github.com/Yulia-GG/pattern-database2/blob/main/задание1.png)

### Задание 2.

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SELECT COUNT(film_id) 
FROM film 
WHERE length > (SELECT AVG(length) FROM film);
```

![image](https://github.com/Yulia-GG/pattern-database2/blob/main/задание2.png)

### Задание 3.

Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.

```
SELECT DATE_FORMAT(payment_date, '%Y-%m') AS month, SUM(amount), COUNT(rental_id)
FROM payment 
GROUP BY month
ORDER BY SUM(amount) DESC
LIMIT 1;
```

![image](https://github.com/Yulia-GG/pattern-database2/blob/main/задание3.png)

## Дополнительные задания (со звездочкой*)

### Задание 4*.

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», 
иначе должно быть значение «Нет».

```sql
SELECT staff_id, COUNT(rental_id),
IF(COUNT(rental_id) > 8000, 'Да', 'Нет') AS Премия
FROM payment
GROUP BY staff_id;
```

![image](https://github.com/Yulia-GG/pattern-database2/blob/main/задание4.png)

### Задание 5*.

Найдите фильмы, которые ни разу не брали в аренду.

```sql
SELECT f.title
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL;
```

![image](https://github.com/Yulia-GG/pattern-database2/blob/main/задание5.png)
