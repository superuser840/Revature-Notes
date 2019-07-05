# Notes for 07-03

## Eclipse

Eclipse marketplace to change perspectives via plugins

## Java Libraries

### ArrayList

Implementation of the List is based on Arrays

~~~~Java
import java.util.ArrayList;
public class CollectionDemo{
    public static void main(String[] args){
        ArrayList<Person> persons = new ArrayList<>();
        persons.add(new Person("Bob", "Builder", 40)); //adds information to the list
        persons.add(new Person("Bruce", "Wayne", 85));

        System.out.println(persons.toString());//toString isn't necessary because Java will automatically use if there are no arguements ie. System.out.println(persons);

        for (Person p : persons){//for each loop to print list
            System.out.println(p);
        }
    }
}


class Person{
    private String firstName;
    private String lastName;
    private int age;

    public Person(String firstName, String lastName, int age){
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    @Override
    public String toString(){//ovverride needed for toString function
        return ...
    }
}
~~~~

To sort lists by desired field

~~~~Java
//in main
TreeSet<Person> persons = new TreeSet<>();

//altered class

class Person implements Comparable<Person>{
    ...
    @Override
    public int compareTo(Person o){
        return this.age - o.age; //reverse order of the dash to flip order
    }
}
~~~~

Third class to compare via Last Name

~~~~Java

class PersonLastNameComparator implements Comparator<Person>{

    public int compare(Person o1, Person o2){
        return o1.lastName.compareTo(o2.lastName);
    }
}

//use in main
TreeSet<Person> persons = new TreeSet<>(new PersonLastNameComparator());
//lambda function for reduction of code
Treeset<Person> personsLambda = new TreeSet<>( (o1,o2) -> o1.firstName.compareTo(o2.firstName)); //removes the need for outside classes in single use case

~~~~

### Types of Collections

- Sets are lists of unique inputs  
- Queues are first in first out  
- Deques are double ended Queues where it is either first in first out or last in last out  
- Maps have a list of key values that map to a value somewhere else in the data
  - keys can be strings/null etc  
  - HashTables are synchonized to prevent errors in multithreading
  - HashMaps are HashTables without sysnchonization to reduce processing requirements

Variables within interfaces can be created but they require the final keyword that makes them read-only

### String Properties

String is immutable, it doesn't change and rather creates new strings in memory (final)  
Previous references are saved in the memory StringPool and are not recreated if referenced again

- StringBuffer
  - synchronized Builder for multithreading
- StringBuilder
  - a mutable version of String(won't save garbage data)
- Substring  
- Length
- toupper

### Exceptions

Checked Exceptions are noticed by the JVM at compile time

- File Not Found
- IO exception
- SQL exception

Unchecked Exceptions/Runtime Exception

- Array Index out of bounds

#### Solve Run-time Exceptions

Try-Catch-Finally to prevent closure of program in the event of exception  
Duck the exception with throws

~~~~Java

try{
    ...
} catch(IOException ex){
    ...
} catch(Exception){ //only the first block that matches will be ran
    ...
} finally { //block of code that always runs regardless of each catch block
    ...
}

~~~~

#### Non-Catchable

Error are unrecoverable - JVM implodes

- Stack Overflow
- Out of Memory

### IO

Input Stream/ Output Stream wrapped in the Reader/Writer which is then wrapped in the Buffered- which is wrapped in the Scanner  
Scanner allows IO to be used without complex try catch blocks

~~~~Java

//Server Demo Main hard way
{
    ServerSocket server;
    try{
        server = new ServerSocket(8080);
    } catch(IOException ex){

    } finally{
        try{
            server.close();
        } catch(IOException e){
            e.printStackTrace();
        }
    }
}
~~~~

Above is needlessly complicated

~~~~Java

//Server Demo Main Easy Way
{
    try(ServerSocket server = new ServerSocket(8080)){
        while(true){
            try(Socket socket = server.accept(); //try blocks can be multiple lines
                OutputStream os = socket.getOutputStream()){
            //InputStreamReader isr = new InputStreamReader(socket.getInputStream());
            Bufferedreader br = new BufferedReader(new InputStreamReader(socket.getInputStream())); //Buffered Example
            String line = br.readLine();
            while(!line.isEmpty()){ //while line is not empty
                System.out.println(line);
                line = br.readLine();
                }
            String response = "HTTP/1.1 200 OK/r/nContent-Type: application/json\r\n\r\n{'name' : 'Mehrab'}"
            os.write(response.getBytes("UTF-8"));
            }
        }
    } catch (IOException ex){

    }
}

~~~~
