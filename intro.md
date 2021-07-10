# Ziro 
## A strictly-typed, modern OOP language
### By John L. Carveth
---
### Getting Started
Ziro aims to combine the best features of C/C++ and Java programming languages. A performant, memory efficient language with a coherent type system and straightforward object-oriented features.  
  
Ziro files use the either `.z` or `.zo` file extensions. Let's look at a "Hello World" program written in Ziro:
```
program Main {
    Console.writeLine("Hello World!");
}
```

The `program` keywords indicates an entry point, ie. a main method where the program begins execution. The name `Main` indicates the target output name. A program declaration is followed by curly braces `{}` and 0 or more statements. In the Hello World example, we are executing a static method `writeLine` from the `Console` class to print a string.

### Challenges & Solutions
These will be a couple challenges I can see immediately with developing this language:
- Storing Arrays
- Parsing Datatypes properly, storing strings
- Properly avoiding primitives
- Handling the class heirarchy, especially inheritance

Arrays should be relatively straightforward. Simply allocate enough memory for the given type and size of the array, as arrays *need* a type and size. Storing strings will be dependent on the buffer that will be reading the source code. If it cannot store the entire string then an error must be thrown.  
When a type is detected it should be parsed, and a relevant type object properly initialized. For example, `1.40993` would be detected as a floating point number, and a Float struct would be created accordingly.  
  
  Class heirarchy should be tracked using a tree data structure. The root of this tree would be a base `Object` class, similar to Java. The built-in classes will be extended from `Object` as needed.