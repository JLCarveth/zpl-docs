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
Identifiers are used to uniquely identify variables, functions, and program names. Identifiers produce the `ID_T` token when detected by the scanner.
```
<identifier> -> ID_T
```
### 1.1.4 - Keywords
Keywords are defined by the token code `KW_T`, with the specific keyword being saved as an index to the keyword table.
```
<keyword> -> { "byte" , "short" ,"int", "long", "float", "double", "char", "bool", "void", "if", "else", "do", "while", "for", "switch", "case", "break", "continue", "default", "program", "function", "return", "write", "writeLine", "read", "readLine" } -> KW_T
```
**Note:** Keywords `enum`, `class`, `new`, `null`, and `foreach` have been removed or deferred for future implementation to keep the language simple and implementable.
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
            | <unary_operator>
```
Arithmetic operators produce an `ART_OP_T` token.
```
<arithmetic_operator> -> one of { +, -, /, *, % }
```
Unary operators produce a `UNA_OP_T` token.
```
<unary_operator> -> one of { ++, -- }
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
### 2.1 ZPL Program Structure
A ZPL program consists of zero or more function declarations followed by a required `program` block:
```
<program-unit>          -> <function-declarations> <program-block>
<function-declarations> -> <function-declaration> <function-declarations> | ε
<program-block>         -> program <identifier> { <statements> }
```
**First Set:** `FIRST(<program-unit>) = { KW_T(function), KW_T(program) }`

The `program` block serves as the entry point of the program, similar to `main()` in C.

## 2.2 Function Declarations
Functions are declared using the `function` keyword, followed by a return type, identifier, parameter list, and body.
```
<function-declaration>  -> function <datatype> <identifier> ( <parameter-list> ) { <statements> }
<parameter-list>        -> <parameter> <parameter-list-tail> | ε
<parameter-list-tail>   -> , <parameter> <parameter-list-tail> | ε
<parameter>             -> <datatype> <identifier> <opt-array-param>
<opt-array-param>       -> [ ] | ε
```
**First Set:** `FIRST(<function-declaration>) = { KW_T(function) }`

**Note:** Array parameters use `[]` without a size. The `void` type is used for functions that do not return a value.

## 2.3 Variable Declarations
Variable declarations must have a data type and an identifier. Variables can optionally be initialized with a value. Arrays are declared by specifying a size in square brackets.
```
<variable-declaration> -> <datatype> <identifier> <opt-array-specifier> <opt-initializer> ;
<datatype> -> byte | short | int | long | float | double | char | bool
<opt-array-specifier> -> [ INL_T ] | ε
<opt-initializer> -> = <expression> | ε
```

**First Set:** `FIRST(<variable-declaration>) = { KW_T }`

**Note:** Multiple variable declarations in a single statement have been removed for simplicity. Arrays must have a compile-time constant size.

## 2.4 Statements
```
<opt-statements> -> <statements> | ε
<statements> -> <statement> | <statements> <statement>
<statement> -> <variable-declaration>
              | <assignment-statement>
              | <selection-statement>
              | <iteration-statement>
              | <input-statement>
              | <output-statement>
              | <function-call> ;
              | <return-statement>
              | break ;
              | continue ;
```
**First Set:** `FIRST(<statements>) = { ID_T, KW_T }`

Note: Left recursion in `<statements>` can be resolved during parsing or left as-is if using bottom-up parsing.
### 2.4.1 Assignment Statement
An assignment statement is used to assign a value to a variable or array element.
```
<assignment-statement> -> <lvalue> = <expression> ;
<lvalue> -> <identifier> | <identifier> [ <expression> ]
```
**First Set:** `FIRST(<assignment-statement>) = { ID_T }`
### 2.4.2 Selection Statements
ZPL supports `if/else` and `switch` selection statements.
```
<selection-statement>   -> <if-statement> | <switch-statement>
<if-statement>          -> if ( <expression> ) { <opt-statements> } <opt-else-part>
<opt-else-part>         -> else { <opt-statements> }
                          | else <if-statement>
                          | ε
<switch-statement>      -> switch ( <expression> ) { <case-blocks> }
<case-blocks>           -> <case-block> <case-blocks> | ε
<case-block>            -> case <constant> : <opt-statements>
                          | default : <opt-statements>
<constant>              -> INL_T | FPL_T | CL_T
```
**First Set:** `FIRST(<selection-statement>) = { KW_T(if), KW_T(switch) }`
### 2.4.3 Iteration Statements
ZPL supports `while`, `do/while`, and `for` loops.
```
<iteration-statement>   -> <while-statement>
                         | <do-while-statement>
                         | <for-statement>
<while-statement>       -> while ( <expression> ) { <opt-statements> }
<do-while-statement>    -> do { <opt-statements> } while ( <expression> ) ;
<for-statement>         -> for ( <for-init> ; <expression> ; <for-update> ) { <opt-statements> }
<for-init>              -> <variable-declaration>
                         | <assignment-statement>
                         | ε
<for-update>            -> <assignment-statement>
                         | <identifier> ++
                         | <identifier> --
                         | ε
```
**First Set:** `FIRST(<iteration-statement>) = { KW_T(while), KW_T(do), KW_T(for) }`

**Note:** `foreach` has been removed for simplicity.
### 2.4.4 Input Statement
Input is performed by using the `read` or `readLine` keywords. These functions return a value that must be assigned to a variable.
```
<input-statement>       -> <lvalue> = read ( ) ;
                         | <lvalue> = readLine ( ) ;
```
**First Set:** `FIRST(<input-statement>) = { ID_T }`

**Note:** `read()` reads a single character, `readLine()` reads an entire line as a string (stored as char array).

### 2.4.5 Output Statement
Keywords `write` and `writeLine` are used to write to an output stream.
```
<output-statement>      -> write ( <expression> ) ;
                         | writeLine ( <expression> ) ;
```
**First Set:** `FIRST(<output-statement>) = { KW_T(write), KW_T(writeLine) }`

### 2.4.6 Return Statement
The `return` statement is used to exit a function and optionally return a value.
```
<return-statement>      -> return <opt-expression> ;
<opt-expression>        -> <expression> | ε
```
**First Set:** `FIRST(<return-statement>) = { KW_T(return) }`
## 2.5 Expressions
The following grammar defines expressions with proper operator precedence (from lowest to highest: logical OR, logical AND, relational, additive, multiplicative, unary, primary).

```
<expression>                -> <logical-or-expression>

<logical-or-expression>     -> <logical-and-expression>
                             | <logical-or-expression> || <logical-and-expression>

<logical-and-expression>    -> <relational-expression>
                             | <logical-and-expression> && <relational-expression>

<relational-expression>     -> <additive-expression>
                             | <additive-expression> < <additive-expression>
                             | <additive-expression> > <additive-expression>
                             | <additive-expression> <= <additive-expression>
                             | <additive-expression> >= <additive-expression>
                             | <additive-expression> == <additive-expression>
                             | <additive-expression> != <additive-expression>

<additive-expression>       -> <multiplicative-expression>
                             | <additive-expression> + <multiplicative-expression>
                             | <additive-expression> - <multiplicative-expression>

<multiplicative-expression> -> <unary-expression>
                             | <multiplicative-expression> * <unary-expression>
                             | <multiplicative-expression> / <unary-expression>
                             | <multiplicative-expression> % <unary-expression>

<unary-expression>          -> <postfix-expression>
                             | - <unary-expression>
                             | ! <unary-expression>
                             | ++ <identifier>
                             | -- <identifier>

<postfix-expression>        -> <primary-expression>
                             | <identifier> ++
                             | <identifier> --
                             | <identifier> [ <expression> ]
                             | <function-call>

<primary-expression>        -> <identifier>
                             | INL_T
                             | FPL_T
                             | CL_T
                             | SL_T
                             | ( <expression> )

<function-call>             -> <identifier> ( <argument-list> )
<argument-list>             -> <expression> <argument-list-tail> | ε
<argument-list-tail>        -> , <expression> <argument-list-tail> | ε
```

**Operator Precedence** (highest to lowest):
1. Primary expressions (literals, identifiers, parentheses)
2. Postfix operators (++, --, array indexing, function calls)
3. Unary operators (-, !, prefix ++ and --)
4. Multiplicative (*, /, %)
5. Additive (+, -)
6. Relational (<, >, <=, >=, ==, !=)
7. Logical AND (&&)
8. Logical OR (||)

**Associativity:**
- Most operators are left-associative
- Unary operators are right-associative
- Assignment is right-associative (handled separately in assignment-statement)

## 3.0 Grammar Notes and Parsing Considerations

### 3.1 Left Recursion
The following productions contain left recursion, which must be handled appropriately:
- `<statements>` (line 105)
- `<logical-or-expression>` (line 193-194)
- `<logical-and-expression>` (line 196-197)
- `<additive-expression>` (line 207-209)
- `<multiplicative-expression>` (line 211-214)

**For Top-Down Parsing (LL):** These productions must be transformed to eliminate left recursion using standard transformation techniques (e.g., introducing new non-terminals with right recursion).

**For Bottom-Up Parsing (LR, LALR):** Left recursion is handled naturally and does not require transformation.

### 3.2 Ambiguities
The grammar has been designed to minimize ambiguities:

1. **Operator Precedence:** Properly enforced through grammar hierarchy
2. **Dangling Else:** Resolved by grammar structure - else associates with nearest if
3. **Expression vs Statement:** Clear distinction through different production rules

### 3.3 Parsing Strategy Recommendation
**Recommended:** Use an LALR(1) or LR(1) parser generator (like Yacc/Bison) which:
- Handles left recursion naturally
- Provides good error messages
- Supports the grammar as written

**Alternative:** For hand-written recursive descent (LL), transform left-recursive productions to right-recursive equivalents.

### 3.4 Semantic Considerations
The following must be checked during semantic analysis (not by grammar alone):
- Type checking for expressions and assignments
- Function return type consistency
- Array bounds (size must be positive constant)
- Break/continue only within loops
- Variable declaration before use
- Function declaration before call
- No duplicate identifiers in same scope

### 3.5 Example Program
```
function int factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

program FactorialTest {
    int num;
    writeLine("Enter a number: ");
    num = read();
    int result;
    result = factorial(num);
    writeLine("Factorial is: ");
    writeLine(result);
}
```
