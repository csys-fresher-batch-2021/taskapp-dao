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

##### Task 2.3: Write a method to connect database using Java
* ConnectionUtil

```java
package in.naresh.util;

import java.sql.Connection;
import java.sql.DriverManager;

public class ConnectionUtil {

	private static final String DRIVER_CLASS_NAME = "org.postgresql.Driver";
	private static final String DATABASE_NAME = "shoppingapp_db";
	private static final String DB_USERNAME = "postgres";
	private static final String DB_PASSWORD = "postgres";
	private static final String HOST = "localhost";
	private static final int PORT = 5432;
	private static final String DB_URL = "jdbc:postgresql://" + HOST + ":" + PORT + "/" + DATABASE_NAME; // jdbc:postgres://localhost:5432/shoppingapp_db

	public static void main(String[] args) throws Exception {


		// Step 1: Load the database driver into memory ( ClassNotFoundException )
		Class.forName(DRIVER_CLASS_NAME); 
		
		// Step 2: Get the Database Connection (SQLException)
		Connection connection = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD);
		System.out.println(connection);

	}

}


```
