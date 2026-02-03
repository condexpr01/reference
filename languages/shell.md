# shell

```ebnf
<program> ::= <linebreak> <complete_commands> <linebreak>
            | <linebreak>

(***************************************)
<linebreak> ::= [<newline_list>]

<complete_commands> ::= <complete_command> {(<newline_list> <complete_command>)}

(***************************************)
<newline_list> ::= {<NEWLINE>}

<complete_command> ::= <list> <separator_op>
                     | <list>

(***************************************)
<NEWLINE> ::= ? new line ?

<list> ::= <and_or> {<separator_op> <and_or>}

(* ';':顺序执行 *)
(* '&':后台执行 *)
<separator_op> ::='&'| ';'

(***************************************)
(* "&&":与 *)
(* "||":或 *)
<and_or> ::= <pipeline>
           | <and_or> "&&" <linebreak> <pipeline>
           | <and_or> "||" <linebreak> <pipeline>

(***************************************)
(* "!":非 *)
<pipeline> ::= ['!'] <pipe_sequence>

(***************************************)
(* '|':管道,lcmd的stdout作为rcmd的stdin *)
<pipe_sequence> ::= <command> {'|' <linebreak> <command>}

(***************************************)
<command> ::= <simple_command>
            | <compound_command>
            | <compound_command> <redirect_list>
            | <function_definition>

(***************************************)
<simple_command> ::= <cmd_prefix> [<cmd_word> [<cmd_suffix>]]
                   | <cmd_name>   [<cmd_suffix>]

<compound_command> ::= <brace_group>
                     | <subshell>
                     | <for_clause>
                     | <case_clause>
                     | <if_clause>
                     | <while_clause>
                     | <until_clause>

<redirect_list> ::= <io_redirect> {<io_redirect>}

(* 函数参数通过$num访问 *)
<function_definition> ::= <fname> '(' ')' <linebreak> <function_body>

(***************************************)
<cmd_prefix> ::= [<cmd_prefix>] <io_redirect>
               | [<cmd_prefix>] <ASSIGNMENT_WORD>

<cmd_word> ::= <WORD> 

<cmd_suffix> ::= [<cmd_suffix>] <io_redirect>
               | [<cmd_suffix>] <WORD>

<cmd_name> ::= <WORD>

(* 花括号语句块 *)
<brace_group> ::= '{' <compound_list> '}'

(* 启动新的子进程执行,返回退出状态码 *)
<subshell> ::= '(' <compound_list> ')'

(* for不写in等价于加上in "$@" *)
<for_clause> ::= "for" <name>                                              <do_group>
               | "for" <name>                             <sequential_sep> <do_group>
               | "for" <name> <linebreak> "in"            <sequential_sep> <do_group>
               | "for" <name> <linebreak> "in" <wordlist> <sequential_sep> <do_group>

<case_clause> ::= "case" <WORD> <linebreak> "in" <linebreak> <case_list>    "esac"
                | "case" <WORD> <linebreak> "in" <linebreak> <case_list_ns> "esac"
                | "case" <WORD> <linebreak> "in" <linebreak>                "esac"

<if_clause> ::= "if" <compound_list> "then" <compound_list> <else_part> "fi"
              | "if" <compound_list> "then" <compound_list>             "fi"

<while_clause> ::= "while" <compound_list> <do_group>

<until_clause> ::= "until" <compound_list> <do_group>

<io_redirect> ::= [<IO_NUMBER>] <io_file>
                | [<IO_NUMBER>] <io_here>

<fname> ::= <NAME> 

<function_body> ::= <compound_command> [<redirect_list>]  

(***************************************)
<ASSIGNMENT_WORD> ::= ? 赋值语句 ?
<WORD> ::= ? word ?

<compound_list> ::= <linebreak> <term> [<separator>]

<name> ::= <NAME> 

<wordlist> ::= <WORD> {<WORD>}

<sequential_sep> ::= ';' <linebreak>
                   | <newline_list>

<do_group> ::= "do" <compound_list> "done" 

<case_list> ::=     <case_item> {<case_item>}
<case_list_ns> ::= [<case_list>] <case_item_ns>

<else_part> ::= "elif" <compound_list> 'then' <compound_list>
              | "elif" <compound_list> "then" <compound_list> <else_part>
              | "else" <compound_list>


<IO_NUMBER> ::= ? fd(0:stdin,1:stdout,2:stderr) ?

(* '<' file :读 *)
(* '>' file :写 *)
(* '>>' file :写追加 *)
(* '<>' file :读写 *)
(* '>|' file :强制覆盖 *)
(* '<&' fd: 文件描述符fd作为输入 *)
(* '>&' fd: 文件描述符fd作为输出 *)
<io_file> ::= '<'  <filename>
            | '>'  <filename>
            | "<&" <filename>
            | ">&" <filename>
            | ">>" <filename>
            | "<>" <filename>
            | ">|"   <filename>

(* '<<' :原样输入 *)
(* '<<-' :原样输入,忽略行首\t *)
<io_here> ::= "<<"  <here_end>
            | "<<-" <here_end>

<NAME> ::= ? name ?

(***************************************)
<term> ::= <and_or> {<separator> <and_or>}
<separator> ::= <separator_op> <linebreak>
              | <newline_list>

(*";;":相当于c/cpp里的break*)
<case_item> ::=        <pattern> ')' <linebreak>     ";;" <linebreak>
                 |     <pattern> ')' <compound_list> ";;" <linebreak>
                 | '(' <pattern> ')' <linebreak>     ";;" <linebreak>
                 | '(' <pattern> ')' <compound_list> ";;" <linebreak>

<case_item_ns> ::=     <pattern> ')' <linebreak>
                 |     <pattern> ')' <compound_list>
                 | '(' <pattern> ')' <linebreak>
                 | '(' <pattern> ')' <compound_list>



<filename> ::= <WORD> 

<here_end> ::= <WORD>

(***************************************)

<pattern> ::= <WORD> {'|' <WORD>}
```


# 权限

```shell
普通文件(-)
目录文件(d)
字符设备(c)
块设备(b)
符号链接文件(l)
管道文件(p)

-rwx:无读写执行，2进制位表示，3个二进制位凑8进制

u:owner
g:group
o:other
a:all ugo

<permission> ::= <type> <owner> <group owner> <other>
<type> ::= '-'|'d'|'c'|'b'|'l'|'p'

<owner> ::= 000 | ... | 777
<group_owner> ::= 000 | ... | 777
<other> ::= 000 | ... | 777
```

# 掩码

```shell
文件:基准0666, 令0666 & ~$(umask)为最终权限
目录:基准0777, 令0777 & ~$(umask)为最终权限
```


<font color=#ff0044>
<center>Written by Vito Devlin:tada:</center>
<center>condexpr01@outlook.com</center>
</font>










