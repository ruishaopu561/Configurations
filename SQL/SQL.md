# SQL
Basic notes about SQL.

## Chapter 3

### Basic type
```sql
char(n), 
varchar(n), 
int, 
smallint, 
numeric, 
real, double precision,
float
```
### Basic Schema Definition
create a table
```sql
create table department 
    (depy_name varchar(20),
    building varchar(15),
    budget numeric(12, 2) not null default 0,
    primary key (dept_name)
    foreign key(...));
```
insert tuple
```sql
insert into instructor values(10211, 'Smith', 'Biology', 6600);
```
delete tuple
```sql
delete from student 
where P;
```
delete table
```sql
drop table student;
```
table add attribute
```sql
alter table r add A D;
```
table delete attribute
```sql
alter table r drop A;
```
select
```sql
select A1, A2, ... , An
from r1, r2, ... , rn
where P;
```
select (distinct 去除重复)
```sql
select (distinct) name 
from instructor 
where a and/or/not/ b;
```
all显示指明不去除重复
```sql
select all dept_name 
from instructor;
```

natural join
```sql
select A1, A2, ... , An
from r1 natural join r2 natural join  ... natural join rn
where P;
```
join...using (避免一些不必要的链接，using中的属性r1,r2中存在相等即可)
```
r1 join r2 using (A1, A2);
```
as in select(easy to combine with himself)
```sql
select r1 as newr1, r2
from A1, A2
where P;
```
as in from 
```sql
select T.r1, S.r2
from A1 as T, A2 as S
where P;
```
### String Opeartions
```sql
% : match any substring;
_ : match any character;
'Intro%' : match any string beginning with "Intro";
'%Comp%' : any string containing "Comp" as a substring;
'___' : match any string of exactly three characters;
'___%' : match any string of at least three characters;
like 'ab\%cd%' escape '\' : match any string beginning with "ab%cd";
like 'ab\\cd%' escape '\' : match any string beginning with "ab\cd";
```
eg
```sql
select dept_name
from department
where building like '%Watson%';
```
order by 
```sql
select r1
from A1
where P
order by A;
```
desc, asc
```sql
select * 
from instructor
order by salary desc, name asc;
```
### Set Operations
union 并
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
union
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
union, keep all
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
union all
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
intersect 交
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
intersect
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
insersect keep all
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
intersect all
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
except 差
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
except
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
except keep all
```sql
(select course_id
from section
where semester = 'Fall' and year=2009)
intersect all
(select course_id
from sectoin
where semester = 'Spring' and year=2010);
```
null
```sql
x and y =>  取小
x or y =>  取大
not x =>  1-x

assume: null=1/2
=>
  unknown=1/2


since (T,F,null)->(T,F,unknown)
so T and null => unknown
```
### Aggregate Functions
```sql
avg
min
max
sum => input must be numbers
count => input must be numbers
```
#### Basic Aggregation
```sql
select avg(salary)
from instructor
where dept_name = 'Comp.sci';
```
```sql
select count(distinct ID)
from teaches
where semester = 'Spring' and year=2010;
```
#### Aggregation with Grouping
```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name;
```
```sql
select dept_name, count(distinct ID) as instr_count
from instructor natural join teaches
where semester = 'Spring' and year = 2010
group by dept_name;
```
#### having Clause
following ```group by```
```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name
having avg(salary) > 4200;
```
```sql
select course_id, semester, year, sec_id, avg(tot_cred)
from takes natural join student
where year=2009
group by course_id, semester, year, sec_id
having count(ID) >=2;
```
### Modify Database
update some attributes
```sql
update instructor
set salary = salary * 1.5;
```
case
```sql
case 
    when pred1 then result1
    when pred2 then result2
    ...
    when predn them resultn
    else result0
end
```
```sql
update instructor
set salary = 
case
    when salary <= 10000 then salary * 1.05
    else salary * 1.03
end
```
## Chapter 4