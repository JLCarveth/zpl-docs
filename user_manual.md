# Ziro User Manual

## About the Ziro Programming Language
Ziro is a type-safe, object-oriented programming language which idealizes modern programming conventions while remaining relatively low-level. Ziro takes inspiration and design cues from a number of languages. For example, the way Ziro handles for loops is inspired by Kotlin. The syntax of Ziro is designed so that anyone familiar with a popular language such as Java, C/C++ should feel right at home. 

## Comments and Keywords
Comments can be declared multiple ways. Single-line comments can be declared with two forward slashes `//`. Multiline comments begin with `/*` and end with `*/`.
  
  - **Self-Documenting Comments:** Similar to Javadoc comments in the Java language, comments on methods, fields, and classes that begin with `/**` are self-documenting comments, allowing the compiler to generate documentation for the source code. A seperate compiler 'mode' would parse these comments and generate HTML or Markdown documentation based on the elements they preceed.

  ```
  // This is a one-line comment
  Integer i = new Integer(20);
  /*
    Anything between a multiline comment
    is ignored
    by
    the
    compiler!
  */
  ```

Keywords are special identifiers reserved for use by the programming language itself. A list of keywords in the Ziro programming language are as follows:  
```
byte  short  int  long  float  double  char  boolean  String  void  null enum  class  new  if  else  do  while  for  foreach  switch  case  break  continue  default  program  function  return  write  writeLine  read  readLine
```

## Datatypes
Ziro supports all common data types present in other programming languages, with a focus on occupying a minimal ammount of space to represent the data. Any uninitialized data is assigned a `Null` value.
|Type Name| Size in Memory (Bits)|Value Range| Additonal Info | Equivalent C Datatype
|---|---|---|---|---|
Integer| 32 | [-2<sup>31</sup>, 2<sup>31</sup>]|Basic 32-bit signed integer.|`int`|
Short | 16 | [-2<sup>15</sup>, 2<sup>15</sup>]|16-bit signed short integer.|`short int`|
Long | 64 | [-2<sup>63</sup>, 2<sup>63</sup>] | 64-bit signed long integer |`long`|
Byte | 8 | [-2<sup>7</sup>, 2<sup>7</sup>]| Represents a single byte|`char`|
Boolean | 1 | [0,1]| Represents True / False or a single bit of data|`char`|
Float | 32 | N/A | IEEE 754 Single-precision floating point format|`float`|
Double | 64 | N/A | IEEE 754 Double-precision floating point format|`double`|
String | N/A| N/A | Represents a string of characters. Length of the string is limited by the size of the buffer being used to read the source code. ANSI C uses a limit of 509 characters.|`char*`|
Array | 64 | N/A | Represents a multitude of elements of a single type. The array will occupy `(n * sizeof(T))` in space, where `n` is the number of elements in the array and `T` is the type being stored in the array.|`void*`|

## Statements
### Assignment
Assignment syntax is very similar to Java. A type declaration is followed by the variable name. Initializing that variable involves using `=` and assigning a value valid for the declared type. For example:
```
String str = "Hello World!";
```
`String` is the type of variable `str`, and we are assigning a value of `Hello World!` to this variable. 
### Selection
Selection (if) statements are again quite similar to many popular programming languages.
The keyword `if` is followed by a boolean statement surrounded by parentheses. The scope of the if statement is delimited by curly braces `{}`.
```
if (x < 10)
{
  // Some statements here...
}
```
### Interaction
Ziro offers the same loops as seen in many other languages, `for`, `while`, `do while` being some examples. These loops follow a common syntax as well:
```
/* A while loop */
while (<conditional_statement>) 
{
    // Some statements here...
}
/* A do while loop */
do {
    // Some statements here...
} while (<conditional_statement>);
/* A for loop */
for (Integer i = 0; i < x; i++)
{
    // Some statements here...
}
```

In addition to these common loops, Ziro also offers a `foreach` loop which can be executed on any Iterable object (Arrays, or other classes extending Iterable):
```
foreach (String s : arrayOfStrings) 
{
    // Some statements here...
}
```

### Input
Ziro will come with a built-in class `Console` which interfaces with `printf` and `scanf` in order to provide proper input functionality. For input specifically, `Console` has the following relevant functionality:
```
Console.readLine() : String
```
This function is essentially `scanf("%s", str);` with `str` being the value returned as a `String` object.
### Output
Similar to input, `Console` provides the relevant functionality to print information to the console.
```
Console.writeLine("Hello World!");
```
This function is equivalent to running `printf("Hello World!\n");` in C. A `Console.write()` exists if the programmer does not want to write to a new line.