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