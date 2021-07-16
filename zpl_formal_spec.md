# The Zero Programming Language Formal Specification  
## 1.1 - ZPL Basic Elements
### 1.1.1 - Whitespace
While there are more characters recognized by C/ASCII as whitespace, they serve no function in modern programs and thus will not be supported by the language.
```
<whitespace> -> SPACE | TAB | NEWLINE
```
Whitespace characters are ignored by the compiler, and is only necessary when separating tokens.
### 1.1.2 - Comments
ZPL supports both single line comments beginning with `//`, and multiline comments surrounded with `/* */`.
```
<comment> -> // { sequence of ASCII chars }
        |    /* { sequence of ASCII chars } */
```
### 1.1.3 - Identifiers
Identifiers are used to uniquely identify variables, functions, classes, and enumerations. Identifiers produce the `ID_T` token when detected by the scanner.
### 1.1.4 - Keywords
Keywords are defined by the token code `KW_T`, with the specific keyword being saved as an index to the ketword table. 
```
<keyword> -> "byte" | "short" | "int" | "long" | "float" | "double" | "char" | "boolean" | "String" | "void" | "null" | "enum" | "class" | "new" | "if" | "else" | "do" | "while" | "for" | "foreach" | "switch" | "case" | "break" | "continue" | "default" | "program" | "function" | "return" | "write" | "writeLine" | "read" | "readLine"
```
### 1.1.5 - Integer Literals
The scanner recognizes integer literals as a token of type `INT_T`. The value of the integer is stored as an attribute.
```
<integer-literal> -> INL_T
```
### 1.1.6- Floating Point Literals
The scanner recognizes floating point literals as a token of type `FPL_T`, with the value of the floating point being stored as the `floatValue` attribute.
```
<floating-point-literal> -> FPL_T
```
### 1.1.7 - String Literals
The scanner recognizes string literals with the `STR_T` token.
```
<string-literal> -> SL_T
```
### 1.1.8 - Character Literals
Character literals recognized by the `CL_T` token.
```
<character-literal> -> CL_T
```
### 1.1.9 - Separators
```
<separator> -> one of { } [ ] ( ) , . : ;
```
Separators are recognized as a `SEP_T` token, with the specific separator type specified by an enum attribute. 
### 1.1.10 - Operators
```
<operator> -> <arithmetic_operator>
            | <logical_operator>
            | <relational_operator>
            | <assignment_operator>
```
Arithmetic operators produce an `ART_T` token.
```
<arithmetic_operator> -> { +, -, /, * }
```
Logical operators produce a `LOG_OP_T` token.
```
<logical_operator> -> { &&, ||, ! }
```
Relational operators produce the `REL_OP_T` token.
```
<relational_operator> -> { <, >, <=, >=, ==, != }
```
The assignment operator produces the `ASS_OP_T` token.
```
<assignment_operator> -> =
```