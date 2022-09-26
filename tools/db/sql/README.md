---
description: Structured Query Language
---

# 📑 SQL

sqlMaterials:

[https://tproger.ru/translations/sql-recap/](https://tproger.ru/translations/sql-recap/) - прочитать\
[https://postgrespro.ru/docs/postgresql/12/index](https://postgrespro.ru/docs/postgresql/12/index) - документация

Курс - [https://geekbrains.ru/chapters/1157](https://geekbrains.ru/chapters/1157)

#### Basic:

СУБД - система управления базами данных

### Правила проектирования:

* Правило №1: Все элементы внутри ячеек должны быть атомарными (ячейка содержит только одно значение, не несколько).
* Правило №2 Все строки должны быть различными.
* Правило №3 Любое поле таблицы, не входящее в состав первичного ключа,функционально полно зависит от первичного ключа.
* Правило №4 Все предыдущие правила, и плюс то, что любой функциональный атрибут зависит только от первичного ключа.

## Синтаксис

### Работа с базами данных

<pre class="language-sql"><code class="lang-sql"># Просмотр доступных баз данных
SHOW DATABASES;
<strong># Создание новой базы данных
</strong>CREATE DATABASE;
# Выбор базы данных для использования
USE &#x3C;database_name>; 
# Импорт SQL-команд из файла .sql
SOURCE &#x3C;path_of_.sql_file>; 
# Удаление базы данных
DROP DATABASE &#x3C;database_name>;</code></pre>

### Работа с таблицами

#### Просмотр таблиц, доступных в базе данных

```sql
SHOW TABLES; 
```

#### Создание новой таблицы

```sql
CREATE TABLE <table_name1> (
  <col_name1> <col_type1>,
  <col_name2> <col_type2>,
  <col_name3> <col_type3>
  PRIMARY KEY (<col_name1>),
  FOREIGN KEY (<col_name2>) REFERENCES <table_name2>(<col_name2>)
); 
```

#### **Ограничения целостности при использовании CREATE TABLE**

Может понадобиться создать ограничения для определённых столбцов в таблице. При создании таблицы можно задать следующие ограничения:

* ячейка таблицы не может иметь значение NULL;
* первичный ключ — `PRIMARY KEY (col_name1, col_name2, …)`;
* внешний ключ — `FOREIGN KEY (col_namex1, …, col_namexn) REFERENCES table_name(col_namex1, …, col_namexn)`.

Можно задать больше одного первичного ключа. В этом случае получится составной первичный ключ.

#### Сведения о таблице

Можно просмотреть различные сведения (тип значений, является ключом или нет) о столбцах таблицы следующей командой:

```sql
DESCRIBE <table_name>; 
```

#### Добавление данных в таблицу

```sql
INSERT INTO <table_name> (<col_name1>, <col_name2>, <col_name3>, …)
VALUES (<value1>, <value2>, <value3>, …);
# При добавлении данных в каждый столбец не требуется указывать названия столбцов.
INSERT INTO <table_name>
VALUES (<value1>, <value2>, <value3>, …); 
```

#### Обновление данных таблицы

```sql
UPDATE <table_name>
SET <col_name1> = <value1>, <col_name2> = <value2>, ...
WHERE <condition>; 
```

#### Удаление данных из таблицы

```sql
DELETE FROM <table_name>
WHERE ID = 2; 
# Delete all table data
DELETE FROM <table_name>
```

#### Удаление таблицы

```sql
DROP TABLE <table_name>; 
```

### Команды для создания запросов

#### SELECT

`SELECT` используется для получения данных из определённой таблицы:

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>;
# Следующей командой можно вывести все данные из таблицы:
SELECT * FROM <table_name>; 
```

#### SELECT DISTINCT

В столбцах таблицы могут содержаться повторяющиеся данные. Используйте `SELECT DISTINCT` для получения только неповторяющихся данных.

```sql
SELECT DISTINCT <col_name1>, <col_name2>, …
  FROM <table_name>; 
```

#### WHERE

Можно использовать ключевое слово `WHERE` в `SELECT` для указания условий в запросе:

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  WHERE <condition>; 
```

В запросе можно задавать следующие условия:

* сравнение текста;
* сравнение численных значений;
* логические операции AND (и), OR (или) и NOT (отрицание)

#### GROUP BY

Оператор `GROUP BY` часто используется с агрегатными функциями, такими как `COUNT`, `MAX`, `MIN`, `SUM` и `AVG`, для группировки выходных значений.

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  GROUP BY <col_namex>; 
```

#### HAVING

Ключевое слово `HAVING` было добавлено в SQL потому, что `WHERE` не может быть использовано для работы с агрегатными функциями.

```sql
SELECT <col_name1>, <col_name2>, ...
  FROM <table_name>
  GROUP BY <column_namex>
  HAVING <condition> 
```

#### ORDER BY

`ORDER BY` используется для сортировки результатов запроса по убыванию или возрастанию. `ORDER BY` отсортирует по возрастанию, если не будет указан способ сортировки `ASC` или `DESC`.

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  ORDER BY <col_name1>, <col_name2>, … ASC|DESC; 
```

#### BETWEEN

`BETWEEN` используется для выбора значений данных из определённого промежутка. Могут быть использованы числовые и текстовые значения, а также даты.

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  WHERE <col_namex> BETWEEN <value1> AND <value2>; 
```

#### LIKE

Оператор `LIKE` используется в `WHERE`, чтобы задать шаблон поиска похожего значения.\
Есть два свободных оператора, которые используются в `LIKE`:

* `%` (ни одного, один или несколько символов);
* `_` (один символ).

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  WHERE <col_namex> LIKE <pattern>; 
```

#### IN

С помощью `IN` можно указать несколько значений для оператора `WHERE`:

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  WHERE <col_namen> IN (<value1>, <value2>, …); 
```

#### JOIN

`JOIN` используется для связи двух или более таблиц с помощью общих атрибутов внутри них. На изображении ниже показаны различные способы объединения в SQL. Обратите внимание на разницу между левым внешним объединением и правым внешним объединением:

![](<../../../.gitbook/assets/image (9).png>)

```sql
SELECT <col_name1>, <col_name2>, …
  FROM <table_name1>
  JOIN <table_name2>
  ON <table_name1.col_namex> = <table2.col_namex>; 
```

#### View

`View` — это виртуальная таблица SQL, созданная в результате выполнения выражения. Она содержит строки и столбцы и очень похожа на обычную SQL-таблицу. `View` всегда показывает самую свежую информацию из базы данных.

**Создание**

```sql
CREATE VIEW <view_name> AS
  SELECT <col_name1>, <col_name2>, …
  FROM <table_name>
  WHERE <condition>; 
```

**Удаление**

```sql
DROP VIEW <view_name>; 
```

#### Агрегатные функции

Эти функции используются для получения совокупного результата, относящегося к рассматриваемым данным. Ниже приведены общеупотребительные агрегированные функции:

* `COUNT (col_name)` — возвращает количество строк;
* `SUM (col_name)` — возвращает сумму значений в данном столбце;
* `AVG (col_name)` — возвращает среднее значение данного столбца;
* `MIN (col_name)` — возвращает наименьшее значение данного столбца;
* `MAX (col_name)` — возвращает наибольшее значение данного столбца.

#### Вложенные подзапросы

Вложенные подзапросы — это SQL-запросы, которые включают выражения `SELECT`, `FROM` и `WHERE`, вложенные в другой запрос.

## Command types

* DDL – Data Definition Language
* DQl – Data Query Language
* DML – Data Manipulation Language
* DCL – Data Control Language
* TCL – Transaction Control Language

<figure><img src="../../../.gitbook/assets/изображение (5).png" alt=""><figcaption></figcaption></figure>
