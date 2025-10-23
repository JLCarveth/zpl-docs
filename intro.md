# Zero (ZPL)
## A Simple, C-like Procedural Language
### By John L. Carveth
---
### Getting Started
Zero (ZPL) is a simple, statically-typed procedural programming language inspired by C. It features a clean syntax, type safety, and straightforward control flow constructs, making it ideal for learning compiler construction and systems programming concepts.

Zero source files use the `.zpl` file extension. Let's look at a "Hello World" program written in Zero:
```
program Main {
    writeLine("Hello World!");
}
```

The `program` keyword indicates an entry point (similar to `main()` in C) where the program begins execution. The name `Main` is the program identifier. A program declaration is followed by curly braces `{}` containing zero or more statements. In the Hello World example, we use the built-in `writeLine` keyword to print a string to the output stream.

### Language Features
ZPL includes the following features:
- **Primitive Types**: byte, short, int, long, float, double, char, bool
- **Arrays**: Fixed-size, single-dimensional arrays with compile-time size specification
- **Functions**: User-defined functions with parameters and return values
- **Control Flow**: if/else, switch, while, do-while, for loops
- **I/O**: Built-in read/readLine and write/writeLine keywords for input/output
- **Operators**: Arithmetic (+, -, *, /, %), logical (&&, ||, !), relational (<, >, <=, >=, ==, !=), unary (++, --)

### Implementation Considerations
Key implementation aspects for the ZPL compiler:
- **Type Safety**: All variables must be declared with explicit types before use
- **Static Typing**: Type checking performed at compile time
- **Array Handling**: Arrays require compile-time constant sizes and use 0-based indexing
- **String Handling**: Strings are represented as char arrays (e.g., `char[100]`)
- **Function Scope**: Functions must be declared before the program block
- **No OOP**: The language is procedural - no classes, objects, or inheritance