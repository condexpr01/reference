
# python

```ebnf
(* py文件 *)
<file> ::= [<statements>] <ENDMARKER> 

(* 交互下interactive *)
<interactive> ::= <statement_newline> 

(* eval()函数里写法 *)
<eval> ::= <expressions> {<NEWLINE>} <ENDMARKER> 

(* 函数类型,表示接收的参数和返回的类型,py3.13+ *)
<func_type> ::= '(' [<type_expressions>] ')'
                '->' <expression> {<NEWLINE>} <ENDMARKER>

(***************************************)
<statements> ::= <statement> {<statement>} 

<statement_newline> ::=
      <single_compound_stmt> <NEWLINE> 
    | <simple_stmts>
    | <NEWLINE> 
    | <ENDMARKER> 

<expressions> ::=
      <expression> {',' <expression>} [','] 


(* 表达式：a if condexpr else b *) 
(* 表示如果condexpr成立则表示式值为a *)
(* 表示如果condexpr不成立则表示值为b *)
<expression> ::=
      <disjunction> 'if' <disjunction> 'else' <expression> 
    | <disjunction>
    | <lambdef>

(* func_type的参数列表 *)
(* '*' :表示tuple解包,位置参数展开 *)
(* '**':表示dict解包,关键字参数展开 *)
<type_expressions> ::=
      '*'  <expression> 
    | '**' <expression> 
    | '*'  <expression> ',' '**' <expression> 
    | <expression> {',' <expression>} 
    | <expression> {',' <expression>} ',' '*'  <expression> 
    | <expression> {',' <expression>} ',' '**' <expression> 
    | <expression> {',' <expression>} ',' '*'  <expression> ',' '**' <expression> 

<ENDMARKER> ::= ? 词法标记,表示输入文件结束EOF ?
<NEWLINE>   ::= ? 词法标记,表示实际的换行\n ?

(***************************************)
<statement> ::=
      <compound_stmt> 
    | <simple_stmts> 

<single_compound_stmt> ::= <compound_stmt> 

(* 可类似c/cpp用;分隔 *)
<simple_stmts> ::=
      <simple_stmt> {';' <simple_stmt>} [';'] <NEWLINE> 

(* 析取 *)
<disjunction> ::= <conjunction> {'or' <conjunction> } 

(* lambda表达式 *)
<lambdef>::= 'lambda' [<lambda_params>] ':' <expression> 

(***************************************)
<simple_stmt> ::=
      <assignment>
    | <type_alias>
    | <star_expressions> 
    | <return_stmt>
    | <import_stmt>
    | <raise_stmt>
    | <pass_stmt>
    | <del_stmt>
    | <yield_stmt>
    | <assert_stmt>
    | <break_stmt>
    | <continue_stmt>
    | <global_stmt>
    | <nonlocal_stmt>

<compound_stmt> ::=
      <function_def>
    | <if_stmt>
    | <class_def>
    | <with_stmt>
    | <for_stmt>
    | <try_stmt>
    | <while_stmt>
    | <match_stmt>

(* 合取 *)
<conjunction>::= <inversion> {'and' <inversion>} 

<lambda_params> ::= <lambda_parameters>

(***************************************)

(* ':'后expression为类型注解 *)
<assignment> ::=
      <NAME> ':' <expression> ['=' <annotated_rhs> ] 
    | ('(' <single_target> ')' | <single_subscript_attribute_target>)
      ':' <expression> ['=' <annotated_rhs> ] 
    | <star_targets> '=' {<star_targets> '='} <annotated_rhs> [<TYPE_COMMENT>] 
    | <single_target> <augassign> <annotated_rhs> 

(* 类型别名 *)
<type_alias> ::=
      "type" <NAME> [<type_params>] '=' <expression> 

<star_expressions> ::=
      <star_expression> {',' <star_expression>} [','] 

(* 返回语句 *)
<return_stmt> ::=
      'return' [<star_expressions>] 

(* 模块导入 *)
<import_stmt> ::=
      <import_name>
    | <import_from>

(* 抛出异常 *)
<raise_stmt> ::=
      'raise' <expression> ['from' <expression> ] 
    | 'raise' 

(* 空语句 *)
<pass_stmt> ::= 'pass' 

(* 删除 *)
<del_stmt> ::= 'del' <del_targets> 

(* yield表达式:函数是对象,可通过next和send成员控制yield *)
<yield_stmt> ::= <yield_expr> 

(* 静态断言 *)
<assert_stmt> ::= 'assert' <expression> [',' <expression>] 

<break_stmt> ::= 'break' 
<continue_stmt> ::= 'continue' 

(* 读取全局变量 *)
<global_stmt> ::= 'global' <NAME> {',' <NAME>} 

(* 读取非局部变量 *)
<nonlocal_stmt> ::= 'nonlocal' <NAME> {',' <NAME>}

(* 函数定义,可带装饰器 *)
(* 带装饰器相当于:decorator(func)(*args,**kwargs) *)
<function_def> ::= [<decorators>] <function_def_raw> 

(* if语句 *)
<if_stmt> ::=
      'if' <named_expression> ':' <block>  <elif_stmt> 
    | 'if' <named_expression> ':' <block> [<else_block>] 

(* 类定义,可带装饰器 *)
(* 带装饰器相当于:decorator(func)(*args,**kwargs) *)
<class_def> ::= [<decorators>] <class_def_raw> 

(* 自动调用with_item的__enter__()和__exit__()方法 *)
(* 异步需要加上async *)
<with_stmt> ::=
      'with' '(' <with_item> {',' <with_item>} [','] ')' ':' [<TYPE_COMMENT>] <block> 
    | 'with'     <with_item> {',' <with_item>}           ':' [<TYPE_COMMENT>] <block> 
    | 'async' 'with' '(' <with_item> {',' <with_item>} [','] ')' ':' <block> 
    | 'async' 'with'     <with_item> {',' <with_item>}  ':' [<TYPE_COMMENT>] <block> 

(* 异步需要加上async *)
(* range(start,stop=None,step=1) *)
<for_stmt> ::=
      'for' <star_targets> 'in' <star_expressions>
            ':' [<TYPE_COMMENT>] <block> [<else_block>] 
    | 'async' 'for' <star_targets> 'in' <star_expressions> 
            ':' [<TYPE_COMMENT>] <block> [<else_block>] 

(* try块exception处理 *)
<try_stmt> ::=
      'try' ':' <block> <finally_block> 
    | 'try' ':' <block> 
      <except_block> {<except_block>}
     [<else_block>] [<finally_block>] 
    | 'try' ':' <block>
      <except_star_block> {<except_star_block>}
     [<else_block>] [<finally_block>] 

(* 循环 *)
<while_stmt> ::=
      'while' <named_expression> ':' <block> [<else_block>] 

(* match类似c/cpp switch,但能结构化的匹配 *)
<match_stmt> ::=
      "match" <subject_expr> ':' <NEWLINE> <INDENT>
      <case_block> {<case_block>} <DEDENT> 

(* 取反 *)
<inversion> ::= ['not'] <inversion> 

(* lambda表达式参数 *)
(* slash'/':标记前面的参数位置传递,不能以关键字传递 *)
<lambda_parameters> ::=
      <lambda_slash_no_default> 
      {<lambda_param_no_default>} {<lambda_param_with_default>}
      [<lambda_star_etc>] 
    | <lambda_param_no_default> 
      {<lambda_slash_no_default>} {<lambda_param_with_default>}
      [<lambda_star_etc>] 
    | <lambda_slash_with_default> {<lambda_param_with_default>}
      [<lambda_star_etc>] 
    | <lambda_param_with_default> {<lambda_param_with_default>}
      [<lambda_star_etc>] 
    | <lambda_star_etc>

(***************************************)
<annotated_rhs> ::= <yield_expr> | <star_expressions>

<single_target> ::=
      <single_subscript_attribute_target>
    | <NAME> 
    | '(' <single_target> ')' 

(* 成员或下标 *)
<single_subscript_attribute_target> ::=
      <t_primary> '.' <NAME> 
    | <t_primary> '[' <slices> ']' 

<star_targets> ::= <star_target> {',' <star_target>} [','] 

<TYPE_COMMENT> ::= ? "#type:" <type_expressions> ?

<augassign> ::=
      '+=' 
    | '-=' 
    | '*=' 
    | '/=' 
    | '%=' 
    | '&=' 
    | '|=' 
    | '^=' 
    | '<<=' 
    | '>>=' 
    | '@=' (* 矩阵乘等 *)
    | '**=' (* 幂等 *)
    | '//=' (* 整除等 *)

(* 类型模板参数 *)
<type_params> ::= '[' <type_param_seq> ']' 

<star_expression> ::=
      '*' <bitwise_or> 
    | <expression>

(* 模块导入 *)
(* dot'.':表示相对位置 .当前 ..上级 ...上上级 *)
(* import_from_targets为*:导入的全部 *)
<import_name> ::= 'import' <dotted_as_names> 
<import_from> ::=
      'from' {('.' | '...')} <dotted_name>  'import' <import_from_targets> 
    | 'from'  ('.' | '...') {('.' | '...')} 'import' <import_from_targets> 

<del_targets> ::= <del_target> {',' <del_target>} [','] 

(* yield from要expression为迭代器 *)
<yield_expr> ::=
      'yield' 'from' <expression> 
    | 'yield' [<star_expressions>] 

(* 装饰器,decorator(func)(*args,*kwargs) *)
<decorators> ::= ('@' <named_expression> <NEWLINE> )
                {('@' <named_expression> <NEWLINE> )} 

(* 无装饰器函数原始定义 *)
<function_def_raw> ::=
      'def' <NAME> [<type_params>] '(' [<params>] ')'
      ['->' <expression> ] ':' [<func_type_comment>] <block> 
    | 'async' 'def' <NAME> [<type_params>] '(' [<params>] ')'
      ['->' <expression> ] ':' [<func_type_comment>] <block> 

<named_expression> ::=
      <assignment_expression>
    | <expression>

<subject_expr> ::=
    | <star_named_expression> ',' [<star_named_expressions>] 
    | <named_expression>

<block> ::=
      <NEWLINE> <INDENT> <statements> <DEDENT> 
    | <simple_stmts>

<elif_stmt> ::=
      'elif' <named_expression> ':' <block>  <elif_stmt> 
    | 'elif' <named_expression> ':' <block> [<else_block>] 

<else_block> ::= 'else' ':' <block>

(* 和cpp类似,有构造析构调用,__init__,__del__,__call__等,也可以定义运算 *)
<class_def_raw> ::=
      'class' <NAME> [<type_params>] ['(' [<arguments>] ')' ] ':' <block> 

<with_item> ::=
      <expression> 'as' <star_target>
    | <expression> 

(* finally_block无条件,总是执行 *)
<finally_block> ::= 'finally' ':' <block> 

(* try的except捕获块 *)
<except_block> ::=
      'except' <expression> ':' <block> 
    | 'except' <expression> 'as' <NAME> ':' <block> 
    | 'except' <expressions> ':' <block> 
    | 'except' ':' <block> 
<except_star_block> ::=
      'except' '*' <expression> ':' <block> 
    | 'except' '*' <expression> 'as' <NAME> ':' <block> 
    | 'except' '*' <expressions> ':' <block> 

<INDENT> ::= ? 词法记号,缩进增加一级 ?
<DEDENT> ::= ? 词法记号,缩进减少一级 ?

(* match的case块 *)
<case_block> ::=
   "case" <patterns> [<guard>] ':' <block> 

<comparison> ::= <bitwise_or> {<compare_op_bitwise_or_pair>} 

(* 无默认参数 *)
<lambda_param_no_default> ::= <lambda_param> [',']

<lambda_slash_no_default> ::= 
   <lambda_param_no_default> {<lambda_param_no_default>} '/' [','] 

(* 有默认参数 *)
<lambda_param_with_default> ::= <lambda_param> <default> [','] 

<lambda_slash_with_default> ::=
     {<lambda_param_no_default>}
      <lambda_param_with_default> {<lambda_param_with_default>} '/' [','] 

<lambda_star_etc> ::=
      '*'     <lambda_param_no_default>    {<lambda_param_maybe_default>} [<lambda_kwds>] 
    | '*' ',' <lambda_param_maybe_default> {<lambda_param_maybe_default>} [<lambda_kwds>] 
    | <lambda_kwds> 

```

```ebnf

(***************************************)

(* 下标可以切片表示 *)
<t_primary> ::=
      <t_primary> '.' <NAME> 
    | <t_primary> '[' <slices> ']' 
    | <t_primary> <genexp>
    | <t_primary> '(' [<arguments>] ')'
    | <atom> 

<slices> ::= <slice>  
      (<slice>|<starred_expression>) 
 {',' (<slice>|<starred_expression>)} [','] 


<star_target> ::=
      '*' <star_target> 
    | <target_with_star_atom>

<type_param_seq> ::= <type_param> {',' <type_param>} [','] 

<bitwise_or> ::=
      <bitwise_or> '|' <bitwise_xor> 
    | <bitwise_xor>

<dotted_as_names> ::= <dotted_as_name> {',' <dotted_as_name>}

<dotted_name> ::=
      <dotted_name> '.' <NAME> 
    | <NAME>

(* import_from_targets为*:导入的全部 *)
<import_from_targets> ::=
      '(' <import_from_as_names> [','] ')' 
    |     <import_from_as_names>
    | '*' 

<del_target> ::=
      <t_primary> '.' <NAME>
    | <t_primary> '[' <slices> ']'
    | <del_t_atom>

<params> ::= <parameters>

<func_type_comment> ::=
      <NEWLINE> <TYPE_COMMENT>
    | <TYPE_COMMENT>

(* ':=':赋值,返回表达式值 *)
<assignment_expression> ::= <NAME> ':='  <expression> 

<arguments> ::= <args> [',']

(* match的匹配模式 *)
<patterns> ::=
      <open_sequence_pattern> 
    | <pattern>

(* 守卫条件 *)
<guard> ::= 'if' <named_expression> 

<compare_op_bitwise_or_pair> ::=
    | <eq_bitwise_or>
    | <noteq_bitwise_or>
    | <lte_bitwise_or>
    | <lt_bitwise_or>
    | <gte_bitwise_or>
    | <gt_bitwise_or>
    | <notin_bitwise_or>
    | <in_bitwise_or>
    | <isnot_bitwise_or>
    | <is_bitwise_or>

<lambda_param> ::= <NAME> 

<default> ::= '=' <expression>

<lambda_param_maybe_default> ::=
      <lambda_param> [<default>] [','] 

<lambda_kwds> ::= '**' <lambda_param_no_default> 


(***************************************)

<genexp> ::=
   '(' (<assignment_expression>|<expression>) <for_if_clauses> ')' 

<atom> ::=
      <NAME>
    | 'True' 
    | 'False' 
    | 'None' 
    | <strings>
    | <NUMBER>
    | (<tuple> | <group> | <genexp>)
    | (<list> | <listcomp>)
    | (<dict> | <set> | <dictcomp> | <setcomp>)
    | '...' 

(* 下标可以切片表示,起始:结尾:步长 *)
<slice> ::=
      [<expression>] ':' [<expression>] [':' [<expression>] ] 
    | <named_expression> 

<starred_expression> ::= '*' <expression> 

<target_with_star_atom> ::=
      <t_primary> '.' <NAME>
    | <t_primary> '[' <slices> ']'
    | <star_atom>

<type_param> ::=
      <NAME> [<type_param_bound>] [<type_param_default>] 
    | '*'  <NAME> [<type_param_starred_default>] 
    | '**' <NAME> [<type_param_default>] 

<bitwise_xor> ::=
    | <bitwise_xor> '^' <bitwise_and> 
    | <bitwise_and>

<dotted_as_name> ::=
    | <dotted_name> ['as' <NAME> ] 

<import_from_as_names> ::= 
    <import_from_as_name> {',' <import_from_as_name>} 

<del_t_atom>:
    | <NAME> 
    | '('  <del_target>   ')' 
    | '(' [<del_targets>] ')' 
    | '[' [<del_targets>] ']' 

<parameters>:
      <slash_no_default> {<param_no_default>} {<param_with_default>} [<star_etc>] 
    | <param_no_default> {<param_no_default>} {<param_with_default>} [<star_etc>] 
    | <slash_with_default> {<param_with_default>} [star_etc] 
    | <param_with_default> {<param_with_default>} [<star_etc>] 
    | <star_etc> 

<args> ::=
         (<starred_expression> | (<assignment_expression>|<expression>))
    {',' (<starred_expression> | (<assignment_expression>|<expression>))}
    [',' <kwargs> ] 
  | <kwargs> 

<open_sequence_pattern> ::=
   <maybe_star_pattern> ',' [<maybe_sequence_pattern>] 

<pattern> ::= <as_pattern>
            | <or_pattern>

<eq_bitwise_or> ::= '==' <bitwise_or> 

(* in,not in:检测是否是集合元素 *)
(* is,is not:检测是否是同一引用 *)
<noteq_bitwise_or> ::= '!=' <bitwise_or> 
<lte_bitwise_or>   ::= '<=' <bitwise_or> 
<lt_bitwise_or>    ::= '<'  <bitwise_or> 
<gte_bitwise_or>   ::= '>=' <bitwise_or> 
<gt_bitwise_or>    ::= '>'  <bitwise_or> 
<is_bitwise_or>    ::= 'is' <bitwise_or> 
<in_bitwise_or>    ::= 'in' <bitwise_or> 
<isnot_bitwise_or> ::= 'is' 'not' <bitwise_or> 
<notin_bitwise_or> ::= 'not' 'in' <bitwise_or> 

(***************************************)
<for_if_clauses> ::= <for_if_clause> {<for_if_clause>} 

<strings> ::= (<fstring>|<string>) 
             {(<fstring>|<string>)}
            | <tstring> {<tstring>} 

<NUMBER> ::= ? 数字 ?

(* 元组 *)
<tuple> ::=
   '(' [<star_named_expression> ',' [<star_named_expressions>]] ')' 

(* 组 *)
<group> ::= '(' (<yield_expr> | <named_expression>) ')' 

(* 列表 *)
<list> ::= '[' [<star_named_expressions>] ']' 

(* 字典 *)
<dict> ::= '{' [<double_starred_kvpairs>] '}' 

(* 集合 *)
<set> ::= '{' <star_named_expressions> '}' 

(* 筛选 *)
<listcomp> ::= '[' <named_expression> <for_if_clauses> ']' 
<dictcomp> ::= '{' <kvpair> <for_if_clauses> '}' 
<setcomp> ::= '{' <named_expression> <for_if_clauses> '}' 

<star_atom> ::=
          <NAME> 
    | '(' <target_with_star_atom> ')' 
    | '(' [<star_targets_tuple_seq>] ')' 
    | '[' [<star_targets_list_seq>] ']' 

<type_param_bound>          ::= ':' <expression> 
<type_param_default>        ::= '=' <expression> 
<type_param_starred_default>::= '=' <star_expression> 

<bitwise_and> ::=
      <bitwise_and> '&' <shift_expr> 
    | <shift_expr>

<import_from_as_name> ::=
      <NAME> ['as' <NAME> ] 

<slash_no_default> ::=
   <param_no_default> {<param_no_default>} '/' [','] 

<slash_with_default> ::=
  {<param_no_default>} <param_with_default> {<param_with_default>} '/' [','] 


<param_no_default> ::=
   <param> [','] [<TYPE_COMMENT>] 

<param_with_default> ::=
   <param> <default> [','] [<TYPE_COMMENT>] 

<star_etc> ::=
      '*' <param_no_default> {<param_maybe_default>} [<kwds>] 
    | '*' <param_no_default_star_annotation> {<param_maybe_default>} [<kwds>] 
    | '*' ',' <param_maybe_default> {<param_maybe_default>} [kwds] 
    | <kwds> 

<kwargs> ::=
   <kwarg_or_starred> {',' <kwarg_or_starred>} ','
   <kwarg_or_double_starred> {',' <kwarg_or_double_starred>}
 | <kwarg_or_starred> {',' <kwarg_or_starred>} ','
 | <kwarg_or_double_starred> {',' <kwarg_or_double_starred>}

<maybe_star_pattern> ::=
   <star_pattern>
 | <pattern>

<maybe_sequence_pattern> ::=
   <maybe_star_pattern> {',' <maybe_star_pattern>} [',']

<as_pattern> ::=
    <or_pattern> 'as' <pattern_capture_target> 

<or_pattern> ::=
    <closed_pattern> {'|' <closed_pattern>} 

(***************************************)
<for_if_clause> ::=
      'async' 'for' <star_targets> 'in' <disjunction> {'if' <disjunction>} 
    |         'for' <star_targets> 'in' <disjunction> {'if' <disjunction>}

<fstring> ::=
   <FSTRING_START> {<fstring_middle>} <FSTRING_END> 

<string> ::= <STRING> 

<tstring> ::=
   <TSTRING_START> {<tstring_middle>} <TSTRING_END> 

<star_named_expressions> ::= 
   <star_named_expression> {',' <star_named_expression>} [','] 

<kvpair> ::= <expression> ':' <expression> 

<star_targets_tuple_seq> ::=
   <star_target> {',' <star_target>} [','] 

<star_targets_list_seq> ::= 
   <star_target> {',' <star_target>} [','] 

<shift_expr> ::=
      <shift_expr> '<<' <sum> 
    | <shift_expr> '>>' <sum> 
    | <sum>

<param> ::= <NAME> [<annotation>]

<param_maybe_default> ::=
    <param> [<default>] [','] [<TYPE_COMMENT>]

<param_no_default_star_annotation> ::=
    <param_star_annotation> [','] [<TYPE_COMMENT>]

<kwds> ::= '**' <param_no_default> 

<kwarg_or_double_starred> ::=
      <NAME> '=' <expression> 
    | '**' <expression> 

<kwarg_or_starred> ::=
      <NAME> '=' <expression> 
    | <starred_expression> 


<star_pattern> ::=
      '*' <pattern_capture_target> 
    | '*' <wildcard_pattern> 


<pattern_capture_target> ::= <NAME>

<closed_pattern> ::=
      <literal_pattern>
    | <capture_pattern>
    | <wildcard_pattern>
    | <value_pattern>
    | <group_pattern>
    | <sequence_pattern>
    | <mapping_pattern>
    | <class_pattern>

<double_starred_kvpairs> ::=
       <double_starred_kvpair>
  {',' <double_starred_kvpair>} [','] 

(***************************************)
<FSTRING_START> ::= ? 串结尾 ?
<FSTRING_END>   ::= ? 串结尾 ?
<TSTRING_START> ::= ? 串结尾 ?
<TSTRING_END>   ::= ? 串结尾 ?
<STRING> ::= ? 串 ?

<fstring_middle> ::=
      <fstring_replacement_field>
    | <FSTRING_MIDDLE> 

<tstring_middle> ::=
      <tstring_replacement_field>
    | <TSTRING_MIDDLE> 

<star_named_expression> ::=
      '*' <bitwise_or> 
    | <named_expression>

<sum> ::=
      <sum> '+' <term> 
    | <sum> '-' <term> 
    | <term>

<annotation> ::= ':' <expression> 

<param_star_annotation> ::= <NAME> <star_annotation> 

<literal_pattern> ::=
      <signed_number>  
    | <complex_number> 
    | <strings> 
    | 'None' 
    | 'True' 
    | 'False' 

<capture_pattern> ::= <pattern_capture_target> 

<wildcard_pattern> ::= "_" 

<value_pattern> ::= <attr> 

<group_pattern>::= '(' <pattern> ')' 

<sequence_pattern> ::=
      '[' [<maybe_sequence_pattern>] ']' 
    | '(' [<open_sequence_pattern>]  ')' 

<mapping_pattern> ::=
      '{' '}' 
    | '{' <double_star_pattern> [','] '}' 
    | '{' <items_pattern>  ',' <double_star_pattern> [','] '}' 
    | '{' <items_pattern> [','] '}' 

<class_pattern> ::=
      <name_or_attr> '(' ')' 
    | <name_or_attr> '(' <positional_patterns> [','] ')' 
    | <name_or_attr> '(' <keyword_patterns>    [','] ')' 
    | <name_or_attr> '(' <positional_patterns>  ',' <keyword_patterns> [','] ')' 

<double_starred_kvpair> ::=
      '**' <bitwise_or> | <kvpair>

(***************************************)
<fstring_replacement_field> ::=
    '{' <annotated_rhs> ['='] [<fstring_conversion>] [<fstring_full_format_spec>] '}'

<FSTRING_MIDDLE> ::= "串"

<term> ::=
      <term> '*' <factor> 
    | <term> '/' <factor> 
    | <term> '//' <factor> 
    | <term> '%' <factor> 
    | <term> '@' <factor> 
    | <factor>

<star_annotation> ::= ':' <star_expression> 

<signed_number> ::= ['-'] <NUMBER> 

<complex_number> ::=
      <signed_real_number> '+' <imaginary_number> 
    | <signed_real_number> '-' <imaginary_number>  


<attr> ::= <name_or_attr> '.' <NAME> 

<double_star_pattern> ::= '**' <pattern_capture_target> 

<items_pattern> ::= 
   <key_value_pattern> {',' <key_value_pattern>}

<name_or_attr> ::= <attr> | <NAME>

<positional_patterns> ::= <pattern> {',' <pattern>}

<keyword_patterns> ::=
    <keyword_pattern> {',' <keyword_pattern>}

(***************************************)
<fstring_conversion> ::= "!" <NAME> 

<fstring_full_format_spec> ::= ':' {<fstring_format_spec>} 

<factor> ::=
      '+' <factor> 
    | '-' <factor> 
    | '~' <factor> 
    | <power>

<signed_real_number> ::= ['-'] <real_number> 

<imaginary_number> ::= <NUMBER> 

<key_value_pattern> ::= (<literal_expr> | <attr>) ':' <pattern> 

<keyword_pattern> ::= <NAME> '=' <pattern> 

(***************************************)
<fstring_format_spec> ::=
      <FSTRING_MIDDLE> 
    | <fstring_replacement_field>

<power> ::=
      <await_primary> '**' <factor> 
    | <await_primary>


<real_number> ::= <NUMBER> 

<literal_expr> ::=
      <signed_number>
    | <complex_number>
    | <strings>
    | 'None' 
    | 'True' 
    | 'False' 

(***************************************)
<await_primary> ::=
      'await' <primary> 
    | <primary>

(***************************************)

<primary> ::=
      <primary> '.' <NAME> 
    | <primary> <genexp> 
    | <primary> '(' [<arguments>] ')' 
    | <primary> '[' <slices> ']' 
    | <atom>

(***************************************)

<tstring_format_spec> ::=
      <TSTRING_MIDDLE> 
    | <tstring_format_spec_replacement_field>

<tstring_full_format_spec> ::=
      ':' {<tstring_format_spec>}

<tstring_format_spec_replacement_field> ::=
      '{' <annotated_rhs> ['=']
         [<fstring_conversion>]
         [<tstring_full_format_spec>] '}' 

<tstring_replacement_field> ::=
   '{' <annotated_rhs> ['=']
      [<fstring_conversion>]
      [<tstring_full_format_spec>] '}' 
```

<font color=#ff0044>
<center>Written by Vito Devlin:tada:</center>
<center>condexpr01@outlook.com</center>
</font>







