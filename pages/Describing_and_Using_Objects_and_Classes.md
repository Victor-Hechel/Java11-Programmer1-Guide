# Describing and Using Objects and Classes

## Declare and instantiate Java objects, and explain objects' lifecycles (including creation, dereferencing by reassignment, and garbage collection)

The rules around declaring an object are the same of primitive ones, there is only one attention point, if you have not imported the class you must use the Full Qualified Class Name (FQCN), if you made the correct import there are no differences.

Another important aspect of objects is their life cycle. Objects are stored in heap space, which is managed only by the JVM. We, as developers, can not interact directly with heap storage, so deallocation of objects in the heap is made by the garbage collector. When there are no longer variables referencing the object the GC removes it from the heap.

## Define the structure of a Java class

A Java file is composed by (in this order):

- none or one package
- none or many imports
- none or many classes, interfaces or enums

Inside a class we may have:

- initializers
- constructors
- fields
- methods
- other type definitions

These are called "members" and can be "static" or "instance". Here is an example using all of them:

```java
package com.xyz;

import java.util.ArrayList;

public class Garage{

    private static int numberOfGarages;
    private ArrayList<Car> cars;

    // static initializer
    static{
        // executed once class is loaded
        numberOfGarages = 0;
    }

    //instance initializer
    {
        // executed before every new instantiation of Garage
        numberOfGarages++;
    }

    public Garage(){
        cars = new ArrayList<Car>();
    }

    public void addCar(Car car){
        cars.add(car);
    }

    public enum CostTypes{
        normal,
        suv,
        compact
    }
}
```

Another important rule is about the name of the java file. It must equal to the name of the public top-level type. You can have multiple top-level types in the same file, but only one can be public.


## Read or write to object fields

When we have a public instance variable we can access and update its value inside and outside the class directly with no impediments.

```java
public class Person{
    public String name;
    public int age;

    public void toString(){
        // accessing inside class
        System.out.println("Name: " + name + " Age: " + age);
    }
}

public class Main{
    public static void main(String[] args){
        Person p = new Person();

        // accessing outside class
        p.name = "Victor";
        p.age = 21;
    }
}
```

But we have one situation which might cause confusion on beginners. What happens when the following constructor runs?

```java
public class Person{
    public String name;

    public Person(String name){
        name = name;
    }
}
```

This code compiles fine, but it does not work as we wanted to. The local variable "name" will be assigned with the value from itself, when we actually wanted the instance variable to be assigned with the value from the local one. To make it work we should use the keyword "this", which represents the current object where the method is being executed.

```java
public class Person{
    public String name;

    public Person(String name){
        // now the instace variable will be assigned with the value from the local one
        this.name = name;
    }
}
```