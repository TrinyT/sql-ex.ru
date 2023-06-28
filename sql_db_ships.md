Рассматривается БД кораблей, участвовавших во второй мировой войне. Имеются следующие отношения:</br>
**Classes** (class, type, country, numGuns, bore, displacement)</br>
**Ships** (name, class, launched)</br>
**Battles** (name, date)</br>
**Outcomes** (ship, battle, result)</br>

Корабли в «классах» построены по одному и тому же проекту, и классу присваивается либо имя первого корабля, построенного по данному проекту, либо названию класса дается имя проекта, которое не совпадает ни с одним из кораблей в БД. Корабль, давший название классу, называется головным.</br>
Отношение *Classes* содержит имя класса, тип (bb для боевого (линейного) корабля или bc для боевого крейсера), страну, в которой построен корабль, число главных орудий, калибр орудий (диаметр ствола орудия в дюймах) и водоизмещение ( вес в тоннах).</br>
В отношении *Ships* записаны название корабля, имя его класса и год спуска на воду. </br>
В отношение *Battles* включены название и дата битвы, в которой участвовали корабли.</br>
B отношении *Outcomes* – результат участия данного корабля в битве (потоплен-sunk, поврежден - damaged или невредим - OK).</br>

Замечания.</br>
1) В отношение *Outcomes* могут входить корабли, отсутствующие в отношении *Ships*.</br>
2) Потопленный корабль в последующих битвах участия не принимает.

## Задание: 14
	Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
```sql
SELECT Ships.class, name, country 
FROM Ships
JOIN Classes
ON Classes.class=Ships.class
WHERE Classes.numGuns>=10;
```
|       class     |       name      |  country |
|-----------------|-----------------|----------|
|  Tennessee      |  California     |    USA   |
|  North Carolina |  North Carolina |    USA   |
|  North Carolina |  South Dakota   |    USA   |
|  Tennessee      |  Tennessee      |    USA   |
|  North Carolina |  Washington     |    USA   |


## Задание: 31
	Для классов кораблей, калибр орудий которых не менее 16 дюймов, укажите класс и страну.
```sql
SELECT class, country
FROM Classes
WHERE bore>=16;
```
| class |country|
|-------|------------|
|Iowa|USA| 
|North Carolina|USA|
|Yamato|Japan|


## Задание: 33
	Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship.
```sql
SELECT ship FROM Outcomes
WHERE battle = 'North Atlantic' and result = 'sunk';
```
|ship|
|----|
|Bismarck| 
|Hood|

## Задание: 42
	Найдите названия кораблей, потопленных в сражениях, и название сражения, в котором они были потоплены.
```sql
SELECT ship, battle FROM Outcomes
WHERE result = 'sunk';
```
| ship |battle|
|-------|------------|
|Bismarck|North Atlantic| 
|Fuso|Surigao Strait|
|Hood|North Atlantic|
|Kirishima|Guadalcanal|
|Schamhorst|North Cape|
|Yamashiro|Surigao Strait|

## Задание: 44
	Найдите названия всех кораблей в базе данных, начинающихся с буквы R.
```sql
SELECT a.name 
FROM (
	SELECT Ships.name FROM Ships
	UNION
	SELECT Outcomes.ship FROM Outcomes) as a
WHERE name LIKE 'R%'
;
```
|name|
|----|
|Ramillies|
|Renown|
|Repulse|
|Resolution|
|Revenge|
|Rodney|
|Royal Oak|
|Royal Sovereign|

## Задание: 45
	Найдите названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V).
Считать, что слова в названиях разделяются единичными пробелами, и нет концевых пробелов.
```sql
SELECT a.name 
FROM (
	SELECT Ships.name FROM Ships
	UNION
	SELECT Outcomes.ship FROM Outcomes) as a
WHERE name LIKE '% % %'
;
```
|name|
|----|
|Duke of York|
|King George V|
|Prince of Wales|
