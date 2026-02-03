# <font color=39c5bb> JSON </font>

```ebnf
<json> ::= <element>

(***************************************)
<element> ::= <ws> <value> <ws>

(***************************************)
(* 值 *)
<value> ::=
	  <object>
	| <array>
	| <string>
	| <number>
	| "true"
	| "false"
	| "null"

(* 空白符列表 *)
<ws> ::=
	  ""          (* empty *)
	| '0020' <ws> (* space *)
	| '000A' <ws> (* \a *)
	| '000D' <ws> (* \r *)
	| '0009' <ws> (* \n *)

(***************************************)
(* 对象 *)
<object> ::=
	  '{' <ws> '}'
	| '{' <members> '}'

(* 数组 *)
<array> ::=
	  '[' <ws> ']'
	| '[' <elements> ']'

(* 串 *)
<string> ::=
	'"' <characters> '"'

(* 实数 *)
<number> ::=
	<integer> <fraction> <exponent>

(***************************************)
<members> ::= <member> {',' <member>}

<elements> ::= <element> {',' <elements>}

<characters> ::= "" | <character> <characters>

<integer> ::=
	  <digit>
	| <onenine> <digits>
	| '-' <digit>
	| '-' <onenine> <digits>

<fraction> ::= "" | '.' <digits>

<exponent> ::=
	  ""
	| 'E' <sign> <digits>
	| 'e' <sign> <digits>

(***************************************)
(* 键值对 *)
<member> ::=
	<ws> <string> <ws> ':' <element>

<character> ::=
	  '0020' . '10FFFF' - '"' - '\'
	| '\' <escape>

<digit> ::= '0' | <onenine>
<onenine> ::= '1' | .. | '9'
<digits> ::= <digit> {<digit>}

<sign> ::=
	  ""
	| '+'
	| '-'

(***************************************)
<escape> ::=
	  '"'
	| '\'
	| '/'
	| 'b'
	| 'f'
	| 'n'
	| 'r'
	| 't'
	| 'u' <hex> <hex> <hex> <hex>

(***************************************)
<hex> ::=
	  <digit>
	| 'A' | .. | 'F'
	| 'a' | .. | 'f'

```






<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>


