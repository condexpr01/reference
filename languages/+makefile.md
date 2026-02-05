
# makefile

```ebnf
<EOF> ::= ? end of file ?
<NL> ::= "\n" | "\r" | "\r\n"
<LEADING_TAB> ::= ? tabulation at the first position in a line (eats NL) ?
<COMMENT> ::= ? # comment (can be multiline) ?
<ASSIGN_OP> ::= "=" | "?=" | ":=" | "::=" | "+=" | "!="
<CHARS> ::= ? sequence of non-whitespace ?
<WS> ::= ? sequence of whitespace ?
<SLIT> ::= ? single or double-quote literal ?
<VAR> ::= "$@"      (* 当前目标文件名 *)
		| "$<"      (* 第一个依赖文件名 *)
		| "$^"      (* 所有依赖（去重，空格分隔） *)
		| "$?"      (* 比目标新的所有依赖 *)
		| "$*"      (* 模式规则中%匹配的部分（茎） *)
		| "$%"      (* 归档成员目标（如lib.a(foo.o)中的foo.o） *)
		| "$+"      (* 所有依赖（保留顺序，不去重） *)
		| "$&"      (* 所有依赖（GNU Make 4.3+，保留重复） *)
		| "$|"      (* 仅出现在静态模式规则的依赖中 *)
		| "$$"      (* 字面量$（转义） *)
		| "$("      (* 字面量(（转义） *)
		| "$)"      (* 字面量)（转义） *)
		| "${"      (* 字面量{（转义） *)
		| "$}"      (* 字面量}（转义） *)

<colon> ::= ':' | "::"

<makefile> ::= <statements> <EOF> | <EOF>

(***************************************)
<statements> ::= <br>
			  | <statements> <br>
			  | <statement>
			  | <statements> <statement>

(***************************************)
(* 新行或引导tab *)
<br> ::= <NL> | <LEADING_TAB>

<statement> ::= <COMMENT>
			| <assignment> <br>
			| <function> <br>
			| <rule>
			| <conditional>
			| <define>
			| <include>
			| <export> <br>

(***************************************)
<assignment> ::= <pattern> <ASSIGN_OP> <comment_opt>
			| <pattern> <ASSIGN_OP> <exprs_in_assign> <comment_opt>
			| <assignment_prefix> <ASSIGN_OP> <comment_opt>
			| <assignment_prefix> <ASSIGN_OP> <exprs_in_assign> <comment_opt>

<function> ::= <VAR>
        | ("${"|"$(") <function_name> (")"|"}")
        | ("${"|"$(") <function_name> <WS> [<arguments>] (")"|"}")
        | ("${"|"$(") <function_name> ',' [<arguments>] (")"|"}")
        | ("${"|"$(") <function_name> ':' <expressions> (")"|"}")
        | ("${"|"$(") <function_name> <ASSIGN_OP> <expressions> (")"|"}")

<rule> ::= <targets> <colon> <prerequisites> <NL>
    | <targets> <colon> <prerequisites> <recipes> <NL>
    | <targets> <colon> <assignment> <NL>

<conditional> ::= <if_eq_kw> <condition> <statements_opt> "endif" <comment_opt> <br>
			| <if_eq_kw> <condition> <statements_opt> "else" <statements_opt> "endif" <comment_opt> <br>
			| <if_eq_kw> <condition> <statements_opt> "else" <conditional>
			| <if_def_kw> <identifier> <statements_opt> "endif" <comment_opt> <br>
			| <if_def_kw> <identifier> <statements_opt> "else" <statements_opt> "endif" <comment_opt> <br>
			| <if_def_kw> <identifier> <statements_opt> "else" <conditional>

<define> ::= "define" <pattern> <definition> "endef" <br>
			| "define" <pattern> <ASSIGN_OP> <definition> "endef" <br>
			| <specifiers> "define" <pattern> <definition> "endef" <br>
			| <specifiers> "define" <pattern> <ASSIGN_OP> <definition> "endef" <br>

<include> ::= "include" <expressions> <br>

<export> ::= "export"
			| "unexport"
			| <assignment_prefix>
			| <assignment_prefix> <WS> <targets>

(***************************************)
<comment_opt> ::= [<COMMENT>]

<pattern> ::= <pattern_text>
		| <pattern_function>

<exprs_in_assign> ::= <expr_in_assign>
               | <exprs_in_assign> <WS> <expr_in_assign>

<assignment_prefix> ::= <specifiers> <pattern>

<function_name> ::= <function_name_text>
             | <function_name_function>

<recipes> ::= <recipe>
       | <recipes> <recipe>

<if_def_kw> ::= "ifdef" | "ifndef"

<if_eq_kw> ::= "ifeq" | "ifneq"

<condition> ::= '(' <expressions_opt> ',' <expressions_opt> ')' 
			| <SLIT> <SLIT>

<statements_opt> ::= <comment_opt> <br>
				| <comment_opt> <br> <statements>

<definition> ::= <comment_opt> <br>
			| <comment_opt> <br> <exprs_in_def> <br>

<specifiers> ::= "override"
          | "export"
          | "unexport"
          | "override" "export"
          | "export" "override"
          | "undefine"
          | "override" "undefine"
          | "undefine" "override"

<expressions> ::= <expression>
           | <expressions> <WS> <expression>

<targets> ::= <target>
       | <targets> <WS> <target>

<arguments> ::= <argument>
         | <arguments> ','
         | <arguments> ',' <argument>

<prerequisites> ::= [<targets>]

(***************************************)
<pattern_text> ::= <identifier>
            | <pattern_function> <identifier>

<pattern_function> ::= <function>
                | <pattern_text> <function>
                | <pattern_function> <function>

<expr_in_assign> ::= <expr_text_in_assign>
              | <expr_func_in_assign>

<function_name_text> ::= <function_name_piece>
                  | <function_name_function> <function_name_piece>

<function_name_function> ::= <function>
                      | <function_name_text> <function>

<recipe> ::= <LEADING_TAB> <exprs_in_recipe>
      | <NL> <conditional_in_recipe>
      | <NL> <COMMENT>

<expressions_opt> ::= [<expressions>]

<exprs_in_def> ::= <first_expr_in_def>
            | <br>
            | <br> <first_expr_in_def>
            | <exprs_in_def> <br>
            | <exprs_in_def> <WS> <expr_in_recipe>
            | <exprs_in_def> <br> <first_expr_in_def>

<expression> ::= <expression_text>
          | <expression_function>

<target> ::= <pattern>

<argument> ::= <expressions>

(***************************************)
<identifier> ::= <CHARS>
          | ','
          | '('
          | ')'
          | <identifier> <CHARS>
          | <identifier> <keywords>
          | <identifier> ','
          | <identifier> '('
          | <identifier> ')'

<expr_text_in_assign> ::= <text_in_assign>
                   | <expr_func_in_assign> <text_in_assign>

<expr_func_in_assign> ::= <function>
                   | <expr_func_in_assign> <function>
                   | <expr_text_in_assign> <function>

<function_name_piece> ::= {<CHARS>}

<conditional_in_recipe> ::= <if_eq_kw> <condition> <recipes_opt> "endif" <comment_opt>
                     | <if_eq_kw> <condition> <recipes_opt> "else" <recipes_opt> "endif" <comment_opt>
                     | <if_eq_kw> <condition> <recipes_opt> "else" <conditional_in_recipe>
                     | <if_def_kw> <identifier> <recipes_opt> "endif" <comment_opt>
                     | <if_def_kw> <identifier> <recipes_opt> "else" <recipes_opt> "endif" <comment_opt>
                     | <if_def_kw> <identifier> <recipes_opt> "else" <conditional_in_recipe>

<first_expr_in_def> ::= <char_in_def> <expr_in_recipe>
                 | <function> <expr_in_recipe>
                 | <char_in_def>
                 | <function>

<expr_in_recipe> ::= <expr_text_in_recipe>
              | <expr_func_in_recipe>

<expression_text> ::= <text>
               | <expression_function> <text>

<expression_function> ::= <function>
                   | '(' <exprs_nested> ')'
                   | <expression_text> <function>
                   | <expression_function> <function>

<exprs_in_recipe> ::= <expr_in_recipe>
					| <exprs_in_recipe> <WS> <expr_in_recipe>

(***************************************)
<keywords> ::= "include"
			| "override"
			| "export"
			| "unexport"
			| "ifdef"
			| "ifndef"
			| "ifeq"
			| "ifneq"
			| "else"
			| "endif"
			| "define"
			| "endef"
			| "undefine"

<text_in_assign> ::= [<text_in_assign>] <char_in_assign>

<recipes_opt> ::= <comment_opt> [<recipes>] <NL>

<char_in_def> ::= <char>
           | '('
           | ')'
           | ','
           | <COMMENT>
           | "include"
           | "override"
           | "export"
           | "unexport"
           | "ifdef"
           | "ifndef"
           | "ifeq"
           | "ifneq"
           | "else"
           | "endif"
           | "define"
           | "undefine"

<expr_text_in_recipe> ::= <text_in_recipe>
                   | <expr_func_in_recipe> <text_in_recipe>

<expr_func_in_recipe> ::= <function>
                   | <expr_func_in_recipe> <function>
                   | <expr_text_in_recipe> <function>

<text> ::= [<text>] <char>

<exprs_nested> ::= <exprs_nested> {<WS> <expr_nested>}

(***************************************)

<char_in_assign> ::= <char_nested>
              | '('
              | ')'
              | <keywords>

<char> ::= <CHARS>
    | <SLIT>
    | <ASSIGN_OP>
    | ':'

<expr_nested> ::= <expr_text_nested>
           | <expr_func_nested>

<text_in_recipe> ::= <char_in_recipe>
              | <text_in_recipe> <char_in_recipe>

(***************************************)
<char_nested> ::= <char>
           | ','

<expr_text_nested> ::= <text_nested>
                | <expr_func_nested> <text_nested>

<expr_func_nested> ::= <function>
                | '(' <exprs_nested> ')'
                | <expr_func_nested> <function>
                | <expr_text_nested> <function>

<char_in_recipe> ::= <char_in_assign>
              | <COMMENT>
(***************************************)


<text_nested> ::= <char_nested>
           | <text_nested> <char_nested>

```





