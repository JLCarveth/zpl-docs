# ZPL (Zero Programming Language) User Manual

## About the Zero Programming Language
Zero (ZPL) is a simple, statically-typed procedural programming language with a C-like syntax. It is designed to be straightforward to implement and understand, making it ideal for learning compiler construction. The syntax is familiar to anyone who has experience with C, Java, or similar languages, focusing on clarity and simplicity over advanced features. 

## Comments
ZPL supports two types of comments:
- **Single-line comments:** Begin with `//` and continue to the end of the line
- **Multi-line comments:** Begin with `/*` and end with `*/`

```
// This is a single-line comment
int x = 5;

/*
   This is a multi-line comment.
   Everything between the delimiters
   is ignored by the compiler.
*/
```

## Keywords
Keywords are special identifiers reserved for use by the programming language. A list of keywords in ZPL:
```
byte  short  int  long  float  double  char  bool  void
if  else  do  while  for  switch  case  break  continue  default
program  function  return
write  writeLine  read  readLine
```

Variables and functions cannot be named using these reserved keywords.

## Data Types
ZPL supports primitive data types common in C-like languages. All variables must be declared with a type before use.

|Type Name| Size (bits) | Value Range | Description | C Equivalent |
|---------|-------------|-------------|-------------|--------------|
|`byte`   | 8  | [-128, 127] | 8-bit signed integer | `char` |
|`short`  | 16 | [-32768, 32767] | 16-bit signed integer | `short` |
|`int`    | 32 | [-2³¹, 2³¹-1] | 32-bit signed integer | `int` |
|`long`   | 64 | [-2⁶³, 2⁶³-1] | 64-bit signed integer | `long` |
|`float`  | 32 | ±3.4×10³⁸ | IEEE 754 single-precision float | `float` |
|`double` | 64 | ±1.7×10³⁰⁸ | IEEE 754 double-precision float | `double` |
|`char`   | 8  | ASCII characters | Single character | `char` |
|`bool`   | 8  | true/false (0 or 1) | Boolean value | `char` |

### Arrays
Arrays are declared by specifying a size in square brackets:
```
int numbers[10];      // Array of 10 integers
char name[50];        // Character array (string)
float values[100];    // Array of 100 floats
```
- Arrays must have a compile-time constant size
- Arrays use 0-based indexing
- Elements are accessed using square brackets: `numbers[0]`, `name[5]`

## Statements

### Variable Declaration and Assignment
Variables must be declared with a type before use. Declaration can optionally include initialization:
```
int x;              // Declaration only
int y = 10;         // Declaration with initialization
float pi = 3.14159;
char letter = 'A';
bool flag = true;
```

Assignment to an existing variable:
```
x = 5;              // Assign value to x
y = x + 10;         // Assign expression result
```

Array element assignment:
```
int arr[5];
arr[0] = 10;
arr[1] = arr[0] * 2;
``` 
### Selection Statements

#### If/Else Statements
The `if` keyword is followed by a boolean expression in parentheses. The body is enclosed in curly braces:
```
if (x < 10) {
    writeLine("x is less than 10");
}

if (y > 0) {
    writeLine("y is positive");
} else {
    writeLine("y is not positive");
}

// Else-if chains
if (score >= 90) {
    writeLine("Grade: A");
} else if (score >= 80) {
    writeLine("Grade: B");
} else if (score >= 70) {
    writeLine("Grade: C");
} else {
    writeLine("Grade: F");
}
```

#### Switch Statements
The `switch` statement allows multi-way branching:
```
switch (day) {
    case 1:
        writeLine("Monday");
        break;
    case 2:
        writeLine("Tuesday");
        break;
    default:
        writeLine("Other day");
        break;
}
```

### Iteration Statements

#### While Loop
```
int i = 0;
while (i < 10) {
    writeLine(i);
    i++;
}
```

#### Do-While Loop
Executes at least once before checking the condition:
```
int i = 0;
do {
    writeLine(i);
    i++;
} while (i < 10);
```

#### For Loop
```
for (int i = 0; i < 10; i++) {
    writeLine(i);
}

// Can use existing variable
int j;
for (j = 0; j < 5; j++) {
    writeLine(j * 2);
}
```

**Loop Control:**
- `break`: Exit the loop immediately
- `continue`: Skip to the next iteration

### Input/Output

#### Output
ZPL provides built-in keywords for output:
- `write(expression)` - Outputs the value of an expression without a newline
- `writeLine(expression)` - Outputs the value and adds a newline

```
writeLine("Hello, World!");
write("Enter a number: ");

int x = 42;
writeLine(x);           // Outputs: 42

float pi = 3.14159;
writeLine(pi);          // Outputs: 3.14159
```

#### Input
Input keywords return values that must be assigned to variables:
- `read()` - Reads a single character
- `readLine()` - Reads an entire line (stores as char array)

```
char ch;
ch = read();            // Read single character

int num;
write("Enter a number: ");
num = read();           // Read integer from input

char name[50];
write("Enter your name: ");
name = readLine();      // Read line into char array
```

## Functions
Functions are declared using the `function` keyword, followed by the return type, name, parameters, and body:

```
function int add(int a, int b) {
    return a + b;
}

function void printGreeting(char name[]) {
    write("Hello, ");
    writeLine(name);
}

function int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

**Key points:**
- Functions must be declared before the `program` block
- Return type can be any data type or `void` (no return value)
- Array parameters use `[]` without a size
- Functions can be called recursively

## Complete Example Program

```
function int max(int a, int b) {
    if (a > b) {
        return a;
    } else {
        return b;
    }
}

function void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        write(arr[i]);
        write(" ");
    }
    writeLine("");
}

program Example {
    int numbers[5];

    // Fill array
    for (int i = 0; i < 5; i++) {
        numbers[i] = i * 10;
    }

    // Print array
    writeLine("Array contents:");
    printArray(numbers, 5);

    // Find maximum
    int maximum = numbers[0];
    for (int i = 1; i < 5; i++) {
        maximum = max(maximum, numbers[i]);
    }

    write("Maximum value: ");
    writeLine(maximum);
}
```