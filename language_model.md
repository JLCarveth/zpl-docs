# Zero Language Model
### Regular Expression Lexeme Classes
|||
|---|---|
|L = [A-Za-z]|Letters|
|D = [0-9]|Digits|
|P = .|Decimal Point (For floating point literals)|
|Q = "|String delimeter|
|C = '|Character delimeter|
|E = EOFS| End-of-file sentinel character|
|O = [^LDPQCE]|Other characters |
***
### Regular Expressions
|Name|Description|Expression|Examples of Accepted Strings|
|---|---|---|---|
|Identifiers|Identifiers are used for naming variables, and must begin with a letter. Identifiers can contain letters and digits only.| `L(D\|L)*`|`a, someVar123, B2`|
|String Literal|A string literal is a sequence of 0 or more characters enclosed within double quotes "".|`Q(^EQ)*Q`|`"Hello World!","abc123", "8233gug821313uy"`|
|Integer Literal|Currently only supports base-10 (decimal) encoding of integer numbers, though octal and hexadecimal encoding wouldn't be too difficult to implement in the future. |`D+`|`123456, 99999999, 1, 0`|
|Floating-Point Literal|A floating point literal consists of an optional sign preceeding one or more digits, a point, one or more digits, and an optional exponent section. The exponent section consists of an upper or lower case 'E' followed by an optional sign and one or more digits.|`-?D+PD+((E\|E)-?D+)?`|`1.00, 1.25e12, -0.245e-2, .5`|
|Character Literal|A character literal consists of a character surrounded by single quotes ''.|`C(^EC)C`|`'a', 'A', '1', '{', '_', '?'`|  
***
## Keywords
There are numerous keywords reserved for the use of the language. Thus, identifiers cannot have the same name as one of the reserved keywords.
|Keyword|Purpose|
|---|---|
|**Data Types**||
|`byte`|8-bit signed integer type|
|`short`|16-bit signed integer type|
|`int`|32-bit signed integer type|
|`long`|64-bit signed integer type|
|`float`|32-bit IEEE 754 floating-point type|
|`double`|64-bit IEEE 754 floating-point type|
|`char`|Character type (single ASCII character)|
|`bool`|Boolean type (true/false)|
|`void`|Used for functions that do not return a value|
|**Control Flow**||
|`if`,`else`|Conditional statement keywords|
|`do`,`while`|Loop keywords for do-while and while loops|
|`for`|For loop keyword|
|`switch`,`case`,`break`,`continue`, `default`|Switch statement keywords|
|**Program Structure**||
|`program`|Indicates the entry point of a ZPL program|
|`function`|Keyword for function declarations|
|`return`|Keyword for returning a value from a function|
|**Input/Output**||
|`write`,`writeLine`|Used for writing to output stream (writeLine adds newline)|
|`read`,`readLine`|Used for reading from input stream (read: single char, readLine: entire line)|

**Note:** The following keywords have been removed from the language to maintain simplicity: `null`, `enum`, `class`, `new`, `foreach`, `boolean` (replaced with `bool`), `String` (use `char[]` arrays instead).