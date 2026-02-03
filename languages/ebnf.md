# <font color=#39c5bb>:sparkles: EBNF(Extended Backus-Naur Form) :sparkles:</font>

| 记号      | 类别       | 含义                                                                 |
|-----------|------------|----------------------------------------------------------------------|
| `=`       | 定义符     | 表示“被定义为”                                                       |
| `::=`     | 旧式定义   | 表示“被定义为”                                                       |
| `< >`     | 非终结符   | 表示“还能继续展开”                                                   |
| `;`       | 终止符     | 表示一条规则的结束                                                   |
| `\|`      | 选择符     | 表示“或”，用于分隔多个可选项                                         |
| `[ ]`     | 可选符     | 表示括号内的内容出现0次或1次(特别的可为属性元写法)                   |
| `?`       | 可选符     | 表示左边内的内容出现0次或1次                                         |
| `{ }`     | 重复符     | 表示括号内的内容出现0次或多次                                        |
| `*`       | 重复符     | 表示左边内的内容出现0次或多次                                        |
| `n *`   | 重复符     | 表示右边内的内容出现n次                                              |
| `+`       | 重复符     | 表示左边内的内容出现1次或多次                                        |
| `( )`     | 分组符     | 将多个元素组合为一个整体，改变操作符的优先级                         |
| `" "`     | 终结符界定符| 双引号包裹的字符串表示终结符（字面量）                              |
| `' '`     | 终结符界定符| 单引号包裹的字符串表示终结符（字面量），与双引号等价                |
| `(* *)`   | 注释符     | 注释块，可以嵌套                                                     |
| `? ?`     | 特殊序列   | 用于嵌入特殊信息或未定义的外部规则，内容不被视为语法的一部分         |
| `-`       | 差集符     | 表示排除                                                             |
| ` `       | 隐式连接符 | 实际连接操作符                                                       |
| `,`       | 连接符     | 连接操作符                                                           |
| `..`      | 范围符     | 字符区间,语法糖                                                      |

> partly refer to ISO/IEC 14977

```EBNF
<syntax>  ::= [<comment>]* <syntax_rule> [<comment>]*
             ,{<syntax_rule> [<comment>]*};
(* --------------------- *)

<syntax_rule>    ::= <meta_identifier>,('=' | '::='),<definitions_list>,';';

<comment> ::= '(*',{<comment_symbol>},'*)';

(* --------------------- *)

<meta_identifier>  ::= '<',<letter>,{<letter> | <decimal_digit> | '_'},'>';
<definitions_list> ::= <single_definition>,{'|',<single_definition>};

<comment_symbol> ::= <comment> | <terminal_string> | <special_sequence> | <character>;

(* --------------------- *)
<letter>        ::= ? [a-zA-Z] ?;
<decimal_digit> ::= ? [0-9] ?;

<single_definition> ::= <term>,{',',<term>};

<special_sequence>  ::= '?',{<character> - '?'},'?';
<terminal_string>   ::= "'",{<character> - "'"},"'"
                       |'"',{<character> - '"'},'"';

(* --------------------- *)

<term> ::= <factor>,['-',<exception>];

(* --------------------- *)

<factor>    ::= [<integer>,'*'],<primary>;
<exception> ::= <factor>;

(* --------------------- *)

<integer> ::= <decimal_digit>,{<decimal_digit>};

<primary> ::= <optional_sequence>
             | <repeated_sequence>
             | <special_sequence>
             | <grouped_sequence>
             | <meta-identifier>
             | <terminal_string>
             | <empty>;

(* --------------------- *)

<optional_sequence> ::= '[',<definitions_list>,']';
<repeated_sequence> ::= '{',<definitions_list>,'}';
<grouped_sequence>  ::= '(',<definitions_list>,')';


<empty> ::= ;

(* --------------------- *)

<character> ::= ? ASCII character ?;

```


<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>




























