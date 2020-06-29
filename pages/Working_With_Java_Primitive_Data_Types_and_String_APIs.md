# Working With Java Primitive Data Types and String APIs

## Declare and initialize variables (including casting and promoting primitive data types)

First of all, before we start on declaring variables, it is important to be clear the difference between primitive and reference data types. Primitive type is just raw data, it is the most simple information possible. They can all be represented as numbers and be used on mathematical operations, except for boolean, which is a logical type. In Java we have the following primitives(from the smallest to the biggest): boolean, char, byte, short, int, long, float and double. On the other hand, a Reference Type is a complex kind of data, which is composed by primitive and reference variables. When we assign a value to a Reference variable we are actually assigning the memory address.

Listed bellow are some examples of legal declaration and initialization of variables:

```java
// Simple declaration
int i;
char character;
String str;

// Multiple declaration of the same type
int int1, int2, int3;
boolean flag, isActive;

// Simple initialization
int j = 10;
String name = "Victor";

// Multiple initialization of same type
int age, year = 2020;
String str1 = "car";
String str2 = "motocycle", str3 = str1 = "train"; // str1 receives "train" and str3 receives str1
```

Listed bellow are some examples o *illegal* declaration and initialization of variables:

```java
int i, int j; // there must be only one type per statement

String age;
int age; // the variable name must be unique

int c = m = 10; // the variable "m" must be previously declared
```

Another important topic about variables is what happens when it has not been initialized and their default values. Here are some important facts about these topics:

- static variables are automatically initialized by JVM
- local variables are never initialized automatically
- instance variables are initialized on constructor. If the default constructor is called it will initialize the variables with their default values
- mathematical variables are initialized by default with 0, boolean ones with false and reference ones with null. The char type is initialized with '\u000' which is the unicode for null.
- java files will not compile if it tries to use a unitialized variable. If you have a unitiliazed variable but it is not accessed anywhere the file will compile

A tricky aspect about initializing variables is the case where it is initialized inside a control structure, because there might be a path where the variable has not been initialized.

The following code will not compile for two reasons: 

- there is a path where the variable "i" has not been initialized
- the JVM only analyses the flow when it is a final variable, so, even if it is clear that a control structure's content will always be executed the compiler will not consider unless it is a final variable.

```java
boolean flag = true;
int i;

if(flag){
    i = 10;
}

System.out.println(i); // "i" might have not been initialized 
```

Now, if we put "flag" as a final variable the code will compile.

```java
final boolean flag = true;
int i;

if(flag){
    i = 10;
}

System.out.println(i); // the code compiles because now the JVM knows that "i" is always initialized 
```

Another way to solve this problem is adding an "else" statement, this way on all possibilities the "i" variable is being initialized. 

```java
boolean flag = true;
int i;

if(flag){
    i = 10;
}else{
    i = 5;
}

System.out.println(i);
```

An important aspect when we are assigning values to variables is the type, because there are situations where we need to assign a value with a different type to the variable. There are basically two cases in Java: widening conversion and narrowing conversion.

**Widening conversion:** happens when we try to assign a small value to a bigger variable. There is no need to explicit the conversion, the JVM handles this.

```java
byte b = 5; // 8 bits
short s = 10; // 16 bits
int i = 1000; // 32 bits
long l = 1_000_000l; // 64 bits
float f = 23.4f; // 32 bits
double d = 53.32d; // 64 bits

i = b;
i = s;
l = i;
f = i;
d = f;
```

**Narrowing conversion:** it is the opposite of widening conversion, it happens when we try to assign a value to variable of smaller type. In this case we need to make an explicit cast, saying that the source value fits in the target type. If we assign a value bigger than the target type it will result in a different value.

```java
int i = 50;
byte b = (byte)i; // works fine because byte goes up to 127

long l = 200_000l
byte b2 = (byte)l; // will compile, but will store a value different than 200.000

final int i2 = 100;
byte b3 = i2; // on this case we do not need to do the explicit cast, because the source variable is final so the compiler will check it is a valid cast and handle that.
```

## Identify the scope of variables

There are two types of scope in Java: visibility and lifespan. 

The visibility scope of a variable covers all places lower than the one it has been defined. For example, if a variable was declared on a method, it is visible inside blocks in the method, but not visible to other methods or to the class directly. It is important to not confuse visibility with accessibility.

```java

public class Student{

    // visible in the whole class 
    public String name;
    public String surname;

    public void print()
    {
        String completeName = name + " " + surname; // visible only inside this method
        System.out.println(completeName);
    }
}

public class Main{
    public static void main(String[] args){
        
        Student s = new Student();
        // "name" is accessible, but not visible, since we had to refer the object
        s.name = "Rodrigo";
    }
}
```

The lifespan scope refers to when the variable will be unusable, in other words, when it's life time ends. There are five types of lifespan scopes:

- class (static and non-static fields)
- instance (only non-static)
- method
- for loop (variables declared on the for loop signature, not inside the for block)
- blocks

There are basically two rules about scope:

- You can not access variables with no visibility directly (obviously)
- The only kind of variable that can be overlaped is the class one, using method, for loop or block variables.

## Use local variable type inference

Thanks to Java 10 we are able to use type inference on local variables (it does not work with class-scope variables or method parameters). Basically it means that we can let the compiler infer the type of the variable when there is no ambiguity.

```java
public class VariablesInference{

    public static void main(String[] args){

        // VALID CASES

        var s = "Test"; // compile infers type as String

        Object obj = "String";
        var objectVariable = obj; // type will be Object and not String, because it is the type of "obj"

        var var = "not a keyword" // "var" is a shortcut, not a keyword, so it is possible to use it as a variable name

        // INVALID CASES

        var number; // it is not possible to use this feature with uninitilized variables

        var nullCase = null; // null is not a type

        var intArray = { 5, 23, 12}; // type of array is not defined

        var[] stringArray = new String[2]; // var is not a type, so there is no need of [] on the declaration;
    }
}
```

## Create and manipulate Strings

A String is an object which represents a unmutable sequence of characters. In Java its class is final, which means that is not possible to have derivations from it, however, it implements the interface CharSequence, so we can use it in place of String in some cases. 

There are many ways to create a String, since it is an object one of the ways is by constructors.

- **String():** empty String
- **String(CharSequence):** creates new String in memory with the value of a CharSequence(String, StringBuilder, ...)
- **String(bytes[]):** decodes the bytes with the default charset
- **String(char[]):** turns a char array into a String

Another way is concatenating a String with some other element, but there are some important informations about it:

- if the String is being concatenated with a reference variable, its method **toString()** will be called
- if the String is being concatenated with a reference variable, which its value is **null**, then **null** will be printed(**NullPointerException** will not be thrown)
- if the String is being concatenated with a primitive value it will be created a wrapper object and then the **toString()** method is called
- in Java, expressions are evaluated from left to right, so **(1 + 2 + "3")** will result in **"33"**, but **("3" + 2 + 1)** will result in **"321"**. 

Strings created with the **new** keyword are stored in the **heap space**, since it is an object, but when it is created by using literals and concatenation they are stored in the **String Pool**. Its purpose is to keep Strings in case that there is the need to create a repeated one, so we will not have multiple identical Strings. 

```java
String s1 = "test";
String s2 = "test";
String s3 = new String("test");
String s4 = new String("test");

// will print true, because both of them point to the same String in the String Pool
System.out.println(s1 == s2); 

// will print false, because "test" from s2 is in the String Pool and the one from s3 
// is in the heap space
System.out.println(s2 == s3);

// will print false, when we use the "new" keyword another object is stored in memory
System.out.println(s3 == s4);

// will print true, "intern" method gets the reference from String Pool
System.out.println(s1 == s3.intern());
```

Another important subject about Strings is their API, since we can not change their values is important to have a wide knowledge of the methods. Here are some of the most important String methods:

- **int length():** returns the length of the String
- **char charAt(int index):** returns the char in the index
- **int indexOf(int character):** returns the index of the first occurance of the char. It also supports String as parameter and an int representing the start index
- **String substring(int beginIndex, int endIndex):** returns the String in the interval
- **String concat(String str):** returns a new String with the parameter concatenated
- **String toLowerCase():** returns the whole String in lower case
- **String toUpperCase():** returns the whole String in upper case
- **String replace(char oldChar, char newChar):** returns a new String with all characters specified replaced
- **String trim():** returns a new String without white spaces with code equal or lower than 'U+0020' on the leading and trailing
- **String strip():** same thing as trim(), but also considers another unicodes greater than 'U+0020'
- **boolean isEmpty():** returns true if String is empty
- **boolean isBlank():** returns true if String is empty or white spaces
- **boolean startsWith(String prefix):** returns true if String starts with the specified parameter
- **boolean endsWith(String suffix):** returns true if String ends with the specified parameter
- **boolean contains(CharSequence s):** returns true if String contains the sequence of characters
- **boolean equals(Object anObject):** returns true if String value is equal to the object's value
- **boolean equalsIgnoreCase(String anObject):** same thing as equals() but ignores case.

## Manipulate data using the StringBuilder class and its methods

StringBuilder is a mutable final class, implements CharSequence and is equivalent to the String class. The difference between them is that it makes use of Strings as regular objects, so they are ellected to be diposed by the garbage collector. It is common to use StringBuilder when we need to make many concatenations to a String, this way the partial ones do not occupy space in memory.

```java
StringBuilder sb = new StringBuilder();

sb.append("test");
sb.append(" another test");

System.out.println(sb); // prints "test another test"
String s = null;
sb.append(s).append(new Object()); // StringBuilder methods return itself

System.out.println(sb); // prints "test another testnulljava.lang.Object@6d06d69c"

sb.insert(5, "inserted test ");

System.out.println(sb); // prints "test inserted test another testnulljava.lang.Object@6d06d69c"

sb.replace(0, 4, "replaced");

System.out.println(sb); // prints "replaced inserted test another testnulljava.lang.Object@6d06d69c"

sb.delete(30, sb.length());

System.out.println(sb); // prints "replaced inserted test another"
```
