# The Zero Programming Language Formal Specification  
## 1.0 Introduction
This document outlines the language specification for the ***Zero Programming Language***. 
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
<comment> -> // { one or more ASCII characters }
           | /* { one or more ASCII chars or newline } */

```
### 1.1.3 - Identifiers
Identifiers are used to uniquely identify variables, functions, classes, and enumerations. Identifiers produce the `ID_T` token when detected by the scanner.
```
<variable-identifier> -> ID_T
```
### 1.1.4 - Keywords
Keywords are defined by the token code `KW_T`, with the specific keyword being saved as an index to the ketword table. 
```
<keyword> -> { "byte" , "short" ,"int", "long", "float", "double", "char", "boolean", "String", "void", "null", "enum", "class", "new", "if", "else", "do", "while", "for", "foreach", "switch", "case", "break", "continue", "default", "program", "function", "return", "write", "writeLine", "read", "readLine" } -> KW_T
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
<separator> = one of { } [ ] ( ) , . : ; -> SEP_T
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
<arithmetic_operator> -> one of { +, -, /, * }
```
Logical operators produce a `LOG_OP_T` token.
```
<logical_operator> -> one of { &&, ||, ! }
```
Relational operators produce the `REL_OP_T` token.
```
<relational_operator> -> one of { <, >, <=, >=, ==, != }
```
The assignment operator produces the `ASS_OP_T` token.
```
<assignment_operator> -> =
```
## 2.0 ZPL Syntactic Specification
### 2.1.1 ZPL Program
A ZPL program consists of the `program` keyword, an identifier, and one or more statements contained within curly-braces:
```
<program> -> program <identifier> { <statements> }
```
**First Set:** `FIRST(<program>) = { KW_T(program) }`
## 2.2 Variable Declarations
Variable declarations must have a data type and an identifier. Multiple variables can be declared in a list by separating the identifiers with a comma.
```
<variable-declaration> -> <datatype> <varlist>
<datatype> -> short | int | long | float | double | String | char | ID_T
<varlist> -> <varlist> , <identifier> | <identifier>
```
In the above grammar, `<varlist>` has a left-recursion we must resolve:
```
<varlist> -> <identifier> <varlistPrime>
<varlistPrime> -> , <identifier> <varlistPrime>
                | ε
```

**First Set:** `FIRST(<variable-declaration>) = { KW_T, ID_T }`  

## 2.3 Statements
```
<opt-statements> -> <statements> | ε
<statements> -> <statement> | <statements> <statement>
<statement> -> <assignment-statement>
              | <selection-statement>
              | <input-statement>
              | <output-statement>
```
**First Set:** `FIRST(<statements>) = { ID_T, KW_T }`
### Assignment Statement
An assignment statement is used to assign a value to a variable. An assignment statement is composed of an assignment expression:
```
<assignment-statement> -> <assignment-expression>
<assignment-expression> -> <identifier> = <expression>
```
**First Set:** `FIRST(<assignment-statement>) = { ID_T }`
### Selection Statements
ZPL supports `if/else` and `switch` selection statements. 
```
<selection-statement>   -> <if-statement>
                          | <if-else-statement>
                          | <switch-statement>
<if-statement>          -> if (<conditional-expression>) { <opt-statements> }
<else-statement>        -> else { <opt-statements> }
<if-else-statement>     -> <if-statement> <else-statement>
<if-else-statement>     -> <if-statement> else <if-else-statements>
<if-else-statements>    -> <if-statement> | <if-else-statements> else <if-statement>
<switch-statement>      -> switch (<identifier>) <switch-block>
<switch-block>          -> { <case-blocks> }
<case-blocks>           -> <case-block> | <case-blocks> <case-block>
<case-block>            -> case <constant> : <opt-statements>
                          | default : <opt-statements>
```
**First Set:** `FIRST(<selection-statement>) = { KW_T }`
### Iteration Statements
ZPL supports `do/while` loops, as well as `for` and `foreach` loops.
```
<iteration-statement>   -> <while-statement>
                         | <do-while-statement>
                         | <for-statement>
                         | <foreach-statement>
<while-statement>       -> while (<conditional-expression>) { <opt-statements> }
<do-while-statement>    -> do <statement> while (<conditional-expression>) ;
<for-statement>         -> for ( <assignment-expression>; <conditional-expression>; <arithmetic-expression> ) { <opt-statements> }
<foreach-statement>     -> foreach (<identifier> in <list-group>) { <opt-statements> }
<list-group>            -> <identifier> | <varlist>
```
**First Set:** `FIRST(<iteration-statement>) = { KW_T }`
### Input Statement
Input is performed by using the `read` or `readLine` reserved keyword. The value read from the input stream is stored in the variable on the left side of the assignment expression.
```
<input-statement>       -> <left-side> = read();
                         | <left-side> = readLine();
<left-side>             -> <identifier> | <variable-declaration>
```
**First Set:** `FIRST(<input-statement>) = { ID_T, KW_T }`
### Output Statement
Similar to the input statement, reserved keywords `write` and `writeLine` are used to write to an output stream.
```
<output-statement> -> write(STR_T) | write(<identifier>)
```
## 2.4 Expressions
### 2.4.1 Arithmetic Expressions
```
<expression>     -> <term> + <expression>
                  | <term> - <expression>
                  | <term>
<term>           -> <factor> * <term>
                  | <factor> / <term>
                  | <factor>
<factor>         -> (<expression>)
                  | - <expression>
                  | <literal>
<literal>         -> INL_T | FPL_T
```

### 2.4.2 String Expressions
```
<string-expression> -> <primary-string-expression> + <string-expression>
<primary-string-expression> -> <string-variable> | SL_T
```
### 2.4.3 Conditional Expressions
```
<conditional-expression> -> <logical-OR-expression>
<logical-OR-expression> -> <logical-AND-expression>
                        | <logical-OR-expression> || <logical-AND-expression>
<logical-AND-expression> -> <logical-NOT-expression>
                        | <logical-AND-expression> && <logical-NOT-expression>
<logical-NOT-expression -> ! <relational-expression>
                        | <relational-expression>
<relational-expression> -> <primary-relational-exp> == <primary-relational-exp>
                        | <primary-relational-exp> != <primary-relational-exp>
                        | <primary-relational-exp> < <primary-relational-exp>
                        | <primary-relational-exp> > <primary-relational-exp>
                        | <primary-relational-exp> <= <primary-relational-exp>
                        | <primary-relational-exp> >= <primary-relational-exp>
<primary-relational-exp> -> <integer-variable> | <float-variable> | INL_T | FPL_T
```
