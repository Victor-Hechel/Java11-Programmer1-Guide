# Using Operators and Decision Constructs

## Use Java operators including the use of parentheses to override operator precedence

Before the talk about operators we need to clarify the difference between **expression** and **statement**, which are terms we will use a lot in this chapter. Basically, an **expression** is a set of variable and values united by operators and returning a primitive or reference value. A **statement** is a line of code, it may return a value or not. A program is composed by statements that are composed by expressions.

That known, let's talk about operators. In Java we have many kinds of them, they can be classified as the type of operations, number of operands and even type of operands. Here we are going to see the available operators in the language.

**Arithmetic Operators**

They are used on numeric primitive values and their wrappers.

- **Addition, Subtraction, Multiplication and Division(+, -, \*, /):** Basic operators
- **Modulus operator(%):** Returns the remainder of a division
- **Unary minus(-):** negates the value inverting the signal
```java
int num = -3;
int num2 = -num; // assigns +3
```
- **Unary increment/decrement(++,--):** increases or dicreases the value by 1. We have two ways to use this operands: pre and post. On the **pre**, the operand is applied before it is evaluated, and in the **post**, after. It's also important to emphasize that this operand can be used on wrapper objects, but since they are immutable it will result in another object;
```java
int num = 2;
int num2 = num++; // assigns 2 to num2 and then num increases num to 3
int num3 = ++num; // increases num to 4 and then assigns 4 to num3
```

**Relational Operators**

Relational operators are use to compare values and return a boolean.

- **Less than, greater than, less than or equal to, greater than or equal to(<, >, <=, >=):** used to compare numerics and return a boolean.

- **Equal to and not equal to(==, !=):** used to compare primitives and references as well, but you must compare elements with the same kind, it is not possible to compare primitive with reference (unless it is a wrapper) or objects with different types.

**Logical Operators**

- **Short circuiting - And, Or(&&, ||):** used to compare two boolean. They are known as **short circuiting** operators because they stop to evaluate the expression when the result is already defined.
```java
int i = 0, j = 0;
boolean flag = true;

// j will not be incremented because this part of the expression will not be evaluated, 
// since this part will make no difference in the final result
if(flag || i == j++){ 
    // do something
}
```

- **Non-Short circuiting - And, Or(&, |):** works like the ones explained before, but these are non-short circuiting, i.e., the whole expression will be evaluated.

- **Exclusive Or(^):** returns true if one, and only one, of the arguments is true, if both of them are true it will return false.

- **Negation(!):** change true to false and vice versa.

**Assignment Operators**

- **Simple Assignment(=):** represents the assignment of a value into a variable. Interesting fact is that it returns the value that was assigned, which means that we can use a simple assignment in expressions.
```java
int i = 20;
if((i = 40) > 30){
    System.out.println("Worked!"); // will print the message because the value assigned was 40
}
```

- **Compound Assignment(+=, -=, \*=, /=, %=, ...):** these operators are shortcuts, they do their normal operation and then assigns the result to the variable. Expanding the expression **int i += 10** we have this: **int i = (int)(i + 10)**.

**Bitwise Operators**

- **And, Or, Xor(&, |, ^):** compare each bit and returns the resultant value.
```java
int a = 1; // ...001
int b = 3; // ...011

int c = a & b; // 1
int d = a | b; // 3
int e = a ^ b; // 2
```

- **Bitwise complement:** inverts the bits, **"0001"** becomes **"1110"**. 

- **Bitwise signed right/left shift:** shifts the bits by a certain quantity to the left or right preserving the sign.
```java
byte b1 = 4; // 0 0 1 0 0

// shifts two bits to the right
byte result1 = b1 >> 2; // 0 0 0 0 1
// shifts two bits to the left
byte result2 = b1 << 2; // 1 0 0 0 0

int i = -1; // 1111 1111 1111 1111 1111 1111 1111 1111
int result3 = i >> 1 // will return -1 as well, because the sign is preserved
```

- **Bitwise unsigned right shift(>>>):** works like the one mentioned before, but does not preserves the sign.
```java
int i = -1; // 1111 1111 1111 1111 1111 1111 1111 1111
int result3 = i >> 1 // 0111 1111 1111 1111 1111 1111 1111 1111 - +2147483647
```

Another important factor about operators is the numeric promotion. This is a very tricky topic, but we have basically two rules only:

- when we are making an operation with variables smaller than int, they are promoted to it.
- when the operation is done with variables bigger than int, they are promoted to the biggest one.

```java
byte b = 2;

// All statements bellow will fail on compile time because b is being promoted to int on each expression
b = -b; 
b = b + 1; 
b = b | 2;

int i = 10;
i = i + 10f; // will not compile because "i" is being promoted to float
```

It's imporant to remember that it occurs when the compiler perceives a possible loss on the convertion, but when we use only literals or final primitive variables this error does not appear. Another way to avoid this error is using compound operators, because they do an implicit cast, as said previously.

Another aspect about operators and expressions is their precedence on the evaluation process. We have some rules, one of them is the priority of each operator as we can see in the table bellow.

| Operator  | Precedence  |
|---|---|
| Member and array access  | . [] |
| Cast | () |
| Postfix | ++ -- |
| Unary | ++a --a +a -a ~ ! |
| Multiplicative | * / % |
| Additive | + - |
| Shift | << >> >>> |
| Relational | < > <= >= instanceof |
| Equality | == != |
| Bitwise And | & |
| Bitwise Xor | ^ |
| Bitwise Inclusive Or | \| |
| Logical And | && |
| Logical Or | \|\| |
| Ternary | ? |
| Assignment | = += -= *= ... |
| Lambda | -> |

The evaluation of an expression is made in three parts:

- first, the rules of precedence and grouping(the use of parenthesis) will be applied in order to have no ambiguity in the expression order
- second, it will be evaluated from left to right independent from the priorities. If a problem occurs or Java perceives that the result of the expression is already defined at that point a short circuit happens and the rest of the expression will not be evaluated
- finally, the priorities defined on the first step are applied and the expression is completly evaluated

On the example bellow we can see a short circuit ignoring the priority. The "i++ == 0" will be evaluated before the parenthesis, since "i" is equal to 0 and the next operator is an "Or", which means the expression will be "true" despite the rest, so the another "i++" and the "j++" will not be executed

```java
int i = 0;
int j = 0;

if(i++ == 0 || (i++ != j++)){
    // do something
}

System.out.println(i); // 1
System.out.println(j); // 0
```

## Use Java control statements including if, if/else, switch

You probably already know how an "if" statement works, so here I am just going to highlight some unusual examples of this structure.

```java

// will not compile because the if has no block or statement
if(true)
else
    System.out.println("else");

// compiles fine because the if has an empty statement
if(true);
else
    System.out.println("else");

```

We also have a shortcut for the if/else statement, which is the ternary operator. To use it we set a condition and a return value in case it is true and another whether it is false, they must be from the same type, unless they are numeric, in this case it will result on the largest type.

```java
int num1 = 20, num2 = 40;

// compiles fine, because if it falls as false the 3 will be promoted to double. Same rule as a simple assignment
double a = num1 == num2 ? 15.3 : 3; 

// will not compile, there must be an else
int num3 = num1 == num2 ? 5;

// will not compile because the last return type is compatible with String
String str1 = num1 == num2 ? "String" : new Integer(4);
```

Similar to the if statement we have the switch, which is a chain of possible results. We pass an argument that must be byte, char, short, int, enum or String, and define many possibilities to the value, if it does not fit in any of them we can set a default option. Another important fact about switch is that it has a cascading effect, when it enters a block all the others bellow are also executed, unless the default.

```java

int i = 2;

// will print Two and Three
switch(i){
    case 1: System.out.println("One");
    case 1 + 1: System.out.println("Two"); // since the operation is done in compile done it works
    case 3: System.out.println("Three");
}


// will print only Two
switch(i){
    case 1: System.out.println("One"); break;
    case 2: System.out.println("Two"); break;
    case 3: System.out.println("Three"); break;
}

// prints Default
switch(i){
    case 1: System.out.println("One"); break;
    case 3: System.out.println("Three"); break;
    default: System.out.println("Default");
}

// prints Default as well
switch(i){
    default: System.out.println("Default");
}

```

A curious aspect of the switch is the case of enums. When we use an enum as parameter there is no need to refer the name on the cases, just the value.

```java

DayOfWeek day = DayOfWeek.TUESDAY;

switch(day){
    // there is no need to write the whole enum reference
    MONDAY: System.out.println("Mon"); break;
    TUESDAY: System.out.println("Tue"); break;
    WEDNESDAY: System.out.println("Wed"); break;
    THURSDAY: System.out.println("Thu"); break;
    FRIDAY: System.out.println("Fri");
    // will not compile because DayOfTheWeek has no value "DayOfWeek.FRIDAY"
    // DayOfWeek.FRIDAY: System.out.println("Fri");
    default: System.out.println("Weekend")

}

```

## Create and use do/while, while, for and for each loops, including nested loops, use break and continue statements

You probably already know how a loop works in a programming language, there are no big differences in Java, so here we are going to see the basic syntax and some attention points.

```java

int i = 0;

// most basic loop, same syntax rules of an if structure
while(i < 10){
    System.out.println("i: " + i);
    i++;
}

// same as a normal while, but it is always executed at least once, 
// because the condition is evaluated after the block
do{
    System.out.println("i: " + i);
}while(i < 0);

// normal "for" estructure
for(int j = 0; j < 10; j++){
    System.out.println("j: " + j);
}

// none of the three sections are required, unlike the while structure where the condition is required
for(;;);

String[] names = { "Luke", "Han Solo", "Leia" };

// for each structure, it can iterate through arrays and objects that implements the Iterable interface
for(String name : names){
    System.out.println(name);
}

// the "break" keyword finishes the current loop
// "continue" keyword ignores and skips the rest of the current iterator

for(int j = 0; j < 10; j++){
    if(j == 6)
        break;
    if(j == 2);
        continue;
    System.out.println("j: " + j);
}

// this block will print: 
// 0
// 1
// 3
// 4
// 5

```

A very interesting tool in Java is the label, it is used to tag statements and blocks (not applicable on declarations neither method's signatures). We can combine labels with break/continue to control the flow of nested loops.

```java

FIRST_LOOP: for(int i = 0; i < 5; i++){
    for(int j = 0; j < 3; j++){
        if(i == 2 && j == 0)
            continue FIRST_LOOP;

        if(i == 3 && j == 1)
            break FIRST_LOOP;

        System.out.println("i: " + i + " j: " + j);
    }
}

// this code prints:

// i: 0 j: 0
// i: 0 j: 1
// i: 0 j: 2

// i: 1 j: 0
// i: 1 j: 1
// i: 1 j: 2

// i: 3 j: 0

```