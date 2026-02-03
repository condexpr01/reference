# 希腊字母


| 大写 | 小写 | 英文拼写    | 中文读音（近似） | 常见用途/例子             |
| --- | --- | ------- | -------- | ------------------- |
| Α  | α  | Alpha   | 阿尔法      | 角度、系数、Alpha通道（图像）   |
| Β  | β  | Beta    | 贝塔       | Beta测试、Beta衰变（物理）   |
| Γ  | γ  | Gamma   | 伽马       | Gamma射线、Gamma函数     |
| Δ  | δ  | Delta   | 德尔塔      | Delta变体（病毒）、变化量     |
| Ε  | ε  | Epsilon | 艾普西隆     | 数学中极小量              |
| Ζ  | ζ  | Zeta    | 泽塔       | Zeta函数（数学）          |
| Η  | η  | Eta     | 伊塔       | 效率（工程中常用η）          |
| Θ  | θ  | Theta   | 西塔       | 角度、Theta函数          |
| Ι  | ι  | Iota    | 约塔       | 极小量（not one iota）   |
| Κ  | κ  | Kappa   | 卡帕       | Kappa曲线、Kappa系数     |
| Λ  | λ  | Lambda  | 拉姆达      | 波长（λ）、Lambda表达式（编程） |
| Μ  | μ  | Mu      | 缪        | 微（百万分之一，μ）、摩擦系数     |
| Ν  | ν  | Nu      | 纽        | 频率（ν，物理学）           |
| Ξ  | ξ  | Xi      | 克西       | Xi函数（数学）            |
| Ο  | ο  | Omicron | 奥密克戎     | 新冠病毒Omicron变异株      |
| Π  | π  | Pi      | 派        | 圆周率π、Pi键（化学）        |
| Ρ  | ρ  | Rho     | 柔        | 密度（ρ）、Rho激酶         |
| Σ  | σ  | Sigma   | 西格玛      | 求和符号Σ、标准差σ          |
| Τ  | τ  | Tau     | 陶        | 时间常数τ、Tau蛋白（生物）     |
| Υ  | υ  | Upsilon | 宇普西隆     | Upsilon介子（物理）       |
| Φ  | φ  | Phi     | 斐        | 黄金比例φ、Phi函数         |
| Χ  | χ  | Chi     | 希        | Chi平方检验（统计）         |
| Ψ  | ψ  | Psi     | 普西       | 波函数ψ（量子力学）          |
| Ω  | ω  | Omega   | 欧米伽      | 欧姆（Ω）、角速度ω          |

# simple english defination in ebnf

```bnf
<sentence> ::= <simple_sentence> 
             | <compound_sentence>
             | <complex_sentence>
(* ------------------------------ *)
<simple_sentence> ::= [<adjunct> [","]] <independent_clause> [<adjunct>]
<compound_sentence> ::= <simple_sentence> <conjunction> <simple_sentence>
<complex_sentence> ::= <dependent_clause> "," <independent_clause> 
                     | <independent_clause> <dependent_clause>
(* ------------------------------ *)

<adjunct> ::= <adverb> | <prepositional_phrase>

<independent_clause> ::= <declarative> 
                       | <interrogative> 
                       | <imperative> 
                       | <exclamative>
<dependent_clause> ::= <adverbial_clause> 
                    | <relative_clause> 
                    | <nominal_clause>

<conjunction> ::= "and" | "but" | "or" 
                  | "so" | "yet" | "for" 
                  | "nor" | "because" | ...

(* ------------------------------ *)

<adverb> ::= "quickly" | "silently" | ...
<prepositional_phrase> ::= <preposition> <noun_phrase>

<declarative> ::= <noun_composition> <verb_phrase> "."
<interrogative> ::= <polar_question> | <wh_question>
<imperative> ::= <verb_phrase> "!"
<exclamative> ::= <what_exclam> 
                | <how_exclam> 
                | <so_such_exclam> 
                | <wh_exclam>

<adverbial_clause> ::= <subordinator> <declarative>
<relative_clause> ::= <relative_pronoun> <noun_phrase> <verb_phrase> 
<nominal_clause> ::= "that" <noun_composition> <verb_phrase> 
                   | <wh_phrase> <noun_composition> <verb_phrase>

(* ------------------------------ *)

<preposition> ::= "in" | "on" | "at" | ...
<noun_phrase> ::= [<determiner>] [<adjective_phrase>] <noun> [<complement>] [<adjunct>] [<post_modifier>]

<noun_composition> ::= <noun_phrase> | <nominal_clause>
<verb_phrase> ::= <past_verb_phrase> | <present_verb_phrase> | <future_verb_phrase>

<polar_question> ::= <aux_verb> <noun_composition> <verb_phrase> "?"
<wh_question> ::= <wh_phrase> <aux_verb> <noun_composition> <verb_phrase> "?"
                | <wh_phrase> <verb_phrase> "?"

<what_exclam> ::= "What" <noun_phrase> [<noun_composition> <verb_phrase>] "!"
<how_exclam> ::= "How" <adjective_phrase> [<noun_composition> <verb_phrase>] "!"
<so_such_exclam> ::= "So" <adjective_phrase> [<noun_composition> <verb_phrase> "!"
<wh_exclam> ::= <wh_phrase> <noun_composition> <verb_phrase> "!"

<subordinator> ::= <time_subordinator> 
                | <cause_subordinator> 
                | <condition_subordinator>

<relative_pronoun> ::= "who" | "which" | ...
<wh_phrase> ::= "What" | "Who" | "Where" | "When" | "Why" | "How" 
              | "How many" | "How much" | "How often" | "How long" | ...
(* ------------------------------ *)
<determiner> ::= "the" | "a" | "an" 
                 | "this" | "that" | "these" | "those" 
                 | "my" | "your" | "his" | "her" | ...
<adjective_phrase> ::= <adjective> | <adjective_phrase> <adjective>
<noun> ::= "cat" | "dog" | "man" | "woman" | ...

<complement> ::= <noun_phrase> | <adjective_phrase>

<post_modifier> ::= <prepositional_phrase> | <relative_clause> | <non_predicate_phrase>

<past_verb_phrase> ::= <past_verb> [<noun_composition>]
                       [<non_predicate_phrase>] [<prepositional_phrase>]
<present_verb_phrase> ::= <present_verb> [<noun_composition>]
                          [<non_predicate_phrase>] [<prepositional_phrase>]
<future_verb_phrase> ::= <future_verb> <present_verb_phrase>

<aux_verb> ::= "do" | "does" | "did" 
             | "has" | "have" | "had" 
             | ...
<time_subordinator> ::= "when" | "while" | "before" | "after" | "since" | "until" | ...
<cause_subordinator> ::= "because" | "since" | "as" | "so that" | "in order that" | ...
<condition_subordinator> ::= "if" | "unless" | "provided that" | "in case" | ...

(* ------------------------------ *)
<non_predicate_phrase> ::= (<infinitive> | <gerund> | <participle> ) <noun_composition>

<adjective> ::= "big" | "small" | "beautiful" | ...
<past_verb> ::= "saw" | "ate" | "went" | ...
<present_verb> ::= "see" | "eat" | ...
<future_verb> ::= "will" | "be going to" | ...

(* ------------------------------ *)
<infinitive> ::= "to do" | "to go" | ...
<gerund> ::= "doing" | "going" | ...
<participle> ::= "done" | "gone" | ...

```


## 语感
> <font color=#ff0044>$ <action> ::= <sentence> $</font>    
> <font color=#ff0044>$ sentence可以终止，action收敛为终止符 $</font>    
> <font color=#ff0044>$ 关注动作，自然可以展开出发出者，动作描述，接受者 $</font>    

## 元音
<font color=#ffa500>

||开(长)|闭(短)|
|---|---|---|
||元辅e、辅元|辅元辅、元辅|
|a|/eɪ/|/æ/|
|e|/iː/|/ɛ/|
|i|/aɪ/|/ɪ/|
|o|/oʊ/|/ɒ/|
|u|/juː/|/ʌ/|

</font>

## 注:变形表说明
> <font color=#ff0044>$ 词的变化 \Rightarrow 变化类型 $</font>    
> <font color=#ff0044>$ 变化类型 \nRightarrow 具体变化 $</font>    

## 复数

<font color=#ffa500>

|||
|---|---|
|元+y|`+s`|    
|辅+y|`变y为i,+es `|    
|||
|无生命-o结尾|`+s`|    
|有生命-o结尾|`+es`|    
|||
|-s,-x,-ch,-sh结尾|`+es`|    
|||
|一般|`+s`|    
|-f,-fe|`变f,fe为v,+es`|    
|不规则|`单复同形` ,`变内元音`|    

</font>


## 比较级最高级

<font color=#ffa500>

|单元音及少数双元音词|||
|---|---|---|
|一般|`+er`|`+est`|
|辅结尾且闭读重音节|`双写辅+er`|`双写辅+est`|
|元+y|`+er`|`+est`|    
|辅+y|`变y为i,+er`|`变y为i,+est`|    
|-e结尾|`+r`|`+st`|
|含不规则|---|---|

</font>

<font color=#ffa500>

||||
|---|---|---|
|多元音词|`more +`|`most +`|

</font>

## 形容词副词:

<font color=#ffa500>

|形容词||
|---|---|
|一般|`+ly`|
|元+y|`+ly`|
|元+e|`去e+ly`|
|辅+y|`变y为i+ly`|
|-ble|`变e为y`|
|-ic|`+ally`|

</font>


## 动词
> <font color=#ffa500>动词确定方法</font>:<font color=#39c5bb>`主被、单复、时态`</font>    
> <font color=#ffa500>$ 主语 $</font>$\Rightarrow $<font color=#39c5bb>$主动被动单数复数 $</font>    
> <font color=#ffa500>$ 情景 $</font>$\Rightarrow$<font color=#39c5bb>$ 时态 $</font>    



* 现在

<font color=#ffa500>

|原形||
|---|---|
|一般|`+ing`|
|辅结尾且闭读重音节|`双写辅+ing`|
|-e结尾|`去e+ing`|
|-ie结尾|`变ie为y+ing`|

</font>

* 过去式

<font color=#ffa500>

|原形||
|---|---|
|一般|`+ed`|
|辅结尾且闭读重音节|`双写辅+ed`|
|元+y|`+ed`|    
|辅+y|`变y为i,+ed`|    
|-e结尾|`+d`|
|含不规则|---|---|

</font>


* 过去分词
> <font color=#39c5bb>一般同过去式</font>    
> <font color=#39c5bb>含不规则</font>    


## 所有格
> <font color=#39c5bb>`'s`所有格</font>    
> <font color=#39c5bb>`of` 所有格</font>    
> <font color=#39c5bb>`of + ... +'s`双重所有格</font>    

## 独立主格
> <font color=#39c5bb>非谓语带逻辑主语</font>

## 冠词
> <font color=#ff0044>`the`</font>:<font color=#39c5bb>定</font>

> <font color=#ff0044>`a/an`</font>:<font color=#39c5bb>(可数)不定</font>    
> <font color=#ff0044>`零冠词`</font>:<font color=#39c5bb>(不可数及复数)不定</font>    

## 形式it
* 作成分的形式
* 强调句
> <font color=#39c5bb>It is/was ... who/that ...</font>    
> <font color=#39c5bb>特点:去除装饰后成分完整</font>    

## 双宾、宾补
> <font color=#39c5bb>v. n1 n2</font>    
> <font color=#39c5bb>n1 be n2能表状态则宾补,否则双宾</font>    

## 双宾、同位语
> <font color=#39c5bb>n1 n2</font>    
> <font color=#39c5bb>n1与n2相同则同位语,否则双宾</font>    


## to do\doing\did\done
* to do
> <font color=#39c5bb>不定、目的、结果</font>    

* doing
> <font color=#39c5bb>确定、非目的、持续</font>    

* did
> <font color=#39c5bb>过去</font>    

* done
> <font color=#39c5bb>(am,is,are,was,were+)被动、(have,has,had+)完成</font>    

## 虚拟语气

> <font color=#39c5bb>时态推一个过去</font>    

<font color=#ffa500>

||主句|从句|
|---|---|---|
|虚拟过去|wscm + have done| had done |
|虚拟现在|wscm + do| did,(be用were)|
|虚拟将来|wscm + do| did/were to do/should do|

</font>

> <font color=#ff0044>含蓄虚拟从句</font>:<font color=#39c5bb>without,but for,otherwise,or</font>    

> <font color=#ff0044>从句表下列意思用虚拟:</font>
> <font color=#39c5bb>(inist)</font>    
> <font color=#39c5bb>(order,command)</font>    
> <font color=#39c5bb>(require,demand,request)</font>    
> <font color=#39c5bb>(suggest,advise,propose,recommend)</font>    


## 定从
> <font color=#ff0044>关系代词</font>: <font color=#39c5bb>(who,whom,which,that,whose,as)</font>     
> <font color=#ff0044>关系副词</font>: <font color=#39c5bb>(when,where,why)</font>    

* 只用which:
> <font color=#39c5bb>非限定从</font>    
> <font color=#39c5bb>介词连用作关系副词</font>        

* 只用that:
> <font color=#39c5bb>先行词</font>：
> <font color=#39c5bb>是不定代词(all,few,little,much,something...)、</font>    
> <font color=#39c5bb>是序数词最高级,或被序数词最高级修饰、</font>    
> <font color=#39c5bb>被(all,the only,the very,the right...)修饰、</font>    
> <font color=#39c5bb>有人又有物</font>    

## 名从
> <font color=#ff0044>连接词</font>:<font color=#39c5bb>连词代词副词</font>    

* whether\if
> <font color=#39c5bb>同从,表从,or not,介词后,用whether</font>    

> <font color=#39c5bb>if主从只能句末，前面得有it形式</font>    

* that\what
> <font color=#39c5bb>that无意义，what有实际含义。</font>    

## 反义疑问:
> <font color=#39c5bb>前肯后否，前否后肯</font>    

## 省略
* be省略
> <font color=#39c5bb>主和从主语一致，省be</font>    

* to do省略
> <font color=#39c5bb>to do中do前文提及,省do</font>    

> <font color=#39c5bb>使役感观动词后宾补,省to</font>    
> <font color=#39c5bb>前有do，but+to do中,省to</font>    


## 倒装
* 完全倒装
> <font color=#39c5bb>句首副介且名词主语时</font>    

* 部分倒装
> <font color=#ff0044>NOS</font>    
> <font color=#ff0044>N</font>: <font color=#39c5bb>句首:否定词或否定短语</font>    
> <font color=#ff0044>O</font>: <font color=#39c5bb>句首:only+状语</font>    
> <font color=#ff0044>S</font>: <font color=#39c5bb>so/such...that(so+助\be\情+主语,such+a/an+n.+助\be\情+主语)</font>   

## 前置强调

* 感叹句
> <font color=#39c5bb>what强调n.</font>    
> <font color=#39c5bb>how强调adj.,adv.</font>    

* as/though
> <font color=#39c5bb>形名动副+as/though+主+谓</font>    













<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>
