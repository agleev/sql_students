# О базе данных
Добрый день! База данных "Учебная аналитика по курсу" была взята мной из курса [Интерактивный тренажер по SQL](https://stepik.org/course/63054), который я проходил, изучая SQL. 
Она была добавлена разработчиками курса на основе прохождения курса студентами. База данных имеет схему:

![Схема базы данных](https://ucarecdn.com/df6e64a7-45f3-4914-8d4b-1f2f272a0e40/ "Таблицы базы данных")

Курс делится на тематические модули `module`, модули на уроки  `lesson`, уроки на шаги `step` с заданиями разных типов `type` - от простого теста до создания sql-запросов.
- таблица `module` - модули курса с названиями `module_name` и id `module_id`;
- таблица `lesson` - названия уроков `lesson_name`, id модулей `module_id`, к которым относятся, и порядковый номер внутри модуля `lesson_position`;
- таблица `step` - названия шагов `step_name`, их id `step_id`, их тип `step_type`, id урока `lesson_id`, к которому относится, и порядковый номер внутри урока `step_position`;
- таблицы `keyword` и `step_keyword` - навигация по курсу с ключевыми словами `keyword_name` и шагами `step_id`, где они используются;
- таблица `step_student` - попытки пользователей `student_id` с id шагов `step_id`, временем начала `attempt_time` и окончанием `submission_time` попытки `step_student_id`, 
результатом `result`.
- и наконец таблица `student` с именами `student_name` и id студентов `student_id`.

# Задания

Исходя из этих данных я придумал задания с использованием рекурсивных таблиц, подзапросов и оконных функции. [Хорошие задания](https://stepik.org/lesson/404275/step/1?unit=393473) на написание запросов есть в самом курсе, но курс больше не модерируется,
поэтому в комментариях уже есть ответы.

# Библиотеки

```python
import pandas as pd
```
Библиотека для работы с данными. Мне удобно получать таблицы в виде датафреймов.

```python
import psycopg2
```
Библиотека для работы с PostgreSQL. Для подключения к базе данных и работы с ней.

```python
from config_postgresql import database, user, password, port, host
```
Файл с конфигурациями

# Функция

```python
def sql(request):
    cursor = connection.cursor()
    cursor.execute(request)
    result = cursor.fetchall()
    columns = []
    for desc in cursor.description:
        columns.append(desc[0])
    
    return pd.DataFrame(result, columns=columns)
 ```
 Объявим функцию, зададим параметр - запрос к базе данных, которая сформирует из sql-таблицы датафрейм.
 
 Пример:
 ```python
 sql('''
 select * from step
 ''')
 ```
 
 # Сылки
 
- [Интерактивный тренажер по SQL](https://stepik.org/course/63054) - сам курс.
- [Учебная аналитика по курсу.sql](https://stepik.org/media/attachments/course/63054/lesson_3_5.sql) - ссылка на скачивание базы данных (MySQL).
- [Мой сертификат](https://stepik.org/cert/1309595) о прохождении этого курса.
- [Остальные сертификаты](https://stepik.org/users/357691718/certificates) курсов, которые я прошел на 'Степике'.
- [Мои работы](https://www.kaggle.com/agleev/code) на kaggle.com
