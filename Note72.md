# Notes 07/02 Java

## Compiler Information

### What happens when the following is used  

>java.exe HelloWorld

java comes from the JRE which contains the bin and lib folders
those folders contribute to the JVM which utilizes the RAM
within the RAM there is a Stack and a Heap
Stack- a queue of processes on threads (can be multiple stacks)  
    Method Calls   
    Types  
    Primitive Values  
Heap - shared memeory (only ever one heap)  
    Objects - calling on a new object using the new keyword causes a connection between the memory locations in the stack and the heap

## Types

### Primitives

* byte  
* short  
* int  
* long  
* float  
* double  
* bool  
* char  

autoboxing properties  
wrapper classes - reference types that primitives are upgraded to to allow for conversions

### Reference

classes
enum
interfaces
Arrays

## Variables

### Example Class

~~~~Java
public class HelloWorld{
    static int id = 0; // class/static scope
    charh = 'H'; // object/instance scope

    void fizz(){
        int x = 5;
        while(x>0){ // loop/block scope
            double y = 2.25;
        }
    }
}
~~~~

Variable Scopes - how long the variable will survive in code  
Access Modifiers - what can access the files  

* public  
* private  
* default  
* protected

## Types of Programming

Procedural + imperative (default)  
Procedural + Functional (minimize state)  
Object Oriented + Imperative (segregating state)  

### OOP

#### Pillars

Abstraction

* Interfaces
* Abstract classes  

Encapsulation

* Access modifiers
* Scopes

Inheritance

* Don't repeat yourself
* Reusability

Polymorphism

* Class casting
* Method polymorphism
  * overriding
  * overloading

## Abstract Classes and Interfaces

Shared states between classes are prime examples for abstract classes

### Abstract Class Example

~~~~Java
public abstract class Hail{//Abstract Class for Farwell and Greetings class
    protected String name;

    public Hail(){//default constructor required
        super(); //called automatically by the compiler so is not required
    }

    public Hail(String name){
        this.name = name;
    }

    public abstract String toString(); //no logic allows child classes to fill data

}
~~~~

If you declare your own constructor the default constructor will not be called

### Including all requirements in one class

~~~~Java
public class SalutationService{ //including in one class allows for ease of changes later
    Hail hello;
    Hail goodbye;

    public SalutationService(Hail hello, Hail goodbye){
        this.hello = hello;
        this.goodbye = goodbye;
    }

    public String Hello(){
        return hello.toString();
    }

    public String goodbye(){
        return goodbye.toString();
    }
}
~~~~

The above allows for greetings to be changed later on without changing the output functions

### Interfaces

Use implements for integration

~~~~Java
public interface Callable{
    String call();//assumed public
}

public class Tip implements Callable{

    @Override
    public abstract String call();

}
~~~~
