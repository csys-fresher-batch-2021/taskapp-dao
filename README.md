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

##### Task 2.4 : Can you convert Connection to reusable method
```java
package in.naresh.util;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConnectionUtil {

	private static final String DRIVER_CLASS_NAME = "org.postgresql.Driver";
	private static final String DATABASE_NAME = "shoppingapp_db";
	private static final String DB_USERNAME = "postgres";
	private static final String DB_PASSWORD = "postgres";
	private static final String HOST = "localhost";
	private static final int PORT = 5432;
	private static final String DB_URL = "jdbc:postgresql://" + HOST + ":" + PORT + "/" + DATABASE_NAME; // jdbc:postgres://localhost:5432/shoppingapp_db

	public static Connection getConnection() {

		Connection connection = null;
		try {
			// Step 1: Load the database driver into memory ( ClassNotFoundException )
			Class.forName(DRIVER_CLASS_NAME);

			// Step 2: Get the Database Connection (SQLException)
			connection = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD);
			System.out.println(connection);
		} catch (ClassNotFoundException | SQLException e) {
			e.printStackTrace();
			throw new RuntimeException("Unable to get the database connection");
		}

		return connection;
	}

}

```

```java
package in.naresh.util;

import java.sql.Connection;

public class ConnectionUtilTest {

	public static void main(String[] args) {

		Connection connection = ConnectionUtil.getConnection();
		System.out.println(connection);
	}

}
```

* Note
```
org.postgresql.jdbc.PgConnection@48e4374
```


##### Task 4: Write Java code to Add Product and save it in database
* ProductDAO
```
package in.naresh.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;

import in.naresh.util.ConnectionUtil;

public class ProductDAO {

	public static void main(String[] args) throws Exception {

		// I want to add product name in database
		
		String productName = "Potato";
		
		
		//Step 1: Get connection
		Connection connection = ConnectionUtil.getConnection();
		
		
		//Step 2: Prepare data
		String sql = "insert into products(name) values ( ? )";
		PreparedStatement pst = connection.prepareStatement(sql);
		pst.setString(1, productName);
		
		//Step 3: Execute Query ( insert/update/delete - call executeUpdate() )
		int rows = pst.executeUpdate(); 
		
		System.out.println("No of rows inserted :" + rows);
		
		
	}

}

```

##### Task 4.2: Can you close the connection resources

```java
package in.naresh.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import in.naresh.util.ConnectionUtil;

public class ProductDAO {

	public static void main(String[] args) throws Exception {

		// I want to add product name in database

		String productName = "Potato";

		// Step 1: Get connection
		Connection connection = null;
		PreparedStatement pst = null;
		try {
			connection = ConnectionUtil.getConnection();

			// Step 2: Prepare data
			String sql = "insert into products(name values ( ? )";
			pst = connection.prepareStatement(sql);
			pst.setString(1, productName);

			// Step 3: Execute Query ( insert/update/delete - call executeUpdate() )
			int rows = pst.executeUpdate();

			System.out.println("No of rows inserted :" + rows);

		} catch (SQLException e) {
			e.printStackTrace();
			throw new Exception("Unable to add product");
		} finally {

			// Null Check - to avoid Null Pointer Exception
			if (pst != null) {
				pst.close();
			}
			if (connection != null) {
				connection.close();
			}
		}

	}

}
```


```
