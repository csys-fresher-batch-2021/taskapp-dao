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

##### Task 2: Use Java, to add products

##### Task 2.1: Get the Database Connection Details
* database - postgres database
* machine - localhost, port: 5432
* username - postgres
* password - postgres
* database name - shoppingapp_db

#### Task 2.2:  Create a Maven Project 

* groupId : in.naresh
* artifactId : shoppingapp-dao
* pom.xml
```
<groupId>in.naresh</groupId>
  <artifactId>shoppingapp-dao</artifactId>
  <version>0.0.1-SNAPSHOT</version>
```

##### Task 2.3: Add postgres dependency
* https://mvnrepository.com/artifact/org.postgresql/postgresql/42.2.20

```
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.2.20</version>
</dependency>
```

```
