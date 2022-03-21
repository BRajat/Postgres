## POSTGRES

Database - Storage for applications.

### Relational Database

Have relationship between tables.

### Postgres -

Object-relational database management 
modern
open-source

Mac-users - with Postgres app
add psql to env var (PATH)

To remove ubuntu installation-
https://askubuntu.com/questions/32730/how-to-remove-postgres-from-my-installation


`\?` --> opens help

`\c [ DB_NAME ] `--> use data base with DB_NAME

`\d [ TABLE_NAME ] `--> describes table

`\d`  --> show list of tables

`\i` file_path  --> to execute sql fro file

`\x` --> turns on expanded display

CREATE TABLE <TABLE_NAME> ( 

	<attribute> + <data-type> + <constraints
	
);

Example:
`CREATE TABLE person (
			id BIGSERIAL NOT NULL PRIMARY KEY,
			first_name VARCHAR(50) NOT NULL,
			last_name VARCHAR(50)  NOT NULL,
			age INT  NOT NULL,
			email VARCHAR(100) ,
			country_of_birth VARCHAR(80) NOT NULL 
		     );
`

**ORDER BY** 
`Select * from person order by age;`

Order by - 
`select * from table_name order by FIELD_1 [ASC|DESC] [ FIELD_2 [ASC|DESC] ... ]`

### Arithmetic and comparision operators-

`select * from person where name = 'XYZ';`

`select * from person where age > 20;`

`select * from person where age > 20 and age < 40;`

`select * from person where age > 20 and country_of_birth = 'India';`

**IN**

`select * from person where country_of_birth in ['US','UK','Australia'];`

**LIKE**

`select * from person where email like '%bloomberg.com'`

% -> any characters 

`select * from person where name like '%ete% ; `

peter, jeter, meter

`select * from person where country ILIKE 'p%';`

=> Above is example of case insensitive matching with ILIKE


**GROUP BY**
(Group by Attribute and Aggregate over it)

`select count(*) from person GROUP BY country_of_birth;`

`select count(country_of_birth) from person GROUP BY country_of_birth;`


**HAVING**

`select count(*) from person GROUP BY country_of_birth having count(*) > 10;`

`select count(*) from person GROUP BY country_of_birth having count(*) > 10 and count(* < 100;`

**AGGREGATE FUNCTIONS**

sum()
count()
min()
max() 

`select max(age) from person;`

Consider an example of CAR table that has make, price-

`select make, max(price) from car group by make;`

[ Once we use group by the number of rows in resulting table are reduced to the number of distinct values of group by field, the aggregation function can then be used to non-grouped field. ]

**BASIC ARITHEMATIC OPERATIONS**

`select (10+2);`

`select (10-6);`

`select (5!) ;  `   FACTORIAL operation

`select (10 % 3); ` MODULO operation
 
`select (10**2);`

`select Round(price, 2) from car` --> rounded to 2 decimal places

----

`select make, model, price, Round(price * 0.9 , 2) as discounted_price from car;`


----
**COALESCE**

`select COALESCE(1) as number; `     => 1

`select COALESCE(null,1) as number;` => 1

`select COALESCE(null, null, 1) as number;`  => 1

`select COALESCE(null, null, null, 1) as number;`  => 1

returns first non-null value

--
`select COALESCE(email, 'email not provided')` from person;

sets the default value for null values in a attribute.

-----
**NULLIF**

`select 10/0; `  => Division by zero error

`select NULLIF(10/0);` => no error

`select 10 / NULL;` => no error

----
**TIMESTAMPS AND DATE**

TIMESTAMP - combination of date(date-month-year), time(hour-min-sec-ms)

`select NOW()::DATE;`   => 2022-03-21

`select NOW()::TIME; `  => time in default format

check documentation for details

----
**Adding and subtracting date and time**

`select NOW() - INTERVAL '1 YEAR'` => 2021-03-21  (RETURNS TIMESTAMP)

`select NOW() - INTERVAL '10 YEARS'` => 2012-03-21 (RETURNS TIMESTAMP)

`select NOW() - INTERVAL '10 MONTHS'` => 2021-05-21 (RETURNS TIMESTAMP)

`select NOW() + INTERVAL '10 DAY';` => 2022-03-31

Casting timestamp data to date data

`select (NOW() + INTERVAL '2 YEARS')::DATE ;` => r-eturns date output

---
**EXTRACTING FIELDS**

`select EXTRACT(YEAR FROM NOW());`

---
**AGE FUNCTION**

adding age column with help of date of birth

`select Name, AGE(NOW(), date_of_birth) as AGE from person;`
=> returns age in year, month and days
Extract individual field as required with Extract function.

----
**PRIMARY KEY**

FIELD/Attribute that uniquely identifies a record(row) in our Database Table.

Trying to insert record with same primary key value, will result in Duplicate Key Error.

Droping constraint
`Alter table person drop CONSTRAINT person_pkey;`

Now, inserting the duplicate records will not give error.

Primary key important to avoid duplication of records.
----

`Alter table person add primary key (id);`

Cannot create this constraint, when table have duplicate record. FIrst remove duplicate records and then add primary key.

----
**UNIQUE KEY**

`select email, count(*) from person group by email;`

-Q - to find if emails are duplicate?

`alter table add constrain unique_email UNIQUE (email);`

Again verify if table has no duplicate emails before altering.

`delete from person where id = 1004;`

Then,
`Alter table add constrain unique_email UNIQUE (email);`
	(OR)
`Alter table add Constraint UNIQUE(email);	`

To drop Constraint;
`Alter table person Drop Constrain unique_email;`

----
**CHECK CONSTRAINT**

select distinct gender from person;

GENDER - Male, Hello, Female

Alter Table person ADD COnstrain gender_constraint check (gender = 'Female' or gender = 'Male')

This will check the gender_constraint whenever new record is added to the table.
----

**DELETE RECORDS**

`delete from person;`  => deletes all record from person

`delete from person where gender = 'Female' and country = 'India';`

**USE WITH CAUTION : DELETE FROM TABLE_NAME**

-----
**UPDATE RECORDS**

`Update person SET email = 'abc@gmail.com';` => updates all record

`Update person SET email = 'abc@gmail.com' where id = 2001;` => udpate single record

`update person SET first_name = 'abc', last_name = 'zyx' where id = 2001;`

Always use where clause or you might update entire table.

----
**INSERT RECORDS**
`
insert into person (field 1, field 2, ... ) Values (value1, value2, ... )
`
<br>

`
insert into person (field 1, field 2, ... ) 
Values (value1, value2, ... )
ON CONFLICT (id) DO NOTHING;
`

On conflict works on unique field only.
`
insert into person (field 1, field 2, ... ) 
Values (value1, value2, ... )
ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email;
`

It will update the old_record incase the id conflicts, without invoking any errors.

----
**FOREIGN KEY, JOINS and RELATIONS**

We cannot have a table that have all data of all entity in only a single table. Reason, the table will be very big, lot of field values might be null, and to read such a table is very difficult.

Instead, we break the table into Entity - relationships

Person <-> Car
A Person can have 0,1 or more car
Every car belongs to only 1 person

Adding foreign key in table - 

`
Create table car(
id bigint primary key
);

CREate table person (
person_id bigint not null primary key,
car_id bigint references car(id)
);
`

-- 
Updating (Adding car_id for persons)
 
`Update person SET car_id = 2 where person_id = 1;`

`Update person SET car_id =99 where person_id = 2;`

Since, car_id=99 not present in car table, this will not work. Field value must be populated as primary key, before getting used as foreign key.

The foreign key can be null if not constrained.

----
**JOIN**

select * from person
join car
on person.car_id = car.id

**selecting custom fields from 2 tables**

select person.name, car.make 
from person
join car
on person.car_id = car.id

**LEFT JOINS**

All from left table and only matching from right table

person <-> car
All person with/without car are outputed
with car are output with non-null value
without car are output with null value

**RIGHT JOIN**

All from right table and only matching from left table
person <-> car
All cars are outputted with person
(This will not have null person value since
Every car has 1 person)

**INNER JOIN** default join

All matching from left and right table


**OUTER JOIN** also called Full outer join

All from Left and Right table

-----
While deleting - first delete from table where the field is foreign key and only then delete record from table where it is primary key. The reverse will invoke error.

---- 
**EXPORT CSV**

`\copy (select * from table_name)  TO 'path/to/file' Delimiter ',' CSV HEADER;`

----
**BIG SERIAL**

`select * from person_id_seq;`

`select * from person;`

`select nextval('person_id_seq'::regclass);`  => can skip sequence number of id field. iterate over sequences

Alter sequence person_id_seq restart with 10;
----
**POSTGRES WITH EXTENSIONS**

select * from pg_available_extensions

----
**UUID - Universally unique identifier**

Combination of pc_address and timestamp - check wiki

CREATE EXTENSION IF NOT EXISTS 'uuid-ossp'

`select uuid.generate_v4();` => randomly generate a uuid

We can use uuid as primary key for Database, since they are globally unique, we can migrate data without much problems.

`
create table car (
car_uid uuid not null primary key,
make varchar(50) not null,
model varchar(50) not null
);

create table person(
person_uid uuid not null primary key,
first_name varchar(50) not null
car_id uid refrences car(car_uid)
);
`

`inser into car (car_uid, make, model) values (uuid.generate_v4(), 'make1', 'model1');`

`insert into persone (person_uid, first_name) values (uuid_generate_v4(), 'name1');`

`update person set car_uid = 'car_uid_code' where person_uid = 'person_uid_code';` 


------
Course complete
------

Next - Learn how to connect databases with application.

