Проблема 1
Таблица: oc_product_to_category

Задача:
Нужно убрать дублирующиеся product_id значения и скрыть столбец category_id

Решение:
SELECT DISTINCT `product_id` FROM `oc_product_to_category` 

Примечание:
Результат выполнения 4401 строка из 13178, запрос работает и показывает кол-во.
Как вариант быстрой проверки - сравнить с количеством товаров в таблице продуктов Таблица: oc_product значение 4111.
Выявлена проблема, остались мусорные значения, разница составила 290 товаров, которые были некорректно удалены.

Проблема 2
Таблица: oc_product_to_category и oc_product

Задача: необходимо из таблицы oc_product_to_category удалить id товара которого нет в таблице oc_product, необходимо только для быстрой проверки по кол-ву.


SELECT * FROM oc_product_to_category LEFT JOIN oc_product ON oc_product_to_category.product_id=oc_product.product_id WHERE oc_product.product_id IS NULL 

SELECT * FROM oc_product JOIN oc_product_to_category USING (product_id)


Запрос возвращает товар без категорий
SELECT `oc_product`.* FROM `oc_product` left join `oc_product_to_category` on `oc_product`.product_id = `oc_product_to_category`.product_id where `oc_product_to_category`.product_id is null 

выбрать только идентифицирующие категории
SELECT  `category_id`, `parent_id` FROM `oc_category` 

удалить товары из категории
DELETE FROM `oc_product_to_category` WHERE `category_id` = 28
