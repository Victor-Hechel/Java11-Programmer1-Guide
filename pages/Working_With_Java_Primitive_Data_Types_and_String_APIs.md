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