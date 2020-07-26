# Creating and Using Methods

## Create methods and constructors with arguments and return values

The Java's method declaration is way more robust than Python's one or Javascript's functions, since we need to inform the type of the parameters and return, there are plenty of details because of this and other characteristics from the language that will be shown along this chapter. A method in Java is composed by:

- **Accesibility(optional):** public, private, protected (default is public).
- **Static modifier(optional):** defines if it is a method from the class or from the object instance.
- **Final modifier(optional):** defines that the method can not be overridden.
- **Return type(required):** type that the client will receive calling this method. Use *"void"* when it does not return anything.
- **Name(required):** method's name.
- **Parameters(optional):** the paramater is the junction of a type and a variable name that will be used inside the method, it is like a variable declaration but each parameter must have its type declared individually. It is also possible to use the **final** modifier here.

Here are some examples of the use of methods.

```java
public void printNameAndAge(String name, int age){
    // even when there is only one line we need to use the curly braces.
    System.out.println("Name: " + name + ", Age: " + age);

    return; // optional statement on void methods
}

// will not compile because there is no return statement outside the if structures
public int abs(int number){
    if(number < 0){
        return -number;
    }

    if(number > 0){
        return number;
    }
}

// this compiles fine
public int abs(int number){
    if(number < 0){
        return -number;
    }else{
        return number;
    }
}

// will compile because Java accepts return wrappers instead of primitives
// since the returned wrapper is equal or smaller than the return type
public int returnWrapper(byte num){
    return new Byte(num);
}
```

In Java we have a special kind of parameter called **varargs**, it converts all values passed as parameters to an array. When we call a method that uses varargs we do not instantiate the array, we only need pass "n" values separed by commas. Some rules about varargs:

- It must be the last parameter in the method declaration
- We can have only one varargs in the method declaration
- If we do not inform any value on the method calling the Java will instantiate an array of length 0.

```java
public int totalSum(int... numbers){
    int total = 0;
    for(int number : numbers)
        total += number;

    return total;
}

int total = totalSum(3, 2, 5, 4); // will return 14

public int[] multiplyBy(int times, int... numbers){
    for(int i = 0; i < numbers.length; i++){
        numbers[i] *= times;
    }
    return numbers;
}

int[] numbers = multiplyBy(2, 3, 4, 5); // int[3] { 6, 8, 10 }
int[] numbers2 = multiplyBy(2); // int[0] { }
```

Similar to a method we have the constructor, theirs structure are almost the same, the main differences are that the constructor is called only when the object is being instantiated, and it does not not have a return type.

```java
public class Student{
    String name;

    public Student(String name){
        this.name = name;
        System.out.println("Constructor called");
    }

    public Student Student(String name){
        this.name = name;
        System.out.println("Method called");
        return this;
    }
}

Student student = new Student("Victor"); // Constructor called
student = student.Student("Victor"); // Method called
```

When we try to instantiate an object, Java goes for a checklist:

1. If the class has not been initialized yet, Java does it
2. Check and allocate the memory space for the object
3. Initialize the instance variables with their default values
4. Execute instance initializers
5. Execute constructors

Every class has a constructor. If we do not create one, Java will build a default constructor with empty body and no parameters. However, if there is any constructor defined by the programmer, Java will not provide this structure. 

It is common to confuse the pratical difference between an initializer and a constructor. There are four main differences:

- Initializers do not take any parameter
- When a object is created, all initializers are called
- On initializers we can not access a variable that is declared after it, but we can assigns values to this variable though
- Initializers do not accept exceptions

## Create and invoke overloaded methods

One of the most important things about methods in Java is their signature. A method's signature is what defines that it will be unique in the class. It is a composition of the name and its ordered parameters's type. Here are some examples of methods signatures:

```java
// method
public void method(int number, String word){ }
// signature
method(int, String);

//method 
private String[] upperCase(String[] words){ }
//signature
upperCase(String[]);
```

Knowing about methods signature is crucial to understand method overloads. A method is overloaded when we have many other methods with the same name, but with different signatures. This is a very tricky strategy, there are many possible pitfalls when we need to guess which method Java will call. There are some rules to understand how the method selection works, and if the compiler can not guess which is the right method it will cause an error on compile time. Here are the rules about selecting overloaded methods:

1. **Exact match:** the compiler tries to find a method that have the **exact** types that were passed as parameters.
2. **Nearest:** the nearest methods are searched. For example, if there is no method that takes **byte** as argument, the compiler tries to find one that takes **short**. If there is no method that takes **String** as argument, the compiler tries to find one that takes **CharSequence** instead, and so on... Java have two hierarchies for primitive values: double > float > long > int > char and int > short > byte. It means that short is a subtype of int, int is subtype of float and etc... In this rule Java tries to find the nearest parent, here is an example: 

```java
void method(float number);
void method(short number);

method(5); // calls the first one, because is the nearest parent
```

3. **Widening > Autoboxing:** if there is a method with a primitive parameter near the desired one, and another method with a wrapper that matches exactly the desired primitive, the compiler will choose the first option, wrappers are only choosen if there is no primitive match.

```java
void method(long number);
void method(Integer number);

method(5); // calls the first option, even the second being a wrapper to int
```

4. **Autoxing > varargs:** using the same logic of the previous rule, if the compiler needs to decide between a wrapper and a varargs, it will choose the wrapper.

If none of these rules can be applied, it will result in a compilation error. Another case that generates error is when two or more methods are equaly able to be selected.

```java
void method(Integer n1, short n2);
void method(int n1, Short n2);

int num1 = 4;
short num2 = 3;

// this will result in compilation error 
// applying the rules both of them are equaly eligible
method(num1, num2);
```

All these rules are also applied to constructors, and they have another feature called **"constructor chain"**, that happens when a constructor calls another overloaded one. The only rule about it is that this call needs to be the first statement in the constructor.

```java
public class Person{
    String name;
    int age;

    public Person(String name, int age){
        this(name);
        this.age = age;
    }

    public Person(String name){
        this.name = name;
    }
}
```

## Apply the static keyword to methods and fields

We can apply the **static** keyword to almost everything inside a class, doing this we are saying that the specific resource belongs to the class, not to the instance of the class. That being said, we can assert that this statement will compile and work.

```java
class Person{
    public static String name = "Luke";

    public static void hello(){
        System.out.println("Hello " + name);
    }
}

Person p = null;

// this statement works because the method "hello" is attached to class, not the instance
p.hello();
```

Another interesting option that static variables provide is to import them in other classes. Let's assume that the class "Person" is on the package "models".

```java
// imports all static fields from Person
import static models.Person.*;

public class Talk{
    public static void startTalk(){
        // there is no need to use the prefix "Person", because "name" was imported
        System.out.println("Hi " + name + ", how are you?");
    } 
}
```