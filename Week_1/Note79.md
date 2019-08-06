# Notes 07-09 on SQL

## SQL

Allows data to be persisted where normal programs are volitile

- Relational database system

Frameworks are basic building blocks  

- A "seperate" program that uses the code you wrote in its own way

Libraries are complete collections of methods, classes etc but are unneccessary for the base of the code

### JDBC

Java DataBase Connectivity API  
java.sql

- DriverManager
- Connection
- ResultSet
- Statement

 All require a Postgres Driver to run  
 Translations are handled by the driver such that the database could be changed and the main code would not have to be  
 Driver requires

- Endpoint URL jdbc:postgresql://localhost:5432/postgres
  - jdbc protocol
    - subprotocol postgresql:port
    - dbname
- Username
- Password

#### Maven Integration

Within pom.xml  
dependency  
groupid: org.postgresql  
artifactid: postgresql  
version: 42.2.6

Terminal

> docker run -d --name demodb -p 5432:5432 postgres  
> docker ps  
> docker exec -it demo psql -U postgres  
> create table movies(id serial primary key, title varchar, year integer);  
> insert into movies(title,year) values ('Silence of the Lambs', 1991);  

#### Connections within java code

~~~~Java
String url = "jdbc:postgresql://192.168.99.100:5432/postgres";
String user = "postgres";
String password = "postgres";
String query;

try(Connection connection = DriverManager.getConnection(url, user, password)){
    Scanner scanner = new Scanner(System.in);

    while (true){
        Statement statement = connection.createStatement(); //from connection comes statement
        System.out.print("postgres=> ");
        query = scanner.nextLine();
        if (query.equalsIgnoreCase("exit")); //ignores case sensitivity
            break;
        boolean isResultSet = statement.executeQuery(query); //pass user query to database

        if(isResultSet){
            ResultSet resultSet = statement.getResultSet(); //statement keeps query data
            ResultSetMetaData rsmd = resultSet.getMetaData();
            //print rows
            while(resultSet.next()){ //rows
                for(int i = 1; i <= rsmd.getColumnCount()){ //starts countin at 1 in sql, print columns
                    System.out.println(resultSet.getString(i) + "\t");
                }
                System.out.println(); // automatically inserts newline
            }
        }
    } else {
        System.out.println(statement.getUpdateCount() + "rows affected."); //lets the user know how many rows were changed
    }
}catch (SQLException e){
    System.err.println(ex.getSQLState()); //error handling
}

~~~~
