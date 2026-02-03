# REGEXP

```ebnf
(* 通用的正则:interoperable regular expression *)
<i_regexp> ::= <branch> { '|' <branch> }

(***************************************)
<branch> ::= {<piece>}

(***************************************)
(* 可带限定后缀的原子 *)
<piece> ::= <atom> [<quantifier>]

(***************************************)
(* 括号:表示捕获组,子表达式 *)
(* 以\1,\2,...引用 *)
(* 以$1,$2,...引用 *)
<atom> ::= <NormalChar> 
         | <charClass> 
         | ( '(' <i_regexp> ')' )

(* '*':0到多个 *)
(* '+':1到多个 *)
(* '?':可选 *)
<quantifier> ::= ('*'|'+'|'?') 
               | <range_quantifier>

(***************************************)
(* 0x28|..|0x2b: '(' ')' '*' '+'有意义 *)
(* 0x2e:         '.'            有意义 *)
(* 0x3f:         '?'            有意义 *)
(* 0x5b|..|0x5d: '[' '\' ']'    有意义 *)
(* 0x7b|..|0x7d: '{' '|' '}'    有意义 *)

(* 普通字符:无特殊意义 *)
<NormalChar> ::= 
(  0x00|..|0x27 | ',' | '-' 
 | 0x2F|..|0x3E        (* '/'-'>' *)
 | 0x40|..|0x5A        (* '@'-'Z' *)
 | 0x5E|..|0x7A        (* '^'-'z' *)
 | 0x7E|..|0xD7FF      (* skip 0xd800-dfff *)
 | 0xE000|..|0x10FFFF
)

(* 字符类 *)
(* '.':任意单字符 *)
(* '^':首 *)
(* '$':尾 *)
<charClass> ::= '.' 
            | <SingleCharEsc>
            | <charClassEsc>
            | <charClassExpr>

(* {q} : 重复次数==q *)
(* {q,} :q<=重复次数 *)
(* {q,e} :q<=重复次数<=e *)
<range_quantifier> ::= '{' <QuantExact> [ ',' [ <QuantExact> ] ] '}'

(***************************************)
(* 单字符转义 *)
<SingleCharEsc> ::=
 '\' (  '(' | ')' | '*' | '+'
      | '-' | '.' | '?' 
      | '[' | '\' | ']' | '^'
      | 'n' | 'r' | 't'
      | '{' | '|' | '}'
     )

(* 字符类转义 *)
<charClassEsc> ::= <catEsc> | <complEsc>

(* 字符类表达式:集合 *)
(* '^':取反集 *)
(* '-':按字面值,CCE1中'-'为范围 *)
<charClassExpr> ::= '[' ['^'] ('-'|<CCE1>) {<CCE1>} ['-'] ']'

<QuantExact> ::= ('0'|..|'9') {('0'|..|'9')}

(***************************************)
(* 匹配 *)
<catEsc> ::= "\p{" <charProp> '}'

(* 匹配补集 *)
<complEsc> ::= "\P{" <charProp> '}'

(* '-':表示范围 *)
<CCE1> ::= (<CCchar> ['-' <CCchar>]) 
          | <charClassEsc>

(***************************************)
<charProp> ::= <IsCategory>

(* 0x2d        : '-'         集合中需转义 *)
(* 0x5b|..|0x5d: '[' '\' ']' 集合中需转义 *)
<CCchar> ::= 
   (   0x00|..|0x2C 
     | 0x2E|..|0x5A
     | 0x5E|..|0xD7FF
     | 0xE000|..|0x10FFFF
   )
   | <SingleCharEsc>

(***************************************)
<IsCategory> ::= <Letters>    | <Marks>
               | <Numbers>    | <Punctuation>
               | <Separators> | <Symbols> 
               | <Others>

(***************************************)

<Letters>     ::= 'L' [('l'|'m'|'o'|'t'|'u')]
<Marks>       ::= 'M' [('c'|'e'|'n')]
<Numbers>     ::= 'N' [('d'|'l'|'o')]
<Punctuation> ::= 'P' [('c'|'d'|'e'|'f'|'i'|'o'|'s')]
<Separators>  ::= 'Z' [('l'|'p'|'s')]
<Symbols>     ::= 'S' [('c'|'k'|'m'|'o')]
<Others>      ::= 'C' [('c'|'f'|'n'|'o')]

(*************
'L':Letters
 [('l':lowercase letters
  |'m':modifier letters
  |'o':other letters
  |'t':titlecase letters
  |'u':uppercase letters)]

'M':Marks
 [('c':spacing combining marks 
  |'e':enclosing marks 
  |'n':non-spacing marks)]

'N':Numbers
 [('d':decimal digit numbers 
  |'l':letter number 
  |'o':other number)]

'P':Punctuation
 [('c':connector punctuation 
  |'d':dash punctuation 
  |'e':close punctuation 
  |'f':final quote punctuation 
  |'i':Initial quote punctuation 
  |'o':other punctuation 
  |'s':open Punctuation)]

'Z':Separator
 [('l':line separator 
  |'p':paragraph separator 
  |'s':space separator)]

'S':Symbols
 [('c':currency symbol 
  |'k':modifier symbol 
  |'m':math symbol 
  |'o':other symbol)]

'C':char 
 [('c':control
  |'f':format
  |'n':unassigned 
  |'o':private use)]
*************)
```



<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>





