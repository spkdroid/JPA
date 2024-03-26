# JPA

Creating a JPA application with a PostgreSQL database involves several steps, from setting up your project environment to writing entity classes and performing CRUD operations. Below is a step-by-step guide to creating a simple JPA application using PostgreSQL.

### Step 1: Setup Dependencies Project

Create a new Maven project in your IDE. Edit the `pom.xml` file to add dependencies for JPA and the PostgreSQL JDBC driver:

```xml
<dependencies>
    <!-- JPA API -->
    <dependency>
        <groupId>jakarta.persistence</groupId>
        <artifactId>jakarta.persistence-api</artifactId>
        <version>2.2.3</version>
    </dependency>
    <!-- Hibernate - JPA Implementation -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>5.4.21.Final</version>
    </dependency>
    <!-- PostgreSQL JDBC Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.18</version>
    </dependency>
</dependencies>
```

Create a file named build.gradle in the root of your project directory and add the following content:
```
groovy
Copy code
plugins {
    id 'java'
}

group 'com.example'
version '1.0-SNAPSHOT'

sourceCompatibility = '1.8'

repositories {
    mavenCentral()
}

dependencies {
    // JPA API
    implementation 'jakarta.persistence:jakarta.persistence-api:2.2.3'
    // Hibernate - JPA Implementation
    implementation 'org.hibernate:hibernate-core:5.4.21.Final'
    // PostgreSQL JDBC Driver
    runtimeOnly 'org.postgresql:postgresql:42.2.18'
}
This script configures your project to use Java, specifies the Maven Central repository for fetching dependencies, and adds the necessary dependencies for JPA, Hibernate, and the PostgreSQL JDBC driver.
```

### Step 2: Configure JPA

Create a `persistence.xml` configuration file in the `src/main/resources/META-INF` directory. This file configures the persistence unit and database connection properties:

```xml
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.2">
    <persistence-unit name="jpa-example" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
        <properties>
            <property name="javax.persistence.jdbc.driver" value="org.postgresql.Driver" />
            <property name="javax.persistence.jdbc.url" value="jdbc:postgresql://localhost:5432/yourdatabase" />
            <property name="javax.persistence.jdbc.user" value="yourusername" />
            <property name="javax.persistence.jdbc.password" value="yourpassword" />
            <property name="hibernate.dialect" value="org.hibernate.dialect.PostgreSQLDialect" />
            <property name="hibernate.hbm2ddl.auto" value="update" />
            <property name="hibernate.show_sql" value="true" />
            <property name="hibernate.format_sql" value="true" />
        </properties>
    </persistence-unit>
</persistence>
```

Replace `yourdatabase`, `yourusername`, and `yourpassword` with your PostgreSQL database name, username, and password.

### Step 3: Define an Entity Class

Create a simple entity class named `Person`:

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Constructor, Getters, and Setters
    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    // standard getters and setters
}
```

### Step 4: Perform CRUD Operations

Create a `Main` class to test CRUD operations:

```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;

public class Main {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpa-example");
        EntityManager em = emf.createEntityManager();

        // Create a new person
        Person person = new Person("John Doe");

        // Begin transaction
        em.getTransaction().begin();
        
        // Persist the person object
        em.persist(person);
        
        // Commit transaction
        em.getTransaction().commit();

        // Close the EntityManager and EntityManagerFactory
        em.close();
        emf.close();
    }
}
```

### Step 5: Run Your Application

Before running your application, ensure your PostgreSQL server is running and the database specified in `persistence.xml` exists.

- Run the `Main` class in your IDE

using Maven: mvn exec:java -Dexec.mainClass=Main. <br>
using Gradle: ./gradle build
