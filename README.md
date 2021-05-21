##### Task App 

##### Task 1: Create Table products
* products ( id,name)

```sql
create table products ( id serial primary key , name varchar(100) not null,
unique (name)
);
select * from products;

insert into products ( name) values ( 'Tomoto');
insert into products ( name) values ( 'Onion');
```

