# lab_01_1

## Работа с переменными и типами данных.

## Цель:
1. Ознакомиться с основными типами данных в языке Python: числа, строки и структуры данных.
2. Научиться работать с операциями для различных типов данных.
3. Закрепить навыки написания программ с использованием базовых конструкций языка.
4. Развить умение формализовать задачи, разрабатывать алгоритмы их решения и переносить их на программный код.

## Задачи:
1.	Изучить основные арифметические и логические операции для работы с числами.
2.	Научиться работать со строками, включая индексацию, срезы и основные операции
3.	Изучить базовые структуры данных Python (списки и словари) и основные действия с ними.
4.	Применить аналитический подход к решению задач: разбить решение на этапы, формализовать алгоритм, перенести его в программный код.

## №1.1.1. Составьте программу, которая запрашивает у пользователя 2 целых числа и выполняет операции: 
• арифметические: + , - , * , / , // , % , ** ; 
• сравнение: < , <= , > , >= , != , == , выводя на экран результат каждого действия. В случае получение вещественного результата, округлите его до 2-х знаков после запятой (используя функцию round() ).

## Решение:


```python
# Задание task_01_01
""" Составьте программу, которая запрашивает у пользователя 2 целых числа и
выполняет операции:
•	арифметические: +, -, * , / , // , %, **;
•	сравнение: <, <=, >, >=, !=, ==,
выводя на экран результат каждого действия. В случае получение вещественного
результата, округлите его до 2-х знаков после запятой (используя функцию
round()).
"""

# Выполнила: Губайдуллина А.И.
# Группа: АБП-231

a = int(input("a="))
b = int(input("b="))

print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a // b)
print(a % b)
print(a ** b)

print(a < b)
print(a <= b)
print(a > b)
print(a >= b)
print(a != b)
print(a == b)
```
Получим результат:


![image](https://github.com/user-attachments/assets/82bab778-c78f-424d-a47b-4b4db55afc20)

2. Проверим данные во временной таблице.
```sql
SELECT * FROM customer_points;
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/85b5c6f5-2318-499a-8f85-f8d4a6400c0c)

3.	Создадим аналогичную таблицу для каждого дилерского центра:
```sql
SELECT * FROM customer_points;
CREATE TEMP TABLE dealership_points AS (
SELECT
dealership_id,
point(longitude, latitude) AS lng_lat_point
FROM dealerships
);
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/8815ebde-daa1-4989-90dc-727fdfda1868)

Проверим данные во временной таблице:
```sql
SELECT * FROM dealership_points;
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/d70fa942-b0ff-46c9-8b49-9745fda1cf02)

3. Объединим эти таблицы, чтобы рассчитать расстояние от каждого клиента до каждого дилерского центра (в милях):
```sql
CREATE TEMP TABLE customer_dealership_distance AS (
SELECT
customer_id,
dealership_id,
c.lng_lat_point <@> d.lng_lat_point AS distance
FROM customer_points c
CROSS JOIN dealership_points d
);
```

Результат выполнения:


![image](https://github.com/user-attachments/assets/3a49071e-1273-4dc1-9612-eb875f9f6937)


Проверим данные во временной таблице:
```sql
SELECT * FROM customer_dealership_distance;
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/5452f099-aa82-4404-8b07-17f9615c1973)

4. Выберем ближайший дилерский центр для каждого клиента, используя следующий запрос:
```sql
CREATE TEMP TABLE closest_dealerships AS (
SELECT DISTINCT ON (customer_id)
customer_id,
dealership_id,
distance
FROM customer_dealership_distance
ORDER BY customer_id, distance
);
```

Результат выполнения:


![image](https://github.com/user-attachments/assets/9e7b707e-554e-4c23-b302-91611959fb6b)

Проверим данные во временной таблице:
```sql
SELECT * FROM closest_dealerships;
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/8021776a-bed8-493b-ad30-8477d3ea962f)

5. Рассчитаем среднее расстояние от каждого клиента до его ближайшего дилерского центра.
```sql
SELECT
AVG(distance) AS avg_dist,
PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY distance) AS
median_dist
FROM closest_dealerships;
```
Результат выполнения:


![image](https://github.com/user-attachments/assets/b5dba697-4074-4ec3-bc2c-dc0757364c03)

6. Проведем выгрузку полученного результата из временной таблицы в CSV

Получим результат:


![Screenshot_63](https://github.com/user-attachments/assets/7cf6e101-21d6-4c9d-af3f-115be56cc3c0)

 
7. Построим карту клиентов и сервисных центров в облачной визуализации Yandex DataLence
Для подключения загрузим CSV файл и создадим датасет. Затем создадим новый чарт и выберем тип "Карта". На ней добавляем слои и указываем связи.

Получаем результат:


![Screenshot_77](https://github.com/user-attachments/assets/7b0c8add-0078-4529-ad65-f99af3e8f729)





Ссылка на результат в BI-системе https://datalens.yandex.cloud/wizard/fy4vaxoy2vhu1-novyy-datasettttttttttttttttttttttttttttttt-karta

8. Удалим временные таблицы:
```sql
drop TABLE IF EXISTS customer_points;
```
Результаты выполнения:


![image](https://github.com/user-attachments/assets/790a0fad-7ddd-43b4-8e94-4ef27c841202)

```sql
drop TABLE IF EXISTS dealership_points;
```
Результаты выполнения:


![image](https://github.com/user-attachments/assets/aa63035a-d761-4e0e-bf4d-e094c3138a6c)

```sql
drop TABLE IF EXISTS customer_dealership_distance;
```
Результаты выполнения:


![image](https://github.com/user-attachments/assets/2d32a3be-0cf5-46e9-860e-3bb2fdd098f6)


## Вывод
Выполнила описательный анализ данных временных рядов с помощью DATETIME. Использовала геопространственные данные для определения взаимосвязей. Использовала сложные типы данных (массивы, JSON и JSONB).
