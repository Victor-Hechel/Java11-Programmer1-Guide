# Working with Java Arrays

## Declare, instantiate, initialize and use a one-dimensional array

Arrays in Java can be simple, but tricky as well, since there are many ways to define and initialize it. It is always important to remember their main features: they are objects, support only one type and have an imutable length. Here are some examples and peculiarities about their declaration and instantiation.

```java
// array of 3 ints
int[] numbers = new int[3];

// square brackets can be placed on the right side of the variable name
int numbers2[] = new int[3];

// it is possible to create arrays with size 0, because of the varargs
int numbers3[] = new int[0];

// array of 3 ints with all slots filled
int[] numbers4 = { 1, 2, 3 };

// compiles since we do not specify the size inside the square brackets
int[] numbers5 = new int[]{ 1, 2, 3 };
```

Here are some invalid cases about arrays declaration and instantiation which cause compilation errors.

```java
// invalid declaration, array's size can not be present on the type
int[3] numbers = new int[3];

// it is not possible to create arrays with negative size
int[] numbers2 = new int[-1];

// size is defined by the quantity of values
int[] numbers3 = new int[3]{ 1, 2, 3 };
```

When we instantiate an array with no values it is filled with the default value of the type, if we have an int array it will filled with zeros, if we have an Integer array it will be filled with nulls. We need to pay attention to this when we are trying to access an slot that has not received any value yet. To set a value to an slot is very simple, there are only two rules to do it: the value must be compatible with the type and the index must valid (0 <= index < array length).

```java
int[] numbers = new int[3];

System.out.println(number[0] == 0);
System.out.println(number[numbers.length()]); // Error: ArrayIndexOutOfBounds(classic)

numbers[1] = 15;
System.out.println(number[1] == 15);

Object[] objs = new Object[2];

objs[0] = "String"; // Strings are Objects so this statement is valid
```

There is a method from the array API that may be tricky, it is called "clone". It returns a new instantiation of an array with the same values of the current, but in the case of reference values only the address to the object is cloned. This issue is called "shallow copy".

```java

Icecream[] icecreams = { new Icecream("chocolate"), new Icecream("strawberry") };

Icecream[] clonedIcecreams = icecreams.clone();

icecreams[0].flavor = "vanilla";

// different memory addresses
System.out.println(icecreams != clonedIcecreams);

// prints vanilla
System.out.println(clonedIcecreams[0].flavor);

// same memory addresses
System.out.println(icecreams[0] == clonedIcecreams[0]);
```

Java 9 has introduced two important methods to the static class Arrays: compare and mismatch. Both of them use the lexicographically comparation, i.e., the comparation of the dictionary. Compare returns a negative value if the first parameter is less lexicographically, a positive one if it is greater and 0 if they are the same. Mismatch returns the index of the first difference between the arrays.

```java
String[] names1 = { "Messi", "Cristiano Ronaldo", "Salah" };
String[] names2 = { "Messi", "Mane", "Salah" };
String[] names3 = { "Messi", "Mane" };

// prints -10 because "Cristiano Ronaldo" comes before "Mane"
System.out.println(Arrays.compare(names1, names2));

// prints 1 because names3 is smaller
System.out.println(Arrays.compare(names2, names3));

// prints 1 because "Cristiano Ronaldo" is the first different value
System.out.println(Arrays.mismatch(names1, names2));

// prints 2 because the absent of a next value counts as a difference
System.out.println(Arrays.mismatch(names2, names3));

// prints -1 because there is no difference
System.out.println(Arrays.mismatch(names2, names2));
```

## Declare, instantiate, initialize and use a two-dimensional array

Two-dimensional arrays can also be called multi-dimensional arrays, since they are all arrays of arrays. It is basically the same as a one-dimensional but we need to be more careful to be certain that we are accessing the right values.

```java
// array of size 5, each slot contains an instance of an array os size 10
int[][] numbers = new int[5][10];

// array of 3 slots of char arrays, they are no instances
char[][] characters = new char[3][];

// valid declarations
short[] shortNumbers[];
short shortNumbers2[][];
short[][][] shortNumbers3; // three-dimensional array

// this statement will not compile, you must always inform the size of the previous array
int[][] invalidInstantiation = new int[][5];
```