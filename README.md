# demo-springboot-postgres
A Java application running in Spring Boot framework using Postgres DB on Docker. 
The goal of this project is to simulate one complete cycle from the `Controller Layer > Service Layer > Data Access Layer  > Postgres, and vice versa`. 
See diagram below:

![Demo Cycle Diagram](src/main/resources/book/resources/demo-diagram.png)

### Dependencies:

     * Development Artifacts
        spring-boot-starter-web  : Support HTTP methods such as GET, POST, PUT, DELETE, so on...
        java                     : You may use versions above 8, Im using version 16
        postgresql               : "postgres:alpine" version is used since this is just a light weight app
        flyway-core              : In-charge with migration of data and validation of migration defined at 'V1__Persontable.sql' 
                                     after connection with DB is established.
        spring-jdbc              : Support SQL query, updates, queryforObject methods of Postgres
        HikariCP                 : Loads the configuration 'app:datasource' for Postgres-Docker at application.yml

    * Test Artifacts
        spring-boot-starter-test : Test libraries that has HTTP GET, POST, PUT, DELETE, so on... methods
        assertj-core             : Assertion functions for comparing and checking values
    


## This project supports `CRUD` API for Person. 

### Person

A `Person` has the ff. properties:

     - id: UUID        // This is auto-generated by the system when you ADD a person
     - name: String    // Full name of a person; value must not be null
     - birthday: Date  // Format is yyyy-mm-dd eg. "2020-07-23"
     - email: String   // Email address should be unique and value is not null
     - address: String // Home address of the person

## PROCEDURE:

A. Setting up the Postgres DB on docker.
1. Download the latest docker and install. Make sure installation is succeful.
2. Go to https://hub.docker.com/_/postgres, 
3. Read about the different postgres tags, Dockerfile content, and finally go to section "How to use this image > start a postgres instance".
4. Now, you have to run a container based from **postgres:alpine** image (small size), then expose its host port 5432 (default of postgres) into 5433 of the container. Name this container as "demodb" and set environment variable POSTGRES_PASSWORD to 'password2'. So to do that, open your terminal and type as follows: `docker run --name demodb -e POSTGRES_PASSWORD=password2 -d -p 5433:5432 postgres:alpine`
5. If your command is successful, you should see an ID output in your terminal like this: `082b160cf4b687691e3070acfbbb23f0860ef2e98e2b2ef6865fce399b80a418`
6. To check if it is running properly: `docker ps`. You must see the container demodb is running.


B. Running the app
1. Using IntelliJ IDE, open the project folder and click its pom.xml
1. On the right side of the IDE, open the maven window. Click the beginner-demo > Lifecycle > Install
![Install via maven](src/main/resources/book/resources/maven-install.png)
1. Select the 'target' folder, right click > Open in Terminal. Once you're in, you can now run the app.
   
    `java -jar beginner-demo-0.0.1-SNAPSHOT.jar`
   
1. In Postman, type in URL `localhost:8080/api/v1/person/`, since the path defined at PersonController.java is `@RequestMapping("api/v1/person")`. We can always change this path later on. Select method GET and click send. Check the status if its 200 OK. The method should return all the people that has been inserted (30 records) initially.

Sample Response:
   
       [
           {
           "id": "8a64b7bb-9e24-401a-be2b-1bf79278f416",
           "name": "Samuel Coe",
           "birthday": "2021-04-29",
           "email": "scoe0@ebay.com",
           "address": "97363 Upham Place"
           },
           {
           "id": "f09644dc-f1f6-4430-a077-525b759548fd",
           "name": "Aldus Kik",
           "birthday": null,
           "email": "akik2@patch.com",
           "address": "8347 Springs Circle"
           },
           {
           "id": "501b177c-7d64-40c9-833a-4a090f3bc654",
           "name": "Horatio Willcott",
           "birthday": null,
           "email": "hwillcott3@issuu.com",
           "address": "29 Ryan Terrace"
           },
           {
           "id": "09111bde-9047-4378-be31-885b36b6db0b",
           "name": "Hadley Kasbye",
           "birthday": "2020-07-02",
           "email": "hkasbye4@fotki.com",
           "address": "8750 Hooker Circle"
           },
         ...
       ]

*After this, try other methods (insert, delete, update person, and retrieve person by ID) in the API.*
[CRUD API Documentation](/src/main/resources/book/javadoc/com/example/beginnerdemo/api/PersonController.html)

## Tips:
1. Inside POSTGRES, these commands are useful:
  - List all the databases: \l
  - Describe the table/s: \d
  - Connect to a database: \c
  - Describe all the tables within a DB: \dt
  - Import an SQL statements from a local file to your DB ➜  ~ psql -h localhost -U postgres -d demo_mule4 /Users/scatapan/Documents/SQL-MOCK-DATA/demo_mule4_createt.sql

