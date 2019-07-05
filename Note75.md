# Notes for 7-5

## Reflection

In runtime the program has to run in the order created in compile time and if there was a difference it throws an error  
Generics are placeholders put in compile time that can change based on input  

Reflection can look at and change private variables

~~~~Java

public class ReflectionDemo{
    public static void main(String[] args){
        Dude dude = new Dude("Bob", 40);
        System.out.println(dude.getClass().getName()); //getClass() comes from the reflection api and gets realtime info from the glass
        System.out.println(Arrays.toString(dude.getClass().getDeclaredFields()));//prints private variables from class
}

~~~~

### Invocation

Framework allows for invocation of methods from the class

~~~~Java

Method m;
        try{
            m = dude.getClass().getMethod("birthday", new Class <?>[0]); //(name of method, parameter types) parameters are indexed for overloaded methods
            m.invoke(dude);//calls the dude method
        } catch(...){
            e.printStackTrace();
        }
    }

~~~~

### Modification

Framework can modify the private variables of the class

~~~~Java

Dude joe = new Dude("Joe", 30);
try{
    Class<?> c = Class.forName(joe.getClass().getName());
    Field f = c.getDeclaredField("age");
    f.setAccessable(true);//allows the variable to be accessed by the api
    f.set(joe, -1000);//sets Joe's age to -1000
}catch (ClassNotFoundException e ...){
    e.printStackTrace();
}

~~~~

## Threads

Threading is more important for networking instead of processing

> Trigger: Start
> Process: Run

~~~~Java
class CustomThread extends Thread{
    public void run(){
        System.out.println("Custom Thread");
    }
}
new CustomThread().start(); //in main

class CusomRunnable implements Runnable{ //allows threads using interfaces
    public void run(){
        System.out.println("Custom Runnable");
    }
}
new Thread(new CustomRunnable()).start(); // in main, starts thread using the class while using the custom interfaces

~~~~

### Anonymous solution

~~~~Java

new Thread(new Runnable(){
    public void run(){
        System.out.println("Anonymous Runnable");
    }
});

~~~~

### Lambda solution

~~~~Java

new Thread ( () -> System.out.println("Lambda Runnable")).start();

~~~~

Resource management is the most difficult part of threading and can lead to "thread death"  
threads run until the synchronized resource would be called
Deadlock

- thread one using resource and needs second resource used by thread two
- Thread two is doing the same thing so both are stuck

### Synchonization

~~~~Java

private int counter;

public void run(){
    synchronized (this){
        for ( int i = 0; i < 25; i++)
            System.out.println("Custom Thread" = counter++);
    }
}
//in main
CustomThread ct = new CustomThread(0);
ct.start();
synchronized(ct){
    try{
        ct.wait();
    } catch (InterruptedException ex){

    }
}

~~~~

Synchronized allows for threads to wait for resources instead of just skipping over them and throwing exceptions.  
volitile keyword can be used to combat this problem as well

## Unit Testing

Test Driven Development -program the tests first and program around them to make it work

### Test Api

~~~~Java

public class ApiService{
    private String value;

    public String get(){
        return value;
    }
    public String set(String value){
        this.value = value;
        return this.value;
    }
    public String scannerSet(Scanner sc){
        this.value = sc.nextLine();
        return this.value;
    }
    public void error() throws IOException{
        throw new IOException();
    }
}

//in main, standard testing way
ApiService api = new ApiService();
String expected = api.set("value 1");
if (expected.equals("value 1")){
    System.out.println("Test Passed")
} else
    System.out.println("Test Failed");
~~~~

### Junit

Create a class in the test files from maven

~~~~Java

ApiService api;
@Before //function will run before everything else
public void setup(){
    api = new ApiService();
}
@Test
public void setTest(){
    String expectedValue = "testValue";
    String actualValue = api.set("testValue");
    assertEquals(expectedValue, actualValue);
}
@Test
public void givenScannerThenScannerSetReturnsScannerLine(){
    Scanner sc = new Scanner("value");
    String expected = "value";
    String actual = api.scannerSet(sc);
    assertEquals(expected, actual);
}
@Test (expected = IOException.class) //allows for a negative result to be a passing test
public void whenErrorIsCalled() throws IOException{
    api.error();
}

~~~~
