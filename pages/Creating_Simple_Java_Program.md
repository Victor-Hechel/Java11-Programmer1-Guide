
# Creating a Simple Java Program

## Create an executable Java program with a main class

First, is important to be clear that **Java classes are not executable, JVM is**. We execute the JVM passing as argument the FQCN (Full Qualified Class Name) of a Java class. Next, it loads the class and tries to find an specific method, which is the **main method**. If the JVM doesn't find it, an error is thrown.

The main method is responsible for starting the application, and has a specific signature that must be respected in order to be found by the JVM. It must be public, static, void, be named main (lower case) and have only one argument, which is an array of String. The exceptions throws doesn't interfere on the method call.

Here are some examples of valid main's declarations. There are other possibilities as well (using "abstract" or "native" for example), but these are the most common.

Most common:

```java
public static void main(String[] args) { }
```

Varargs is equivalent to array:

```java
public static void main(String... args) { }
```

Exceptions doesn't affect the method call:

```java
public static void main(String[] args) throws Exception {
    throw new Exception();
}
```

Another important fact about the main method is it's end. When the main method reaches it's end, doesn't necessarily mean the application is finished, because there might still be threads running. A Java application is only finished when all processes reach their ends.

## Compile and run a Java program from the command line

To compile a Java class we need to use the command "javac", which is included on JDK, and inform the Java file. Another very imporant rule is that the file name must be equal to the top-level class name, otherwise there will be a compilation error.

Here is an example of how to compile and run a simple Java program.

```java
public class ConsoleTest{
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```

```console
javac ConsoleTest.java
```

```console
java ConsoleTest
Hello World!
```

There is also possible to pass informations as arguments on the console when we execute the program.

Class for tests:

```java
public class MultipleArgsTest{
    public static void main(String[] args){
        System.out.println("Argument 1: " + args[0]);
        System.out.println("Argument 2: " + args[1]);
    }
}
```

Example without quotes:

```console
javac MultipleArgsTest.java
java MultipleArgsTest foo bar

Argument 1: foo
Argument 2: bar
```

Example with quotes:

```console
javac MultipleArgsTest.java
java MultipleArgsTest "Hello World" "Hello Universe"

Argument 1: Hello World
Argument 2: Hello Universe
```

On Java 11 was included a very interesting feature. Now is possible to compile and run a Java class using only the "java" command. The difference is that informing the ".java" extension is required, just like the "javac" instruction.

```java
public class ConsoleTest{
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```

```console
java ConsoleTest.java
Hello World!
```

## Create and import packages

### Creating Packages

Every Java class belongs to a package. The package declaration is the first statement on Java source file. If there is no package declared, the class belongs to the "unnamed package", also refered as "default" (inclusive this is a reserved word, it is not possible to create packages with this name). The main problem with this package is that is not possible to reference it's classes since it doesn't have a name.

A good practice to name your packages is to use an inverse domain. Let's suppose you work on a company called "Byte Revolution", your group is responsible for the clients and you are devoloping the models. Here is an example of a class within this situation.

```java
package com.byterevolution.clients.models;

public class ClientOrder{
    // class content
}
```

This approach is used to create unique FQCN, in case you need to expose this class in a large scale, such as a maven dependencie.

### Importing Features

If the class you want to use on your code is on the same package you just need call it by its simple name, but if the class is in another package you need to use the FQCN to reference it.

Let's suppose we want to use the class ClientOrder, created previously, in another package.

```java
package com.byterevolution.clients.tests;

public class ClientOrderTests{

    public void test(){
        com.byterevolution.clients.models.ClientOrder clientOrder =
            new com.byterevolution.clients.models.ClientOrder();
    }
}
```

You see that if everytime the class is referenced we need to use the full name it would lead to a lot of repetition. The **import** statement solves the problem. Using it we can import classes from other packages and reference to its simple name. We also can use the wild card "*" to import all classes from an specific package.

Single import example:

```java
package com.byterevolution.clients.tests;

//imports only the Class ClientOrder
import com.byterevolution.clients.models.ClientOrder;

public class ClientOrderTests{

    public void test(){
        ClientOrder clientOrder = new ClientOrder();
    }
}
```

Import with wild card example:

```java
package com.byterevolution.clients.tests;

//imports all classes from the package models
import com.byterevolution.clients.models.*;

public class ClientOrderTests{

    public void test(){
        ClientOrder clientOrder = new ClientOrder();
    }
}
```

Since Java 5 there is another option when we are using features from other files, the **static import**. We use it to import static members, such as methods and variables. In this case, when the wild card is used it imports all static members from the class.

Refactoring ClientOrder class:

```java
package com.byterevolution.clients.models;

public class ClientOrder{
    public static int totalNumberOfOrders = 0;

    public static void makeOrder(){
        totalNumberOfOrders++;
    }
}
```

Example with single static import:

```java
package com.byterevolution.clients.tests;

import static com.byterevolution.clients.models.ClientOrder.totalNumberOfOrders;

public class ClientOrderTests{

    public void test(){
        System.out.println("Number of orders: " + totalNumberOfOrders);
    }
}
```

Example of static import with wild card:

```java
package com.byterevolution.clients.tests;

import static com.byterevolution.clients.models.ClientOrder.*;

public class ClientOrderTests{

    public void test(){
        makeOrder(); // now the method is also available
        System.out.println("Number of orders: " + totalNumberOfOrders);
    }
}
```

### Important facts about packages in Java

- You can import class, enum and interfaces.

- A duplicated import is ignored. The java.lang, for example, is always implicitly imported, if you try to import it explicit won't result in any error, it will just be ignored.

- It is not possible to import a child package when importing its father. If you import the package like this "import com.byterevolution.clients.models.*", you won't be importing this "com.byterevolution.clients.models.subpackage".

- Importing different classes with the same name doesn't result in any error, but you can't reference them using their simple name, because it will be ambiguous. So in this case you should import one of them and reference the other with the FQCN.
