1) Написать запрос, который выдаст все заявки, которые были отработаны после «2020-12-02 12:00:00». Формат вывода должен быть следующий: номер заявки, имя пользователя, время отработки, название статуса. Данные должны быть отсортированы по возрастанию по дате отработки.

```sql
select t1.tt_number as 'номер заявки'
,t2.name as 'имя пользователя'
,t1.work_time as 'время отработки'
,t3.name as 'название статуса'
from TroubleTickets as t1 LEFT JOIN Users as t2 ON t1.user_id = t2.id 
LEFT JOIN TTStatus as t3 ON t1.tt_status = t3.id
where work_time > '2020-12-02 12:00:00'
```
2) Написать запрос, который выдаст все заявки, отработанные «Петровой Марией» со статусом «ожидание». Формат вывода должен быть следующий: номер заявки, имя пользователя, время отработки, название статуса.

```sql
WITH table1 as (select t1.tt_number as 'номер заявки'
,t2.name as 'имя пользователя'
,t1.work_time as 'время отработки'
,t3.name as 'название статуса'
from TroubleTickets as t1 LEFT JOIN Users as t2 ON t1.user_id = t2.id 
LEFT JOIN TTStatus as t3 ON t1.tt_status = t3.id)

select *
FROM table1
where  table1.[имя пользователя]= 'Петрова Мария' 
AND table1.[название статуса]= 'Ожидание'
```

3) Написать запрос, который выдаст все заявки, отработанные сотрудниками с фамилией «Иванов» со статусом «ожидание». Формат вывода должен быть следующий: номер заявки, имя пользователя, время отработки, название статуса. Данные должны быть отсортированы по убыванию по дате отработки. 
```sql
WITH table1 as (select t1.tt_number as 'номер заявки'
,t2.name as 'имя пользователя'
,t1.work_time as 'время отработки'
,t3.name as 'название статуса'
from TroubleTickets as t1 LEFT JOIN Users as t2 ON t1.user_id = t2.id 
LEFT JOIN TTStatus as t3 ON t1.tt_status = t3.id)

select *
FROM table1
where  table1.[имя пользователя] like 'Иванов%' 
AND table1.[название статуса]= 'Ожидание'
ORDER BY table1.[время отработки] DESC
```
4) Написать запрос, который выдаст количество отработанных заявок по дням, со статусом «открыта» и «ожидание» за период «2020-12-02» по «2020-12-04». Формат вывода должен быть следующий: дата в формате день-месяц-год, название статуса, количество заявок. 
```sql
WITH table1 as (select t1.tt_number as 'номер заявки'
,t2.name as 'имя пользователя'
,date(t1.work_time) as 'время отработки'
,t3.name as 'название статуса'
from TroubleTickets as t1 LEFT JOIN Users as t2 ON t1.user_id = t2.id 
LEFT JOIN TTStatus as t3 ON t1.tt_status = t3.id)

select  table1.[время отработки] as 'дата'
, table1.[название статуса]
,count(table1.[название статуса]) as 'количество заявок'
FROM table1
where  table1.[название статуса] != 'Закрыта' 
and table1.[время отработки] BETWEEN '2020-12-02'
and '2020-12-04'
GROUP BY table1.[время отработки], table1.[название статуса]
```
5) Написать запрос, который выведет всех пользователей и количество их заявок со статусом «открыта» с сортировкой по имени пользователя.
```sql
WITH table1 as (select t1.tt_number as 'номер заявки'
,t2.name as 'имя пользователя'
,date(t1.work_time) as 'время отработки'
,t3.name as 'название статуса'
from TroubleTickets as t1 LEFT JOIN Users as t2 ON t1.user_id = t2.id 
LEFT JOIN TTStatus as t3 ON t1.tt_status = t3.id)

select  table1.[имя пользователя]
, count(table1.[название статуса]) as 'Количество заявок со статусом открыта'
FROM table1
where  table1.[название статуса] = 'Открыта'
group by table1.[имя пользователя]
ORDER BY table1.[имя пользователя] ASC
```
