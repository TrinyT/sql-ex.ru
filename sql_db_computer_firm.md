Схема БД состоит из четырех таблиц:<br/>
**Product** (maker, model, type)<br/>
**PC** (code, model, speed, ram, hd, cd, price)<br/>
**Laptop** (code, model, speed, ram, hd, price, screen)<br/>
**Printer** (code, model, color, type, price)<br/>

Таблица *Product* представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер).
Предполагается, что номера моделей в таблице *Product* уникальны для всех производителей и типов продуктов. 
В таблице *PC* для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах).
Таблица *Laptop* аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.


## Задание: 1
	Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd
```sql
select model, speed, hd
from pc
where price<500
```
| model  | speed |  hd  |
|--------|-------|------|
|  1232  |  500  | 10.0 |
|  1232  |  450  | 8.0  |
|  1232  |  450  | 10.0 |
|  1260  |  500  | 10.0 |


## Задание: 2
	Найдите производителей принтеров. Вывести: maker
```sql
SELECT DISTINCT maker 
FROM product 
WHERE type = 'printer';
```
| maker | 
|-------|
|   A   | 
|   D   | 
|   E   | 


## Задание: 3
	Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT model, ram, screen 
FROM Laptop
WHERE price > 1000;
```
| model  |  ram  | screen |
|--------|-------|--------|
|  1750  |  128  |   14   |
|  1298  |  64   |   15   |
|  1752  |  128  |   14   |


## Задание: 4
	Найдите все записи таблицы Printer для цветных принтеров.
```sql
SELECT * FROM Printer
WHERE color = 'y';
```
| code | model  | color | type  |   price   |
|------|--------|-------|-------|-----------|
|   3  |  1433  |   y   |  Jet  |  290.0000 |
|   2  |  1433  |   y   |  Jet  |  270.0000 |


## Задание: 5
	Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```sql
SELECT model, speed, hd
FROM PC
WHERE (cd = '12x' OR cd = '24x') AND price < 600;
```
| model  | speed |  hd  |
|--------|-------|------|
|  1232  |  500  | 10.0 |
|  1232  |  450  | 8.0  |
|  1232  |  450  | 10.0 |
|  1260  |  500  | 10.0 |


## Задание: 6
	Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
```sql
SSELECT DISTINCT maker, speed
FROM Laptop as l
JOIN Product as p
ON l.model=p.model
WHERE hd>=10;
```
| maker |  speed |
|-------|--------|
|   A   |   450  | 
|   A   |   600  | 
|   A   |   750  |
|   B   |   750  |


## Задание: 7
	Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
```sql
WITH tab AS (
	 (SELECT model, price FROM pc)
	 UNION
	 (SELECT model, price FROM laptop)
	 UNION
	 (SELECT model, price FROM printer)
	)
SELECT product.model, tab.price 
FROM product 
JOIN tab 
ON product.model=tab.model 
WHERE product.maker='B'
```
| model |    price  |
|-------|-----------|
|  1121 | 850.0000  | 
|  1750 | 1200.0000 | 


## Задание: 8 
	Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```sql
SELECT Maker FROM product WHERE type = 'pc'
EXCEPT 
SELECT Maker FROM product WHERE type='Laptop';
```
| Maker |   
|-------|
|   E   |


## Задание: 9
	Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
```sql
SELECT DISTINCT maker
FROM product
JOIN pc
ON product.model=pc.model
WHERE pc.speed >=450
```
| Maker |   
|-------|
|   A   |
|   B   |
|   E   |


## Задание: 10
	Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
```sql
SELECT model, price
FROM printer
WHERE price = (SELECT max(price) FROM printer);
```
| model |    price  |
|-------|-----------|
|  1288 | 400.0000  | 
|  1276 | 400.0000  |


## Задание: 11
	Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) AS AVG_speed
FROM pc;
```
| AVG_speed |
|-----------|
|    608    |


## Задание: 12
	Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT AVG(speed) AS AVG_speed
FROM laptop
WHERE price>1000;
```
| AVG_speed |
|-----------|
|    700    |


## Задание: 13
	Найдите среднюю скорость ПК, выпущенных производителем A.
```sql
	WITH tab AS (
	SELECT speed FROM pc
	JOIN Product
	ON Product.model=pc.model
	WHERE Product.maker = 'A'
	)
SELECT AVG(speed) AS AVG_speed
FROM tab;
```
| AVG_speed |
|-----------|
|    606    |


## Задание: 19
	Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. Вывести: maker, средний размер экрана.
```sql
SELECT maker, avg(screen) as  средний_размер_экрана
FROM laptop
JOIN product
ON Product.model=laptop.model
GROUP BY maker;
```
| maker |средний_размер_экрана|
|-------|---------------------|
|   A   |          13         | 
|   B   |          14         |
|   C   |          12         |


## Задание: 21
	Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC. Вывести: maker, максимальная цена.
```sql
SELECT maker, round(max(price),2) as  максимальная_цена
FROM pc
JOIN product
ON pc.model=product.model
GROUP BY maker;
```
| maker |максимальная_цена|
|-------|-----------------|
|   A   |      980.00     | 
|   B   |      850.00     |
|   E   |      350.00     |


## Задание: 22
	Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.
```sql
SELECT speed, round(avg(price),2) as  средняя_цена
FROM pc
WHERE speed>600
GROUP BY speed;
```
| speed |средняя_цена|
|-------|------------|
|  750  |  900.00    | 
|  800  |  970.00    |
|  900  |  980.00    |
