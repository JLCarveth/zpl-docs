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
|`byte`|Used for declaring a variable of type `Byte`|
|`short`|Used for declaring a variable of type `Short`|
|`int`|Used for declaring a variable of type `Integer`|
|`long`|Used for declaring a variable of type `Long`|
|`float`|Used for declaring a variable of type `Float`|
|`double`|Used for declaring a variable of type `Double`|
|`char`|Used for declarign a variable of type `Char`|
|`boolean`|Used for declaring a variable of type `Boolean`|
|`String`|Used for declaring a variable of type `String`|
|`void`|Used to refer to a lack of a type, more specifically the lack of a return type from a function. |
|`null`|The nullable type, representing the special value NULL.|
|`enum`|Reserved for future use.|
|`class`|Reserved for future use.|
|`new`|Reserved for future use.|
|**Loops / Flow-Control**||
|`if`,`else`|Traditional conditional loop keywords.|
|`do`,`while`|Do & Do-while loop keywords.|
|`for`, `foreach`|For creating loops over a collection or a specific range.|
|`switch`,`case`,`break`,`continue`, `default`|Keywords required when writing switch statements.|
|**Program Functions**||
|`program`|Indicates the starting point of a Zero program.|
|`function`|Keyword for function declarations.|
|`return`|Keyword for returning a value from a function.|
|`write`,`writeLine`|Used for writing information to an output stream (ie., the command line.)|
|`read`,`readLine`|Used for reading either a single character or an entire line from an input stream respectively.|