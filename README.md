###Лабораторная 1
##Цель: Провести оценку производитеьлности с помощью PGBENCH
#Концептуальный вид бд
![alt text](image.png)
#физическая модель:
create table customers(
	customer_id serial primary key,
	name varchar,
	address varchar,
	phone_number varchar
);

create table goods(
	goods_id serial primary key,
	name varchar,
	description varchar,
	retail_price float,
	wholesale_price float
);

create table deals(
	id serial primary key,
	customer_id int references customers.customer_id,
	goods_id int references goods.goods_id,
	quantity int check( quantity > 0),
	date date,
	is_wholesale bool
);