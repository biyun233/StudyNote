# Introduction

```sql
show databases;
create database testsql;
use testsql;
show tables;
drop database testsql; -- delete database
```



# Connection

```sql
mysql -u {username} -p
mysql -h 192.168.10.121 -P 3306 -u root -p
```



# Data Definition Language 

### Numeric Data Types

`INT` : whole numbers

`FLOAT(M, D)` : Decimal numbers (approximate)

```sql
-- M: length of the number
-- D: length of the nomber go after the decimal point
-- float(3, 2) : 1.78    float(4, 2) : 10.25
```

`DECIMAL(M, D)` : Decimal numbers (precise)

### Non-Numeric Data  Types

`CHAR(N)` : Fixed length character

`VARCHAR(N)` : Varying length character

`ENUM('M', 'F')` : Value from a defined list

`BOOLEAN` : True or False values



### Primary Keys

1. A primary key is a column, or a set of columns , which uniquely identifies a record within a table
2. A primary key must be unique
3. A primary key cannot be NULL
4. A table can only have one primary key

### Foreign Keys

1. A foreign key is used to link two tables together
2. A foreign key is a column whose values match the value of another tables primary key column
3. The table with the primary key is called the reference, or parent table and the table with the foreign key is called the child table
4. A table can have multiple foreign keys

### Creating the Coffee Store Database

```sql
create database coffee_store;
use coffee_store;

create table products(
	id int auto_increment primary key,
    name varchar(30),
    price decimal(3,2)
);

create table customers(
	id int auto_increment primary key,
    first_name varchar(30),
    last_name varchar(30),
    gender enum('M', 'F'),
    phone_number varchar(11)
);

create table orders (
	id int auto_increment primary key,
    product_id int,
    customer_id int,
    order_time datetime,
    foreign key (product_id) references products(id),
    foreign key (customer_id) references customers(id)
);
show tables;
```

### Modifying Tables: Adding and Removing Columns

```sql
alter table products
add column coffee_origin varchar(30);

alter table products 
drop column coffee_origin;
```

### Renaming Tables

```sql
rename table category to categories;
```

### Deleting Tables

```sql
drop table test;
```

### Truncating Tables

```sql
truncate table test;
-- delete the data of table
```

# More On Alter Table

### Add and Remove Primary Key

```sql
describe addresses;

alter table addresses
add primary key (id);

alter table addresses
drop primary key;
```

### Add and Remove Foreign Key

```sql
alter table people
add constraint FK
foreign key (address_id) references addresses(id);
-- addresses(id) must be primary key

alter table people
drop foreign key FK;
```

### Add unique Constraint

```sql
alter table pets
add constraint u_species unique (species);

alter table pets
drop index u_species;
```

### Change Column Name

```sql
alter table pets change `species` `animal_type` varchar(20);
```

### Change Column Data Type

```sql
alter table addresses modify city varchar(30);
```

# Data Manipulation Language

### Inserting Data Into Tables

```sql
insert into products(name, price, coffee_origin)
values ('Espresso', 2.50, 'Brazil');

insert into products(name, price, coffee_origin)
values ('Latte', 3.50, 'Indonesia'), 
	     ('Americano', 3.00, 'Brazil'),
       ('Flat White', 3.00, 'Indonesia'),
       ('Filter', 3.00, 'India');
```

### Updating Data in Tables

```sql
update products
set coffee_origin = 'Sri Lanka'
where id = 7;
```

### Deleting Data from Tables

```sql
delete from products
where name = 'Filter';

delete from products -- delete all data
```

# Selecting from a Table

### Select Statement

```sql
select * from customers;

select last_name, phone_number from customers;
```

### Where Clause

```sql
select * from products
where coffee_origin = 'Colombia'
and price = 3.00;
```

### Null Value

```sql
select * from customers
where phone_number is null;
```

### In, Not In (IMPORTANT)

```sql
select * from customers
where last_name in ('Taylor', 'Bluth', 'Armstrong');

select * from customers
where first_name not in ('Katie', 'John', 'George');
```

```sql
-- my solution
select * from orders
where order_time between '2017-02-01' and now() 
and
customer_id % 2 = 0 
and 
customer_id between 2 and 8;

-- better solution
select * from orders
where order_time between '2017-02-01' and now() 
and customer_id in (2, 4, 6, 8);
```

### Between

```sql
select product_id, customer_id, order_time from orders
where order_time between '2017-01-01' and '2017-01-07';
```

### Like

```sql
-- % : any number of characters
-- _ : just one character

select * from customers
where last_name like 'W%';
-- any last name start with 'W'
```

### Order By

```sql
select * from products
order by price desc;
-- asc, desc
```

### Distinct

select different values of data

```sql
select coffee_origin from products;
/*
'Colombia'
'Colombia'
'Costa Rica'
'Indonesia'
'Colombia'
'Indonesia'
'Sri Lanka'
*/

select distinct coffee_origin from products;
/*
'Colombia'
'Costa Rica'
'Indonesia'
'Sri Lanka'
*/
```

### Limit

```sql
select * from customers
limit 10 offset 5;
-- 5 is not included
```

### Column Name Alias

​		rename the column name of result

```sql
select name as coffee, price, coffee_origin as country from products;
```

# Selecting From Multiple Tables

Joins allow you to retrieve data from multiple tables in a single select statement. To join two tables there need to be a related column between them.

### Inner Join

will retrieve data only when there is matching values in both tables.

![mysql1](img/mysql1.jpeg)

```sql
select products.name, orders.order_time from orders
inner join products on orders.product_id = products.id;

-- shorthand
select p.name, o.order_time from orders o
join products p on o.product_id = p.id
where o.product_id = 5
order by o.order_time;
```

### Left Join

will retrieve all data from the left table (table 1) and matching rows from the right table (table 2).

![mysql2](img/mysql2.jpeg)

```sql
select o.id, c.phone_number, c.last_name, o.order_time from orders o
left join customers c on o.customer_id = c.id
order by o.order_time
limit 10;
```

### Right Join

![mysql3](img/mysql3.jpeg)

### Joining more than two tables

```sql
select p.name, p.price, c.first_name, c.last_name, o.order_time from products p
join orders o on p.id = o.product_id
join customers c on o.customer_id = c.id;
```

# Database Design

### Normalization

![mysql4](img/mysql4.jpeg)

avoid storing repeated data in different tables.

Benefirts:

1. reduce storage space
2. reduce insert, update and deletion anomalies
3. improve query performance

![mysql5](img/mysql5.jpeg)

### 1st Normal Form

![mysql6](img/mysql6.jpeg)

![mysql7](img/mysql7.jpeg)

![mysql8](img/mysql8.jpeg)

### 2nd Normal Form

![mysql9](img/mysql9.jpeg)

(Not 2NF)

![mysql10](img/mysql10.jpeg)

The whole of primary key is student + subject, the AGE column is dependent on the student but not the subject.

(2NF)

![mysql11](img/mysql11.jpeg)

![mysql12](img/mysql12.jpeg)

### 3rd Normal Form

![mysql13](img/mysql13.jpeg)

(Not 3NF)

![mysql14](img/mysql14.jpeg)

the star pupil date of birth also depend on star pupil

(3NF)

![mysql15](img/mysql15.jpeg)

![mysql16](img/mysql16.jpeg)



### Relationship

![mysql17](img/mysql17.jpeg)

### One to One

a key to one table appears no more than once as the key in another table and vice versa (反之亦然)。

Ex: A department can only have one head of department, a head of department can not be head of another department.

![mysql18](img/mysql18.jpeg)

### One to Many

a primary key of one table can be in multiple rows of a foreign key column of another table

### Many to Many

two tables can have instances of each other

![mysql19](img/mysql19-3419173.jpeg)

Filter coffee can be in many orders, a order could contains many kinds of coffee

### Constraints



![mysql20](img/mysql20.jpeg)

![mysql21](img/mysql21.jpeg)

# Aggregate Functions

1. perform a calculation on data within a column and returns on result row.
2. can use GROUP BY clauses to group the results by one (or more) columns.
3. can use a HAVING clause in a similar way to a WHERE clause in a SELECT statement to filter the results set.

### Count

```sql
select count(*) from customers;
-- count rows
```

### Sum

```sql
select sum(no_seats) from rooms;
-- add values in a table or a column
```

### Min and Max

```sql
select max(length_min) from films;
```

### Average

```sql
select avg(length_min) from films;
```

### Grouping Data

```sql
select customer_id, count(id) from bookings
group by customer_id;
-- count the number of bookings for each customer
```

any column in the select statement which isn't being aggregated over must appear in the GROUP BY clause.

### Having Clause

HAVING clauses acts like a WHERE clause in a select statement but for GROUP BY statement.

```sql
select customer_id, screening_id, count(id) from bookings
group by customer_id, screening_id
having customer_id = 10;
```

# Subqueries

### Non-Correlated Subquery

The inner query can run independently of the outer query.

![mysql22](img/mysql22.jpeg)

Inner query runs first and produces a result set, which is then used by the outer query.

```sql
select max(no_seats) from
(select booking_id, count(seat_id) as no_seats from reserved_seat
group by booking_id) b;
```

### Correlated Subquery

The inner query can't run independently of the outer query.

![mysql23](img/mysql23.jpeg)

The inner query runs for every row in the outer query.

# Mysql Functions

### Concatenation

```sql
select concat(first_name,' ', last_name) as full_name from customers;
```

### Substrings

`substring(string, start, length)`

```sql
select substring('Example', 3, 3) as sub;
-- amp
```

### Upper and Lower case

turns the data in upper/lower case

```sql
select upper(name) from rooms;
/*
'CHAPLIN'
'KUBRICK'
'COPPOLA'
*/
```

### Date Function

```sql
select date('2018-06-05 07:45:32');
-- 2018-06-05

select SUBDATE("2017-06-15", 1);
-- 2017-06-14

select product_id, customer_id, order_time from orders
where date(order_time) between '2017-01-01' and '2017-01-07';
-- 2017-01-07 is included

select product_id, customer_id, order_time from orders
where order_time between '2017-01-01' and '2017-01-07';
-- 2017-01-07 is not included, 2017-01-07 is considered as '2017-01-07 00:00:00'
-- but order_time on 2017-01-07 is after 00:00:00
```

### Month Function

```sql
select month('2018-06-05 07:45:32');
-- 6

-- same as year
```



# Performance Tune

### Use MySQL EXPLAIN for Query Optimization

```sql
explain select * from outbound_info where proxy = '佳景' \G
# explain just show the query plan
```

![mysql24](img/mysql24.jpeg)

**type: ALL** ->

​	 The output has the access type set to ALL which is the most basic access type because it scans all rows for the table,

it is also the most expensive one. 

​	It means 176 rows will be examined and for each row a WHERE clause will be applied.

**Filtered: 10.00** -> 

​	10% of the rows examined will match the workloads.

```sql
explain analyze select * from outbound_info \G
# explain analyze not only show the query plan, but also execute the query
```

![mysql25](img/mysql25.jpeg)

```sql
explain format=json select * from outbound_info where proxy = '佳景' \G
```

![mysql26](img/mysql26.jpeg)

### Composite Index

#### Index Commands

```sql
show index from outbound_info;
create index public_id_idx on outbound_info (public_id);
drop index `public_id_idx` on outbound_info;
```



```sql
SELECT count(*) FROM userinfo WHERE name = 'John' AND state_id = 100;
```

当where涉及到多个column，composite index 比index individual 更快捷。![mysql27](img/mysql27.jpeg)

```sql
alter table userinfo add index name_state_id_idx(name, state_id);
```



### Redundant Indexes 多余的

![mysql28](img/mysql28.jpeg)

![mysql29](img/mysql29.jpeg)

保留两个index让Q1和Q2都保持了高性能，即使index1是redundant index，我们也考虑保留。

#### Examples

1. No index

![mysql30](img/mysql30.jpeg)

2. Index (public_id)![mysql31](img/mysql31.jpeg)

  3. Index (public_id, outbound_no)

     ![mysql32](img/mysql32.jpeg)

![mysql33](img/mysql33.jpeg)

但是index越多，insert和delete的性能也会越低。

```sql
select * from sys.schema_redundant_indexes
```

![mysql34](img/mysql34.jpeg)

#### Extra Remarks

Idx(A) = Idx(A, id)

Idx(A, B) = Idx(A, B, id)



### Benifits Of Secondary Index

1. reduce the rows examined
2. sort data
3. validate data
4. avoid reading data
5. find min/max values



### Tips Of Performance

1. Avoid unnecessary columns in SELECT clause
2. Indexing of all the predicates(WHERE, JOIN, ORDER BY, GROUP BY)
3. Avoid using functions in predicates
4. Avoid using a wildcard(%) at the beginning for LIKE
5. Limit using of Outer join, Distinct, Union. (Inter join / Union All instead)
6. Use ORDER BY if mandatory
7. Don't periodically check and update table using multiple threads. (prefer Redis as persistent queue instead)
8. Use 'next page' instead of Limit and offset
9. Use a join instead of subqueries
10. Use mysql query cache
11. Use Memcached for mysql caching



### Statement Of Truncate all tables

```sql
SELECT CONCAT('TRUNCATE TABLE ',TABLE_NAME,';') AS a 
FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'db' ;
```



### Mysqldump

```sh
--column-statistics=0 --default-character-set=utf8 # 特殊字符
```

### Privilege

```sh
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO 'new_user'@'localhost';
FLUSH PRIVILEGES;

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
```

