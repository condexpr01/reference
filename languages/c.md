
# <font color=#39c5bb>:sparkles: 字符集 :sparkles:</font>

### <font color=#39c5bb> - :star: ASCII :star: </font>
> ASCII(american standard code for information interchange)    

<font color=#ffa500>$ scope:0 \sim 127(2^7-1) $</font>

### <font color=#39c5bb> - :star: UTF :star: </font>
> UTF(unicode trasformation format)    
> BMP(Basic Multilingual Plane),U+0000~U+FFFF    

<font color=#ffa500>

|codec|u8|u16|u32|
|---|---|---|---|
|byte|1~4|2or4| 4 |
|BOM(byte order mark)|~~[EF BB BF]~~|le[FF FE]<br>be[FE FF]|le[FF FE 00 00]<br>be[00 00 FE FF]|
|recognize|1byte:0xxxxxxxx<br>2byte:110xxxxx 10xxxxxx<br>3byte:1110xxxx 10xxxxxx 10xxxxxx<br>4byte:11110xxx 10xxxxxx 10xxxxxx 10xxxxxx|2byte:BMP代理区外<br>非[0xD800,0xDFFF]<br><br>4byte:高+低(代理)<br>高:[0xD800,0xDBFF]<br>低:[0xDC00,0xDFFF]|4byte固定|

</font>


* <font color=#39c5bb> :rocket: 字符常量及字面量,字面值 :rocket:</font>
<font color=#ffa500>

|   |常量|字面量|
|---|---|---|
|char|`''` `u8''`|`""` `u8""`|
|wchar\_t|`L''`|`L""`|
|char16\_t|`u''`|`u""`|
|char32\_t|`U''`|`U""`|

|后缀suffix|整数后|浮点后|
|---       |---   |---   |
|u/U       |unsigned|    |
|l/L       | long |long double|
|ll/LL  |long long|      |
|f/F       |      | float|
|ul,lu,ull,llu...|||     

</font>

> 字面量编码是实现定义的,从执行字符集到指定的编码的映射

### <font color=#39c5bb> - :star: GB :star: </font>
> GB(国标),K(扩)

<font color=#ffa500>

|codec|gb2312|gbk|gb18030|
|---|---|---|---|
|byte|1byte:[0x00,0x7e]<br><br>2byte:<br>首[0xa1,0xf7]<br>尾[0xa1,0xfe]|1byte:[0x00,0x7e]<br><br>2byte:<br>首[0x81,0xfe]<br>尾[0x40,0xfe]|1byte:[0x00,0x7e]<br><br>2byte:<br>首[0x81,0xfe]<br>尾[0x40,0xfe]<br><br>4byte:<br>首[0x81,0xfe]<br>次[0x30,0x39]<br>三[0x81,0xfe]<br>四[0x30,0x39]|

</font>

# <font color=#39c5bb>:sparkles: cc1翻译阶段 :sparkles:</font>

<font color=#ffa500>

|1.字符映射|1.映射为源字符集<br>2.处理双三标符|
|:-|:-|
|2.行拼接|1.拼接`\`结束行|
|3.预处理记号化|1.源文件最大吞噬分解为注释,空白字符,预处理记号<br>2.一个空格替换每段注释<br>3.保持换行符|
|4.预处理|1.执行预处理器<br>2.include引入文件都递归执行1~4阶段<br>3.结束后移除预处理器指令|
|5.字符集转换|1.字符常量字面量及转义序列从源字符集转换为执行字符集|
|6.字符串连接|1.连接相邻的字符串|
|7.编译|1.按语法和语义分析记号,翻译成汇编|
|8.链接|1.汇编到机械码as,链接库ld|

</font>

> 基本源字符集={5个空白符(space,\t,\v,\f,\n),10个数字,52个大小写字母,29个标点}
> 源字符集:源文件字符编码,包括基本源字符集

> 基本执行字符集={基本源字符集,\n,\r,\a,\b,\0}
> 执行字符集:程序所用字符编码,包括基本执行字符集
> gcc:-finput-charset=,-fexec-charset=,-fwide-exec-charset=

> <iso646.h>
> |基本|双标符|三标符(c23移)|
> |---|---|---|
> |{|<%|??<|
> |}|%>|??>|
> |[|<:|??(|
> |]|:>|??)|
> |#|%:|??=|
> |#|%:%:|??=??=|
> |&#92;||??/|
> |^||??'|
> |\|||??!|
> |~||??-|

> 预处理记号:
>> 头文件名<br>
>> 标识符<br>
>> 预处理数字<br>
>> 字符常量与字符串字面量<br>
>> 运算符与标点<br>
>> 其它非空白字符<br>

> 最大吞噬原则:词法分析器会尽可能多地读取字符以形成最长的有效token    

> 转义序列:
>> 简单转义序列:`\'` `\"` `\?` `\\` `\a` `\b` `\f` `\n` `\r` `\t` `\v`    
>> 数值转义序列:8进制`\nnn` 16进制`\xnn`    
>> 通用字符名:`\unnnn` `\Unnnnnnnn`    


# <font color=#39c5bb>:sparkles: 概念 :sparkles:</font>


## <font color=#39c5bb> -:star: 换算 :star:</font>

<font color=#ffa500>

|1byte|8bit|
|---|---|
|2byte|1word|
|4byte|1dword|
|8byte|1qword|
|1hex|4bit|
|2hex|1byte|

</font>

> n进制m位内容量:$n^m$    
> 最大值=容量-1    
> 字节容量转换:$(2)^8 = (2^4)^2 < (2^3)^3 $    

## <font color=#39c5bb> -:star: 端序 :star:</font>
```c
do{
	uint16_t v=1;

	//&v low addr byte
	if(*(uint8_t*)&v == (uint8_t)1){
		printf("小端:低字节放低地址\n");
		break;
	}

	printf("大端:高字节放低地址\n");
}while(0);
```

## <font color=#39c5bb> -:star: 对象 :star:</font>
> <font color=#ffa500>c中，对象是执行环境中数据存储的一个区域拥有：</font>    

<font color=#ffa500>

|存储期|
|---|
|大小|
|对齐要求|
|生存期|
|存储期|
|有效类型|
|值|
|标识符(opt)|

</font>

> 对象表示：对象字节序列;对象表示相同，比较相等，逆命题不成立    
> 陷阱表示：无映射对应值的对象表示    

## <font color=#39c5bb> -:star: 对齐 :star:</font>
> <font color=#ffa500>每个完整对象类型拥有一个对齐要求属性(size\_t类型的整数,可以\_Alignof查看);</font>    
> <font color=#ffa500>表示可以分配的相继地址之间的字节数;</font>    

## <font color=#39c5bb> -:star: 有效类型 :star:</font>
> <font color=#ffa500>声明创建时则声明类型即是有效类型</font>    

> <font color=#ffa500>分配创建的则没有声明类型，有效类型为rw时所用的类型:</font>    
>> <font color=#ffa500>首次通过异于char类型的左值写入时的类型(在该次写入和后继读取)</font>    
>> <font color=#ffa500>memcpy或memmove复制时的源对象的类型(在该次写入和后继读取)</font>    
>> <font color=#ffa500>任何其他对无声明类型对象的访问中所用的类型</font>    


## <font color=#39c5bb> -:star: 作用域 :star:</font>

<font color=#ffa500>

|作用域|说明|
|---|---|
|块作用域|块内声明的作用域为整个块<br>不能重复声明<br>参数列表属于定义块内|
|文件作用域|全局作用域|
|函数作用域|函数内标签所有位置都在作用域内|
|函数原型作用域|函数声明中参数名只在参数列表内部可见|

</font>

> 声明点:
>> 结构体,联合体,枚举标签的作用域在类型指定符中的标签出现后,立即开始;
>> 枚举常量的作用域,在其定义枚举项的出现后立即开始;
>> 任何其他标识符的作用域,dcl完整识别后identifier才可用;

## <font color=#39c5bb> -:star: 生存期 :star:</font>
> <font color=#ffa500>每个对象存在,拥有常地址,保有其最近一次存储值(对于VLA还有大小)的程序执行部分为该对象的生存期</font>    
>> 声明中有auto,register,static,extern,\_Thread\_local生存期为其存储期    
>> 分配存储期的对象始于函数返回，终于realloc或解分配函数的调用    
>> caution:生存期外访问是未定义的    

> 右值表达式中(数组成员的结构体,联合体)拥有临时生存周期,只能读,不能修改,生命周期止于(包含它的完整表达式或声明器)结束(c11之前为下一个序列点);

## <font color=#39c5bb> -:star: 命名空间 :star:</font>
|命名空间||
|---|---|
|goto标号命名空间|label标签名|
|struct,union,enum标签名|共享同一命名空间|
|struct,union的成员的标识符|每个结构和联合引入自己的这种命名空间|
|全局属性命名空间|由标准定义的属性前缀或实现定义的属性前缀|
|非标准属性名|后随属性前缀的属性名<br>每个属性前缀拥有分离的它所引入的<br>实现定义属性所在命名空间|
|一般标识符|function函数名,对象名,typedef名,enum枚举常量|
|宏名不属于任何命名空间|

## <font color=#39c5bb> -:star: As-if rule :star:</font>
> <font color=#ffa500>As-if rule:允许任何不改变程序可观测行为的代码转换</font>，编译器需要保证：
>> volatile的可观测副作用    
>> 文件输出顺序与程序一致不重排    
>> 交互式提示先出现再输入    
>> 浮点可观测时不破坏语义    

## <font color=#39c5bb> -:star: 抽象机内存模型 :star:</font>
> 每个字节拥有唯一的地址    
> 内存位置:`标量类型的对象`或`非零长位域的最大连续序列`    

## <font color=#39c5bb> -:star: 求值顺序 :star:</font>    
* <font color=#39c5bb> :rocket: 求值 :rocket:</font>    
> 对于每个表达式或子表达式有两种求值为编译器进行    

<font color=#ffa500>

|求值||
|---|---|
|值计算|计算表达式所返回的值|
|副作用|读写volatile左值指代的对象、写对象、<br>原子同步、修改文件、修改浮点环境<br>（及调用以上操作的函数）|

</font>

* <font color=#39c5bb> :rocket: 序列点 :rocket: </font>    

<font color=#ffa500>

|存在序列点|
|---|
|函数参数和函数指代器求值后|
|`&&` `\|\|` `,` 左运算数求值后右运算数求值前|
|条件运算`?:`第一运算数求值后，第二或第三运算数求值前|
|完整表达式(分号结束或作为控制条件)的求值后，下个完整表达式求值前|
|完整声明器的结尾|
|在紧接库函数返回前|
|格式化I/O中关联到每个转换指定符的动作后|
|每次通过qsort,bsearch调用比较函数前和紧随其后，<br>qsort比较函数和移动对象之间|

</font>

* <font color=#39c5bb> :rocket: 求值排序 :rocket: </font>    

<font color=#ffa500>

|排序方式||
|---|---|
|sequenced before|序列点前求值先完成|
|sequenced after|序列点后求值先完成|
|unsequenced|无顺序可以并行重排|
|indeterminably-sequnced|可以任意顺序进行但不能重叠|

|排序|
|---|
|运算符的各个操作数的值计算先序于运算符本身的值计算|
|直接赋值`=`与所有复合赋值`e.g.+=`的副效应(写左值)后序于左右参数的值计算|
|`++` `--` 的计算值先序于副效应 |
|不先序也不后序的不同函数调用其求值顺序是未确定的|
|初始化器列表中各表达式的求值顺序是未确定的|
|非确定顺序的函数调用、复合赋值、自增减运算的前后缀形式是单独求值|

</font>

## <font color=#39c5bb> -:star: UB :star:</font>
> 正确的c程序没有UB(undefined behaviour)    

<font color=#ffa500>

| UB 类别 | 说明 / 示例 |
|---|---|
| **有符号整数溢出** | `int x = INT_MAX; x++;`//UB，因为有符号整数算术超出表示范围。 |
| **移位越界** | `1 << 32`(在 32 位 int 上)或负数位移`1 << -1`|
| **越界访问** | 访问数组下标超出范围 |
| **生命周期外访问** | 使用作用域结束或 `free` 掉的对象 |
| **对齐错误** | 更强的对齐要求和地址模对齐不等于0：<br> `char buf[4]; int* pi=(int*)(buf+1);` |
| **解引用空指针** | `int* p = NULL; *p;` |
| **悬空指针访问** | `int* p = malloc(4); free(p); *p;` |
| **无效转换（strict aliasing 违背）** | 用不兼容类型指针访问对象：`float f=1.0; printf("%d", *(int*)&f);` |
| **对 realloc 旧指针解引用** | `p = malloc(10); q = realloc(p,20); *p = 1;`//UB（p 已无效）|
| **使用未初始化标量** | `int x; x++;` |
| **非法位模式** | (uint8\_t*)&b = (uint8_t)value;//UB:bool b不保证是0/1 |
| **双重修改 / 修改+读取无序冲突** | `i = i++;` 或 `a[i] = i++;` |
| **除零** | `int x = 1/0;` |
| **无副作用的无限循环** | `for(;;) {}` （如果没有任何可观察副作用）。 |
| **goto/longjmp 到不活跃作用域** | `goto` 跳过变量初始化，或跳入已离开作用域 |
| **调用未定义函数行为** | 调用未声明函数或声明/定义原型不匹配。 |
| **restrict 违规** | 通过 restrict 限定的两个指针同时指向同一对象并被使用。 |
| **调用约定冲突** | 原型与定义不一致，或 ABI(Binary) 约定不同导致 UB。 |
| **使用未初始化的库对象** | 使用未通过 `fopen` 初始化的 `FILE*`。 |
| **传入无效指针** | `strlen(NULL); memcpy(NULL, buf, 10);` |
| **memcpy 源和目标重叠** | 必须用 `memmove`，否则 UB。 |
| **数据竞争 (data race)** | 多线程并发访问同一对象且至少一个是写操作，没有同步。 |
| **超出实现限制** | 过长的标识符、过深的递归导致栈溢出等。 |
| **未定义宏或预处理** | 触发标准未定义的预处理行为，例如非法的 `#include` 路径格式。 |
| **以const声明函数** | 行为未定义UB |
| **以非volatile类型读写volatile类型** | 行为未定义UB |

</font>

## <font color=#39c5bb> -:star: 标点 :star:</font>

<font color=#ffa500>

|标点|含义|
|---|---|
|`{}`|列表,块|
|`[]`|下标,声明|
|`#`|预处理起始,字符串化|
|`##`|宏拼接|
|`()`|分组,划分|
|`;`|结束|
|`:`|标签,说明|
|`...`|变参|
|`?`|条件运算符一部分|
|`::`|c23属性作用域|
|`.`|成员访问|
|`->`|成员访问|
|`~`|逐位非运算|
|`!`|逻辑非|
|`+`|加|
|`-`|减|
|`*`|乘,解引用,指针|
|`/`|除|
|`%`|余mod|
|`^`|逐位异或|
|`&`|逐位与,取地址|
|`\|`|逐位或|
|`=`|赋值|
|`+=`|复合运算|
|`-=`|复合运算|
|`*=`|复合运算|
|`/=`|复合运算|
|`%=`|复合运算|
|`^=`|复合运算|
|`&=`|复合运算|
|`\|=`|复合运算|
|`==`|等于|
|`!=`|不等于|
|`<`|小于|
|`>`|大于|
|`<=`|小于等于|
|`>=`|大于等于|
|`&&`|逻辑与|
|`\|\|`|逻辑或|
|`<<`|左移|
|`>>`|右移|
|`<<=`|复合赋值运算符|
|`>>=`|复合赋值运算符|
|`++`|自增|
|`--`|自减|
|`,`|运算符,分隔|

</font>

# <font color=#39c5bb>:sparkles: C grammar :sparkles:</font>

## <font color=#39c5bb> -:star: 注释 :star:</font>

```C
//单行
/*多行*/
```


## <font color=#39c5bb> -:star: 标识符 :star:</font>

```ebnf
<identifier> ::= (<letter> | '_' | '\Unnnnnnnn' | '\unnnn') 
          , {<number> | <letter> | '_' | '\Unnnnnnnn' | '\unnnn'}

<number> ::= '0' | ... | '9';
<letter> ::= 'a' | ... | 'z' | 'A' | ... | 'Z';
```
> 保留标识符:    
>> 关键字,    
>> 单下划线开始的外部标识符,    
>> 单下划线后随大写字母开始的标识符,    
>> 双下划线开始的标识符,    
>> 标准库定义的所有外部标识符(有宿主环境中),    
>> 声明为实现或标准库未来使用保留的标识符,    
>> 声明为被潜在保留并且为实现所提供的标识符    

> 左值:任何类型异于void的对象类型,且可隐含指代一个对象的表达式    
> 右值:非对象或无存储位置的值    



## <font color=#39c5bb> -:star: 声明 :star:</font>

```ebnf
<declaration> ::= <specifiers-and-qualifiers> <declarators-and-initializer> ';'
| <attr-spec-seq> <specifiers-and-qualifiers> <declarators-and-initializer> ';'
| <attr-spec-seq> ';'

<static-assert> ::= ? static_assert(const_expr); ?
                  | ? static_assert(const_expr,str); ?

(* -------------------------------------------------------------------- *)

<specifiers-and-qualifiers> ::= {<specifiers>|<qualifiers>}
<declarators-and-initializer> ::= <declarator> '=' [<initializer] 
                             {',' <declarator> '=' [<initializer]}

<attr-spec-seq> ::= ? 属性 ?

(* -------------------------------------------------------------------- *)

(* 声明中数量:(类型说明符+|存储类说明符?|函数说明符*|对齐说明符*|类型限定符*) *)
<specifiers> ::=  <type-specifier> 
                | <storage-class-specifier> 
                | <function-specifier>
                | <alignment-specifier>
<qualifiers> ::= "const" | "volatile" | "restrict" | "_Atomic"

(* 声明符：对象，数组，函数，指针 *)
<declarator> ::= <identifier> [<attr-spec-seq>]
 | '(' <declarator> ')'
 | [<identifier>]:<const_expr> [<attr-spec-seq>] (* 位域需要struct-union context *)
 | <norptr-declarator> '[' ["static"] [<qualifiers>] <expression> ']' <attr-spec-seq>
 | <norptr-declarator> '[' [<qualifiers>] '*' ']' <attr-spec-seq> (* 作为形参 *)
 | <norptr-declarator> '(' <parameters-or-identifiers> ')' <attr-spec-seq>
 | '*' [<attr-spec-seq>] [<qualifiers>] <declarator> (* pointers's not dir-dcl *)

<initializer> ::= <assignment-expression>
                 | '{' <initializer-list> '}'


(* -------------------------------------------------------------------- *)
<const_expr> ::= ? 常量表达式 ?

<type-specifier> ::= ? typedef名 ?
 |  "void" 
 |  "_Bool" 
 |  "char"
 |  "signed",("char"|"short"|"int"|"long"|"long long")
 |  "unsigned",("char"|"short"|"int"|"long"|"long long")
 | ? 扩展实现定义:__int128,__uint128 ?
 | ? stdint.h:intN_t,uintN_t,intptr_t,uintptr_t N(8,16,32,64) ?
 |  ("float"|"double"|"long double"),["_Complex"|"_Imaginary"]
 |  ("_Decimal32"|"_Decimal64"|"_Decimal128")
 | <struct-union-specifier>
 | <enum-specifier>
 | "_Atomic" '(' <type-specifier> ')'

<storage-class-specifier> ::= "typedef" | "auto" | "register" 
                              | "static" | "extern" | "_Thread_local" 
<function-specifier> ::= "inline" | "_Noreturn"
<alignment-specifier> ::= "_Alignas"

(* 直接声明符，旧<direct-declarator>新别名<norptr-declarator> *)
<norptr-declarator> ::= <identifier> [<attr-spec-seq>]
 | '(' <declarator> ')'
 | [<identifier>]:<const_expr> [<attr-spec-seq>] (* 位域需要struct-union context *)
 | <norptr-declarator> '[' ["static"] [<qualifiers>] <expression> ']' <attr-spec-seq>
 | <norptr-declarator> '[' [<qualifiers>] '*' ']' <attr-spec-seq> (* 形参 *)
 | <norptr-declarator> '(' <parameters-or-identifiers> ')' <attr-spec-seq>

<expression> ::= ? 表达式(VLA support) ?
<identifier> ::= ? 标识符 ?
<assignment-expression> ::= ? 赋值表达式 ?
<initializer-list> ::= ? 初始化列表 ?
<parameters-or-identifiers> ::= ? 函数形参列表 ?

(* -------------------------------------------------------------------- *)

<struct-union-specifier> ::= ("struct" | "union") [<attr-spec-seq>] <name>
            | ("struct" | "union") [<attr-spec-seq>] [<name>] '{' <struct-union-list> '}'

<enum-specifier> ::= "enum" [<attr-spec-seq>] [<name>] '{' <enumerator-list> '}'

(* -------------------------------------------------------------------- *)


<name> ::= ? 从命名空间上区别于identifier ?

(* 任意数量的变量声明,位域声明,和静态断言声明 *)
<struct-union-list> ::= {<declarator> | <assert-declaration>}

<enumerator-list> ::= (<identifier> [<attr-spec-seq>] ['=' <const_expr>])
                   {',' <identifier> [<attr-spec-seq>] ['=' <const_expr>] }
```


## <font color=#39c5bb> -:star: 定义 :star:</font>

> <font color=#39c5bb>定义是一个提供所有关于其所声明标识符信息的声明</font>    
>> <font color=#ffa500>每个enum,typedef声明都是定义</font>    
>> <font color=#ffa500>对于函数包含函数体的声明即是定义</font>    
>> <font color=#ffa500>对于对象分期其存储的声明即是定义</font>    
>> <font color=#ffa500>对于struct,union指定其成员列表的声明是定义</font>    

## <font color=#39c5bb> -:star: 属性 :star:</font>

* <font color=#39c5bb> :rocket: 属性语法 :rocket: </font>

```ebnf
<attr-spec-seq> ::= {<attr-spec>}

(* -------------------------------------------------------------------- *)

<attr-spec> ::= "[[" <attr> {',' <attr>} "]]"

(* -------------------------------------------------------------------- *)

<attr> ::= <standard-attribute>
        | <standard-attribute> '(' [<arguments>] ')'
        | <attribute-prefix> "::" <identifier>
        | <attribute-prefix> "::" <identifier> '(' [<arguments>] ')'

(* -------------------------------------------------------------------- *)

<arguments> ::= ? 实参列表 ?
<identifier> ::= ? 标识符 ?
<attribute-prefix> ::= <identifier>

(* 标准属性 *)
<standard-attribute> ::= "deprecated"  (* 弃用的,不推荐使用警告 *)
                       | "fallthrough" (* case直落,不警告 *)
                       | "maybe_unused"(* 可能未用,不警告 *)
                       | "nodiscard"   (* 不应舍弃,舍弃时警告 *)
                       | "noreturn"    (* 永不返回 *)
                       | "unsequenced" (* 无序的 *)
                       | "reproducible"(* 可复现的 *)

```

* <font color=#39c5bb> :rocket: 属性测试:rocket: </font>

> 预处理运算符:\_\_has\_c\_attribute(attribute\_token)返回值(特定于厂商的为非零整数常量):

<font color=#ffa500>

|attribute\_token|\_\_has\_c\_attribute(attribute\_token)|Standard
|---|---|---|
|deprecated|201904L|(C23)|
|fallthrough|201904L|(C23)|
|maybe\_unused|201904L|(C23)|
|nodiscard|202003L|(C23)|
|noreturn|202202L|(C23)|
|unsequenced|202207L|(C23)|
|reproducible|202207L|(C23)|

</font>

* <font color=#39c5bb> :rocket: 标准属性:rocket: </font>

```c
//deprecated属性:弃用的,允许但不推荐使用
//string_literal为弃用原因,编译器对其警告
[[deprecated]]
[[deprecated(string_literal)]]
[[__deprecated__]]
[[__deprecated__(string_literal)]]
```

```c
//fallthrough属性:case直落是有意的,编译器不应该警告
[[fallthrough]];
[[__fallthrough__]];
```

```c
//maybe_unused属性:实体未使用,编译器不应该警告
[[fallthrough]]
[[__fallthrough__]]
```

```c
//nodiscard属性:强制转换到非void的弃值表达式,
//及某struct/union/enum类型的返回值,不应舍弃
//string_literal为原因,编译器对其警告
[[nodiscard]]
[[nodiscard(string_literal)]]
[[__nodiscard__]]
[[__nodiscard__(string_literal)]]

(void)function(args);//显示弃值表达式
```

```c
//noreturn属性:function不通过return返回或到达函数体结尾返回
[[noreturn]]
[[__noreturn__]]
```

```c
// unsequenced属性:纯数学函数(无副作用+幂等+无状态+独立)，
// 将多次相同的调用优化为一次，
// 还能多核并行、乱序重排、SIMD(single instruction multiple data)向量化批量计算
//// 无副作用：不修改外部对象，内用自己的局部对象
//// 幂等：映射是固定的，多次调用结果一样
//// 无状态：函数每次调用完全取决于参数，没有记忆
//// 独立：不读取外部对象
[[unsequenced]]
[[__unsequenced__]]
```

```c
//reproducible属性:函数(无副作用+幂等)
//将多次相同的调用优化为一次,消除重复调用
[[reproducible]]
[[__reproducible__]]
```

* <font color=#39c5bb> :rocket: gnu::属性列表:rocket: </font>

<font color=#ffa500>

| 属性名        | 说明（参数格式） |
|---------------|-----------------|
| constructor   | 程序启动时调用函数 |
| destructor    | 程序退出时调用函数 |
| hot           | 提示编译器是常被调用 |
| cold          | 提示编译器不常被调用 |
| access        | 指示函数对参数的访问模式，<br>参数格式 `(access_type, parameter_index)` |
| alias         | 当前符号是另一个符号的别名，参数为目标符号名称字符串 |
| always\_inline| 强制内联函数，即使编译器认为不合适 |
| const         | 函数返回值仅依赖参数，不依赖全局变量 |
| deprecated    | 标记函数或变量已弃用，可带字符串参数 |
| format        | printf/scanf 风格检查，<br>参数格式 `(format_type, format_string_index, first_arg_index)` |
| format\_arg   | 指示参数是格式化字符串，参数为格式化字符串的位置 |
| sentinel      | 可变参数函数尾部标识符，参数为终止值 |
| nonnull       | 指示函数参数不能为空指针，可指定位置 |
| weak          | 定义弱符号，允许在链接时被其他定义覆盖 |
| used          | 防止编译器优化掉变量/函数 |
| unused        | 防止未使用警告 |
| noreturn      | 函数不会返回 |
| pure          | 函数返回值仅依赖参数，不修改全局状态 |
| malloc        | 函数返回的新分配内存块 |
| nothrow       | 函数不会抛出异常 |
| gnu\_inline         | GNU 风格内联函数 |
| externally\_visible | 符号对外可见 |

</font>

## <font color=#39c5bb> -:star: address :star:</font>


|解引用|取地址|
|---|---|
|\*address|\&address|
|identifier语义：\*(&identifier) ||

> <font color=#ffa500>n级内容::= '\*'(第n个地址);</font>
> <font color=#ffa500>n级地址::= '&'(第n个地址);</font>


## <font color=#39c5bb> -:star: 表达式 :star:</font>

* <font color=#39c5bb> :rocket: 表达式 :rocket: </font>

```ebnf
<expression> ::= ? 表达式是运算符及其操作数的序列它指定一个运算 ?
```

> 大体顺序：右单目、左单目、双目、关系、逻辑、赋值    

<font color=#ffa500>

| 优先级 | 运算符 | 描述 | 结合性 |
|---|---|---|---|
| 1 | `++` `--`<br>`()`<br>`[]`<br>`.`<br>`->`<br>`(type){list}` | 后缀/后置增量和减量<br>函数调用<br>数组下标<br>结构体和联合体成员访问<br>通过指针访问结构体和联合体成员<br>复合字面量 (C99) | 从左到右 |
| 2 | `++` `--`<br>`+` `-`<br>`!` `~`<br>`(type)`<br>`*`<br>`&`<br>`sizeof`<br>`_Alignof` | 前置增量和减量<br>正负号<br>逻辑非和按位非<br>类型转换<br>间接寻址（解引用）<br>取地址<br>大小<br>对齐要求 (C11) | 从右到左 |
| 3 | `*` `/` `%` | 乘法、除法和取模 | 从左到右 |
| 4 | `+` `-` | 加法和减法 | 从左到右 |
| 5 | `<<` `>>` | 按位左移和右移 | 从左到右 |
| 6 | `<` `<=`<br>`>` `>=` | 分别用于关系运算符 < 和 ≤<br>分别用于关系运算符 > 和 ≥ | 从左到右 |
| 7 | `==` `!=` | 分别用于关系运算符 = 和 ≠ | 从左到右 |
| 8 | `&` | 按位与 | 从左到右 |
| 9 | `^` | 按位异或（排他或） | 从左到右 |
| 10 | `\|` | 按位或（包含或） | 从左到右 |
| 11 | `&&` | 逻辑与 | 从左到右 |
| 12 | `\|\|` | 逻辑或 | 从左到右 |
| 13 | `?:` | 三元条件 | 从右到左 |
| 14 | `=`<br>`+=` `-=`<br>`*=` `/=` `%=`<br>`<<=` `>>=`<br>`&=` `^=` `\|=` | 简单赋值<br>和差赋值<br>积、商和余数赋值<br>按位左移和右移赋值<br>按位与、异或和或赋值 | 从右到左 |
| 15 | `,` | 逗号 | 从左到右 |

</font>

## <font color=#39c5bb> -:star: 转换 :star:</font>

> 隐式转换依赖于上下文，显示转换可以由：    

```ebnf
<cast> ::= '(' <type> ')' <expression> 
```


## <font color=#39c5bb> -:star: 主函数 :star:</font>

> 宿主环境下由main起始    
> 非宿主环境下取决于具体实现    

```c
//argc:argument count
//argv:argument vector
//hInst:hinstance(程序的实例句柄)
//hPrev:always NULL(区分是否已运行，历史遗留)
//lpCmdLine:command line(不含程序名)
//nShowCmd:窗口显示方式,ShowWindow的宏

int main(void){<body>}
int main(int argc,char *argv[]){body}
int WinMain(HINSTANCE hInst,HINSTANCE hPrev,LPSTR lpCmdLine,int nShowCmd){body}

//A(ANSI,char),W(Wide,unicode/UTF-16le)
//指定wmain/wWinmain入口
//-m unicode
int wmain(int argc,wchar_t *argv[]){body}
int wWinMain(HINSTANCE hInst,HINSTANCE hPrev,LPWSTR lpCmdLine,int nShowCmd){body}

/*以及其他实现定义签名*/

```


## <font color=#39c5bb> -:star: 预处理指令 :star:</font>
> 预处理指令以#开头    
> 宏名不是任何命名空间的一部分，在预处理器阶段替换    

<font color=#ffa500>

|#if|#elif|#else|#endif|#ifdef|#ifndef|
|---|---|---|---|---|---|
|#elifdef|#elifndef|#define|#undef|#include|#embed|
|#line|#error|#warning|#pragma|#defined|
|\_\_has\_include|\_\_has\_embed|\_\_has\_c\_attribute|\_\_func\_\_|

</font>

```ebnf
(* 条件控制 *)
<preprocessor-condition> ::= (<sharp-if>|<sharp-ifdef>|<sharp-ifndef>)
                            {<sharp-elif>|<sharp-elifdef>|<sharp-elifndef>}
                            [<sharp-else>]
                            <sharp-endif>

(* 文件包含,__has_include运算符检测 *)
<sharp-include> ::= ('#' "include" '<' <file> '>') (* 搜索实现控制下的文件 *)
		| ('#' "include" '"' <file> '"') (* 搜索不在实现控制下的文件 *)
		| ('#' "include" <pp-tokens>) (* 宏替换后包含 *)


(* 二进制资源包含,__has_embed运算符检测 *)
<sharp-embed> ::= ('#' "embed" '<' <file> '>') <embed-param> (* 搜索实现控制下的文件 *)
		| ('#' "embed" '"' <file> '"') <embed-param> (* 搜索不在实现控制下的文件 *)
		| ('#' "embed" <pp-tokens>) (* 宏替换后内嵌 *)

(* 仿对象宏，仿函数定义 *)
(* 变参替换列表中__VA_ARGS__展开为对应所有的实参 *)
(* 变参替换列表中__VA_OPT__(content)在__VA_ARGS__非空时替换为content，否则为空 *)
(* #运算符对标识符字符串化(以引号包围并转义其中的引号) *)
(* ##运算符连接左右标识符参数替换并连接结果 *)

(* 一元运算符defined(identifier)有定义时为1否则为0 *)
<sharp-define> ::= '#' "define" <identifier> [<replacement-list>]
 |  '#' "define" <identifier> '(' <parameters> [',' "..."] ')' <replacement-list>
 |  '#' "define" <identifier> '(' "..." ')' <replacement-list>

(* 取消宏定义 *)
<sharp-undef> ::= '#' "undef" <identifier>

(* 更改当前行号和文件名(__LINE__,__FILE__) *)
<sharp-line> ::= '#' "line" <number>
		   | '#' "line" <number> <file>

(* 显示msg并停止编译 *)
<sharp-error> ::= '#' "error" <msg>

(* 显示msg但继续编译 *)
<sharp-warning> ::= '#' "warning" <msg>

(* ------------------------------------------ *)
<sharp-if> ::= '#' "if" <const_expr>
<sharp-ifdef> ::= '#' "ifdef" <identifier>
<sharp-ifndef> ::= '#' "ifndef" <identifier>
<sharp-elif> ::= '#' "elif" <const_expr>
<sharp-elifdef> ::= '#' "elifdef" <identifier>
<sharp-elifndef> ::= '#' "elifndef" <identifier>
<sharp-else> ::= '#' "else"
<sharp-endif> ::= '#' "endif"

<embed-param> ::=  ? limit,suffix,prefix,if_emptyt等以空白分隔 ?
<pp-tokens> ::= ? preprocessing tokens:一个或多个预处理标记序列 ?

<replacement-list> ::= ? 替换列表 ?

```

* <font color=#39c5bb> :rocket:预定义宏名:rocket:</font>

<font color=#ffa500>

| 预定义宏名 | C 版本 | 类型 | 展开值/说明 |
|------|-------|------|------------|
| `__STDC__` | C89 | 整数常量 | 始终为 `1`，表示符合标准实现 |
| `__STDC_VERSION__` | C95 | long 整数常量 | 199409L (C95)、199901L (C99)、<br>201112L (C11)、201710L (C17)、<br>202311L (C23) |
| `__STDC_HOSTED__` | C99 | 整数常量 | 有宿主环境为 `1`，独立环境为 `0` |
| `__FILE__` | C89 | 字符串字面量 | 当前文件名，可被 `#line` 修改 |
| `__LINE__` | C89 | 整数常量 | 当前行号，可被 `#line` 修改 |
| `__DATE__` | C89 | 字符串字面量 | 编译日期 `"Mmm dd yyyy"` |
| `__TIME__` | C89 | 字符串字面量 | 编译时间 `"hh:mm:ss"` |
| `__STDC_ISO_10646__` | C99 | long 整数常量 | 若 `wchar_t` 使用 Unicode，<br>则形如 `yyyymmL` <br>表示所支持的 Unicode 版本 |
| `__STDC_IEC_559__` | C99 (C23 弃用) | 整数常量 | 支持 IEC 60559 浮点则为 `1` |
| `__STDC_IEC_559_COMPLEX__` | C99 (C23 弃用) | 整数常量 | 支持 IEC 60559 复数算术则为 `1` |
| `__STDC_UTF_16__` | C11 | 整数常量 | 若 `char16_t` 使用 UTF-16 则为 `1` |
| `__STDC_UTF_32__` | C11 | 整数常量 | 若 `char32_t` 使用 UTF-32 则为 `1` |
| `__STDC_MB_MIGHT_NEQ_WC__` | C99 | 整数常量 | 若 `'x' == L'x'` 可能为 `false` 则为 `1` |
| `__STDC_ANALYZABLE__` | C11 | 整数常量 | 若支持可分析性则为 `1` |
| `__STDC_LIB_EXT1__` | C11 | long 整数常量 | 支持边界检查接口则为 `201112L` |
| `__STDC_NO_ATOMICS__` | C11 | 整数常量 | 不支持原子类型/库则为 `1` |
| `__STDC_NO_COMPLEX__` | C11 | 整数常量 | 不支持复数类型/库则为 `1` |
| `__STDC_NO_THREADS__` | C11 | 整数常量 | 不支持多线程则为 `1` |
| `__STDC_NO_VLA__` | C11 | 整数常量 | 不支持自动存储期可变长度数组则为 `1` |
| `__STDC_IEC_60559_BFP__` | C23 | long 整数常量 | 支持 IEC 60559 二进制浮点则为 `202311L` |
| `__STDC_IEC_60559_DFP__` | C23 | long 整数常量 | 支持 IEC 60559 十进制浮点则为 `202311L` |
| `__STDC_IEC_60559_COMPLEX__` | C23 | long 整数常量 | 支持 IEC 60559 复数算术则为 `202311L` |

</font>

> __func__为当前函数名字符串，为预定义标识符

* <font color=#39c5bb> :rocket: pragma :rocket:</font>

<font color=#ffa500>

| 类别 | 指令 | 作用/说明 | 备注 |
|------|---------|----------|------|
| **C 标准 (可移植)** | `#pragma STDC FENV_ACCESS on/off/default` | 控制是否访问/修改浮点环境 | C99+
|      | `#pragma STDC FP_CONTRACT on/off/default` | 允许或禁止浮点表达式结合（如 FMA） | C99+
|      | `#pragma STDC CX_LIMITED_RANGE on/off/default` | 控制复数运算是否可假设有限范围 | C99+
| **通用便利** | `#pragma once` | 防止头文件被重复包含 | GCC/Clang/MSVC 常用扩展
| **结构体对齐** | `#pragma pack(push, n)`<br>`#pragma pack(pop)` | 改变结构体成员字节对齐方式 | MSVC/GCC/Clang 支持
| **警告控制** | `#pragma warning(disable:4996)` | 关闭或调整特定警告 | 主要用于 MSVC
| **优化控制** | `#pragma GCC optimize("O3")` | 指定编译优化等级 | GCC/Clang 扩展
| **分段代码属性** | `#pragma section("name", read, write)` | 指定代码或数据放入特定段 | MSVC
| **其他示例** | `#pragma message("text")` | 在编译时输出信息 | GCC/Clang/MSVC
| **宏中使用** | `_Pragma("once")` | 运算符形式，可放入宏中触发 pragma | C99+

</font>

## <font color=#39c5bb> -:star: keywords :star:</font>

### <font color=#39c5bb> :star: 对齐 :star:</font>

|alignas|设置对齐要求|
|---|---|
|alignof|查询对齐要求结果是size\_t类型整数常量|

> 对齐说明符alignas(\_Alignas),    
> 设置非位域且不具有register,typedef存储类的声明对象的对齐方式    
> 当出现多个时使用最严格的那个    

```c
//常量表达式设置对齐
_Alignas(const_expr)
alignas(const_expr)

//使用类型对齐要求
_Alignas(type)
alignas(type)

```

> 对齐运算符alignof(\_Alignof)    
> 查询其操作数类型的对齐要求    

```c
_Alignof(type)
alignof(type)
```

### <font color=#39c5bb> :star: 类型限定符 :star:</font>

<font color=#ffa500>

|限定符|效果|
|---|---|
|const|编译器语义const限定对象地址中内容不可修改|
|restrict|指针地址内容与访问的对象地址绑定，在指针第一次被赋值开始的生命周期内，<br>只会由这一个restrict指针或通过它可衍生的指针来访问|
|volatile|对volatile读写认作可观测副效应，所以写入会在下一个序列点之前完成(不能被优化掉，被重排)|

</font>

### <font color=#39c5bb> :star: 存储类说明符 :star:</font>
> 翻译单元：一个源文件及其所有包含的头文件和宏展开后的内容

<font color=#ffa500>

|存储类说明符|存储期和链接|
|---|---|
|auto|自动存储期且无链接|
|register|自动存储期且无链接(不能取地址)|
|static|静态存储期与内部链接|
|extern|静态存储期与外部链接|
|typedef|提供一种声明标识符为类型别名的方式，对存储和链接无影响|
|\_Thread\_local|线程存储期|
|constexpr|编译期常量<br>在编译期完全确定且运行时不可以修改<br>(需与其它组合指定存储期和链接)|

|存储期||
|---|---|
|自动存储期|于块中分配存储，退出该块时解分配存储|
|静态存储期|整个程序的执行过程|
|线程存储期|线程的整个执行过程|
|分配存储期|动态内存分配函数分配和解分配存储|


|链接||
|---|---|
|无链接|只能从其所在的作用域指代该标识符|
|内部链接|能从当前翻译单元的所有作用域指代该标识符|
|外部链接|能从整个程序的任何其他翻译单元指代该标识符|

</font>

### <font color=#39c5bb> :star: 类型说明符 :star:</font>

<font color=#ffa500>

|type specifiers|c标准|
|---|---|
|void|无类型void\*可以任意类型,f(void)表示不接收参数|
|\_BitInt(N)|固定N bit数的任意位宽整数|
|bool(\_Bool)|1bit|
|true|bool值1|
|false|bool值0|
|char|至少1byte|
|short|至少2byte|
|int|至少2byte|
|long|至少4byte|
|signed|有符号位|
|unsigned|无符号位|
|float|$ (-1)^{sign31} \times 2^{E30\_23-127} \times (1 + \sum _{i=1} ^{23} { b _{23-i} 2 ^{-i})} $ |
|double|$(-1)^{sign63} \times 2^{E62\_52-1023} \times (1 + \sum _{i=1} ^{52} { b _{52-i} 2 ^{-i})} $|
|\_Complex|复数复浮点|
|\_Imaginary|虚数虚浮点|
|\_Decimal32|32bit十进制浮点|
|\_Decimal64|64bit十进制浮点|
|\_Decimal128|128bit十进制浮点|



| 数据模型 | `char` | `short` | `int` | `long` | `long long` | pointer |
|---------|-------|--------|------|-------|-----------|------|
| **ILP32** (int long pointer32) | 8  | 16 | 32 | 32 | 64 | 32 |
| **LP32**  (long pointer32)     | 8  | 16 | 16 | 32 | 64 | 32 |
| **LP64**  (long pointer64)     | 8  | 16 | 32 | 64 | 64 | 64 |
| **LLP64** (long long pointer64)| 8  | 16 | 32 | 32 | 64 | 64 |
| **ILP64** (int long pointer64) | 8  | 64 | 64 | 64 | 64 | 64 |

</font>



### <font color=#39c5bb> :star: break/continue语句 :star:</font>

> break终止封闭的for\while\do-while\switch    

```ebnf
	<break-statement> ::= [<attr-spec-seq>] "break"

(* 跟[[likely]] [[unlikely]] [[gnu:hot]] [[gnu:cold]]以优化 *)
```

> continue跳至for\while\do-while循环体末尾    

```ebnf
	<continue-statement> ::= [<attr-spec-seq>] "continue"

(* 跟[[likely]] [[unlikely]] [[gnu:hot]] [[gnu:cold]]以优化 *)
```

### <font color=#39c5bb> :star: return语句 :star:</font>

<font color=#ffa500>

|return|
|---|

</font>

> 函数返回    

```ebnf
<return> ::= [<attr-spec-seq>] "return" [<expression>] ';'
```

### <font color=#39c5bb> :star: goto语句 :star:</font>

<font color=#ffa500>

|goto|
|---|

</font>

> 跳转到label处代码执行(这是很危险的行为，如跳过声明定义)    

```ebnf
<goto> ::= [<attr-spec-seq>] "goto" <label> ';'

<label> ::=[<attr-spec-seq>] <identifier> ':'
```

### <font color=#39c5bb> :star: 泛型表达式 :star:</font>

<font color=#ffa500>

|\_Generic|
|---|

</font>

> 对cexpr的类型检查后在alist中找对应匹配项    
> 找不到则匹配default项    

```ebnf
<generic-expression> ::= "_Generic" '(' <controlling-expression> , <association-list> ')'

<controlling-expression> ::= ? 用来检查类型的表达式，不会求值 ?
<association-list> ::= {("default" | <type-specifier>) ':' <expression>}
                   {',' ("default" | <type-specifier>) ':' <expression>}
```

### <font color=#39c5bb> :star: 内联汇编 :star:</font>

> AT&T语法的汇编    

```ebnf
<asm> ::= "asm(string_literal);"
```

### <font color=#39c5bb> :star: typeof运算符 :star:</font>
> 确定对象的类型    

```c
<typeof> ::= "typeof" '(' <type-specifier> ')' (* 完整类型 *)
           | "typeof" '(' <expression> ')'     (* 完整类型 *)
           | "typeof_unqual" '(' <type-specifier> ')' (* 移除qualifiers *)
           | "typeof_unqual" '(' <expression> ')'     (* 移除qualifiers *)
```

### <font color=#39c5bb> :star: sizeof运算符 :star:</font>
> 返回size\_t类型的值(对象表示的字节大小)    

```c
<sizeof> ::= "sizeof" '(' <type-specifier> ')'
           | "sizeof" '(' <expression> ')'
```

### <font color=#39c5bb> :star: switch语句 :star:</font>

<font color=#ffa500>

|switch|case|default|
|---|---|---|

</font>


```ebnf
<switch> ::= [<attr-spec-seq>] "switch" ( <expression> ) <switch-statement>

<switch-statement> ::= ? 任何语句，允许有case,default标号,break会跳出语句体,VLA不能声明于此 ?

(* 表达式求值等于case常量表达式则跳转到这个case的label执行 *)
<case-label> ::= [<attr-spec-seq>] "case" <const_expr> : [<statement>] 

(* 表达式求值不等于任何case常量表达式则跳转到这个default的label执行 *)
<default-label> ::= [<attr-spec-seq>] "default" : [<statement>] 

(* 表达式求值不等于任何case常量表达式无default，则不执行switch体语句 *)
```

### <font color=#39c5bb> :star: 循环语句 :star:</font>

<font color=#ffa500>

|do|while|for|
|---|---|---|

</font>

```ebnf
<do-while> ::= [<attr-spec-seq>] "do" <statement> "while" '(' <expression> ')' ';'
<while> ::= [<attr-spec-seq>] "while" '(' <expression> ')' <statement>
<for> ::= [<attr-spec-seq>] "for" '(' [<init>] ';' [<condition>] ';' [<iteration>] ')'
           <statement>

<init> ::= ? 声明(作用域于循环体内，开始于条件表达式前)或表达式(会舍弃结果) ?
<condition> ::= ? 条件表达式(循环体前求值) ?
<iteration> ::= ? 迭代表达式(循环体后求值并舍弃结果) ?

```

### <font color=#39c5bb> :star: if/else语句 :star:</font>

<font color=#ffa500>

|if|else|
|---|---|

</font>

> 条件表达式condition!=0则为真,为0即为假    

```ebnf
<if> ::= [<attr-spec-seq>] "if" '(' <condition> ')' <statement> [ "else" <statement> ]
```

### <font color=#39c5bb> :star: 卫语句 :star:</font>

```c
do{
	if(exp1)break;
	if(exp2)break;
	if(exp3)break;
	//...
	if(expn)break;

	opration();
}while(0);
```

### <font color=#39c5bb> :star: struct/union/enum :star:</font>

<font color=#ffa500>

|struct|union|enum|
|---|---|---|

</font>

> struct/union/enum类型说明符:    
> struct,union会对齐最大的对齐要求    
> 从c23起,enum可以指定类型    

```ebnf
<struct-union-specifier> ::= ("struct" | "union") [<attr-spec-seq>] <name>
            | ("struct" | "union") [<attr-spec-seq>] [<name>] '{' <struct-union-list> '}'

<enum-specifier> ::= "enum" [<attr-spec-seq>] [<name>] 
                  [':' <type-specifier>] '{' <enumerator-list> '}'

(* 任意数量的变量声明,位域声明,和静态断言声明 *)
<struct-union-list> ::= {<declarator> | <assert-declaration>}

<enumerator-list> ::= (<identifier> [<attr-spec-seq>] ['=' <const_expr>])
                   {',' <identifier> [<attr-spec-seq>] ['=' <const_expr>] }
```


### <font color=#39c5bb> :star: 内联说明符inline :star:</font>

<font color=#ffa500>

|inline|
|---|

</font>

> 目的是提示编译器做优化    
> 避免函数调用的开销    
> 但会生成更大的执行文件    
> inline不一定生成符号    
> 必要时要extern inline或static inline    

```assembly

//函数调用：
//传参压栈
...
call API


//函数
API:
push rbp
mov rbp,rsp
//局部变量
sub rsp,value
...
leave
ret

```


### <font color=#39c5bb> :star: nullptr :star:</font>

<font color=#ffa500>

|nullptr|
|---|

</font>

> nullptr\_t类型空指针，不同于NULL (void\*)0具有二义性    
> 它只有空指针的语义    
> 能转换指针类型或bool    


### <font color=#39c5bb> :star: 变参数函数 :star:</font>
> 至少一个具名参数，列表的最后指定`...`    

```
//<stdarg.h>中宏:
//va_list为类型实例指针(char*,解释用intptr_t*)
void va_start(va_list ap, parmN);//初始化ap,起始参数为parmN
T va_arg(va_list ap,T);//T:以类型T解释下个参数(无结尾判断机制)
void va_copy(va_list dest,va_list src);
void va_end(va_list ap);//清理va_list对象
```


### <font color=#39c5bb> :star: 静态断言 :star:</font>

> const\_expr在编译时求值并与零比较，等于零则发生错误显示str    

```ebnf
<static-assert> ::= ? static_assert(const_expr); ?
                  | ? static_assert(const_expr,str); ?
```


### <font color=#39c5bb> :star: 原子 :star:</font>

* <font color=#39c5bb> :rocket:唯一不受数据竞争影响的对象:rocket: </font>    

<font color=#ffa500>

|原子一致性||
|---|---|
|写后写一致性|如果修改原子对象 M 的操作 A 先于修改 M 的操作 B 发生，<br>则 A 在 M 的修改顺序中出现在 B 之前|
|写后读一致性|如果原子对象 M 上的副作用 X 先于 M 的值计算 B 发生，<br>则评估 B 从 X 或从在 M 的修改顺序中出现在 X 之后的副作用 Y 获取其值|
|读后读一致性|如果原子对象 M 的值计算 A 先于 M 的值计算 B 发生，<br>并且 A 从 M 上的副作用 X 获取其值，<br>则 B 计算出的值要么是 X 存储的值，<br>要么是 M 上的副作用 Y 存储的值，<br>其中 Y 在 M 的修改顺序中出现在 X 之后|
|读后写一致性|如果原子对象 M 的值计算 A 先于对 M 的操作 B 发生，<br>则 A 从 M 上的副作用 X 获取其值，<br>其中 X 在 M 的修改顺序中出现在 B 之前|

</font>

> 对原子操作应用原子函数    

```c
//指定一个新的原子类型
_Atomic(<type-specifier>) (* type-specifier *)

//限定type-specifier的原子版本
_Atomic <type-specifier>  (* qualifier *)
```

* <font color=#39c5bb> :rocket: memory\_order:rocket: </font>    

```c
//#include <stdatomic.h>
//函数_explicit版指定order

//原子类型A的基本类型C的value给obj
void atomic_store(volatile A *obj,C value);

//返回原子类型A的基本类型C的值
C atomic_load(volatile A*obj);

//原子类型A的基本类型C的value给obj
//返回obj先前的基本类型的值
C atomic_exchange(volatile A *obj,C value);


//若*expected与*obj相等,原子类型A的基本类型C的value给obj,返回true
//若*expected与*obj不等,原子类型A的基本类型C的value给expected,返回false
_Bool atomic_exchange_weak(volatile A *obj,C* expected,C value);//允许虚假失败
_Bool atomic_exchange_strong(volatile A *obj,C* expected,C value);//严格失败

//*obj (+,-,|,^,&)= arg,返回旧*obj
C atomic_fetch_add(volatile A* obj,M arg);
C atomic_fetch_sub(volatile A* obj,M arg);
C atomic_fetch_or(volatile A* obj,M arg);
C atomic_fetch_xor(volatile A* obj,M arg);
C atomic_fetch_and(volatile A* obj,M arg);


enum memory_order {
    memory_order_relaxed,//可乱序重排,只保证原子性
    memory_order_consume,//后面依赖于此加载的副作用在后
    memory_order_acquire,//此加载后的副作用在后
    memory_order_release,//此加载前的副作用在前
    memory_order_acq_rel,//此加载同时分隔前后副作用
    memory_order_seq_cst //moar和单独全序
};
//moc常提升为moa实现
//单独全序会把相关原子的所有操作以逻辑顺序发生

//原子布尔类型 lock-free
struct atomic_flag;
#define ATOMIC_FLAG_INIT /*初始为false*/

//返回值都为旧值，有_explicit版本指定memory_order
_Bool atomic_flag_test_and_set(volatile atomic_flag *obj);//置位
_Bool atomic_flag_clear(volatile atomic_flag *obj);//清除位

//此宏告诉编译器moc的依赖树不再延伸过它的返回值
//返回表达式y,与原值等价但不继承数据依赖的结果
A kill_dependency(A y);

void atomic_thread_fence(memory_order order);//不执行原子操作但建立order
void atomic_signal_fence(memory_order order);//以order抑制编译器指令重排

#define ATOMIC_BOOL_LOCK_FREE     /* implementation-defined */
#define ATOMIC_CHAR_LOCK_FREE     /* implementation-defined */
#define ATOMIC_CHAR16_T_LOCK_FREE /* implementation-defined */
#define ATOMIC_CHAR32_T_LOCK_FREE /* implementation-defined */
#define ATOMIC_WCHAR_T_LOCK_FREE  /* implementation-defined */
#define ATOMIC_SHORT_LOCK_FREE    /* implementation-defined */
#define ATOMIC_INT_LOCK_FREE      /* implementation-defined */
#define ATOMIC_LONG_LOCK_FREE     /* implementation-defined */
#define ATOMIC_LLONG_LOCK_FREE    /* implementation-defined */
#define ATOMIC_POINTER_LOCK_FREE  /* implementation-defined */
#define ATOMIC_CHAR8_T_LOCK_FREE  /* implementation-defined */

//无需锁lock free则返回true否则为false
_Bool atomic_is_lock_free(const valatile A* obj);
```


# <font color=#39c5bb>:sparkles: 库 :sparkles:</font>

## <font color=#39c5bb> :rocket: stdio.h :rocket: </font>    

```c
#include <stdio.h>//输入/输出
typedef /*文件对象类型C IO*/ FILE
typedef /*file position(fgetpos,fsetpos)*/ fpos_t

#define stdin  /* FILE* implementation-defined */
#define stdout /* FILE* implementation-defined */
#define stderr /* FILE* implementation-defined */

#define EOF (-1)
#define FOPEN_MAX /*能同时打开的文件数*/
#define FILENAME_MAX /*最长文件名*/

#define BUFSIZ /*最少的setbuf缓冲区大小*/

#define _IOFBF /*file全缓冲setvbuf*/
#define _IOLBF /*line行缓冲setvbuf*/
#define _IONBF /*no无缓冲setvbuf*/

#define SEEK_SET /*fseek开头位置*/
#define SEEK_CUR /*fseek当前位置*/
#define SEEK_END /*fseek结尾位置*/

#define TMP_MAX /*tmpnam所能生成最大独有文件数*/
#define TMP_MAX_S /*tmpnam_s所能生成最大独有文件数*/

#define L_tmpnam /*保有tmpnam结果所需char数组大小*/
#define L_tmpnam_s /*保有tmpnam_s结果所需char数组大小*/

//fprintf format格式点位符:
//%[flags][width][.precision][length]conversion
//flags:(
//      '-':指定左对齐(默认右对齐)
//      '+':总是输出符号,
//      ' ':正数前留一空格占用+号,
//      '#':替用形式保证8,16进制前缀,浮点小数点出现,
//      '0':宽度不足以0填充
//      )
//width:(
//     10进制整数:指定最小字段宽度,
//     '*':由对应int参数提供
//      )
//precision:(
//     精确度:
//      整数最少打印数不足由0补足,
//      浮点保留小数位数,
//      字符串最大输出字符数
//     '*':由对应int参数提供
//          )
//length:(
//     hh:half of half int
//     h: half int
//     l: long/wchar_t
//     ll:long long
//     j: intmax_t
//     z: size_t
//     t: ptrdiff_t
//     L: long double
//       )
//conversion:(
//     %:字面%
//     d/i:十进制整数
//     u:无符号十进制
//     o:进制
//     X/x:大小写16进制
//     f/F:float
//     e/E:科学计数法
//     g/G:自动选f或e
//     a/A:16进制浮点
//     c:字符char
//     s:串string
//     p:指针值pointer
//     n:当前写入的字符数number
//)
//返回输到流的字符数否则为负
int fprintf(FILE *stream,const char *restrict format,...);
//sprintf,printf,vprintf,etc.
//<wchar.h>:fwprintf,swprintf,wprintf,vwprintf,etc.

//fscanf format格式点位符:
//%[*][width][length]conversion
//*:(读取但不存储,用来跳过),
//width:(10进制整数:指定最大读取宽度),
//length:(
//     hh:half of half int
//     h: half int
//     l: long/wchar_t
//     ll:long long
//     j: intmax_t
//     z: size_t
//     t: ptrdiff_t
//     L: long double
//       )
//conversion:(
//     %:字面%
//     []:匹配集合,'-'表示范围,
//     [^]:匹配反集,'-'表示范围
//     d/i:十进制整数
//     u:无符号十进制
//     o:进制
//     X/x:大小写16进制
//     f/F:float
//     e/E:科学计数法
//     g/G:自动选f或e
//     a/A:16进制浮点
//     c:字符char
//     s:匹配非空白字符序列string
//     p:指针值pointer
//     n:当前写入的字符数number
//)
//返回成功赋值的接收参数数量
int fscanf(FILE *stream,const char *format,...);
//sscanf,scanf,vscanf,etc.
//<wchar.h>:fwscanf,swscanf,wscanf,vwscanf,etc.


// r    : 从头只读（文件必须存在）
// w    : 覆盖写入（文件存在则清空，不存在则创建）
// a    : 追加写入（输出总在末尾，不存在则创建）
// r+   : 从头读写（文件必须存在，初始内容不变）
// w+   : 读写并覆盖（存在则清空，不存在则创建）
// a+   : 读写追加（输出总在末尾，读从头开始，不存在则创建）
// b    : 二进制模式，严格按字节处理（Windows 有效，POSIX 被忽略）
// x    : 独占创建文件，如果已存在则失败（C11 / O_EXCL）
// e    : 文件描述符在 exec() 时自动关闭（FD_CLOEXEC，glibc 扩展）
// c    : 禁用线程取消（glibc 扩展）
// m    : 使用 mmap 访问文件，只读（glibc 扩展）
// ,ccs=ENC : 指定宽字符流编码，如 UTF-8、UTF-16（glibc 扩展）
// +    : 可同时读写，切换读写需先 fflush() 或 fseek()
FILE *fopen(const char *filename,const char *mode);
FILE *freopen(const char *restrict filename,const char *restrict mode,
              FILE *restrict stream);//关闭stream再以mode打开filename

//冲入任何未写入的缓冲数据到OS
//舍弃任何未读取的缓冲数据
//关闭给定流
//返回0或EOF
int *fclose(const char *stream);

//从stream的缓冲区写入未写数据到输出设备
//返回0或EOF
int fflush(FILE *stream);

//设置流内部缓冲区(至少BUFSIZ)
//buffer生存期外访问是未定义的
void setbuf(FILE *restrict stream,char *restrict buffer);


//set vector buffer
//stream要设置缓冲的文件流
//buffer指向要使用的流缓冲区的指针，或若仅更改大小和模式则为空指针
//buffer生存期外访问是未定义的
//mode使用的缓冲模式：
//    _IOFBF全缓冲：当缓冲区为空时，从流读入数据。或者当缓冲区满时，向流写入数据。
//    _IOLBF行缓冲：每次从流中读入一行数据或向流中写入一行数据。
//    _IONBF无缓冲：直接从流中读入数据或直接向流中写入数据，缓冲设置无效。
//size为使用的缓冲区的大小
//成功时返回为0，失败时为非零。
int setvbuf(FILE *restrict stream,char *restrict buffer,
            int mode,size_t size);

//从stream中读size*count个byte到buffer
//返回成功读取多少个size对象数
//少于count则eof或error
//调用feof,ferror以判断
size_t fread(void *buffer,
            size_t size,
            size_t count,
            FILE *stream);


//检查是否抵达文件流结尾
//eof则返回非0,否则为0
int feof(FILE *stream);

//检查是否抵达文件流错误
//error则返回非0,否则为0
int ferror(FILE *stream);

//到达结尾或错误时会置位
//重置error和EOF标志,以复用流
void clearerr(FILE *stream);

//从buffer写入size*count个byte到stream
//返回成功写入多少个size对象数
//少于count则error
size_t fwrite(void *buffer,
            size_t size,
            size_t count,
            FILE *stream);

//操作当前fpos后,fpos+1
int fgetc(FILE *stream);//失败返回EOF(语义EOF或error)
int getc(FILE *stream);//失败返回EOF(语义EOF或error)
int fputc(int ch,FILE *stream);//失败返回EOF(语义error)
int putc(int ch,FILE *stream);//失败返回EOF(语义error)

//fpos-1后,推ch到fpos对应流缓冲区,返回ch或EOF(不置位流)
int ungetc(int ch,FILE *stream);

//从stream中读取最多count-1个字符到str
//返回str或空指针
char *fgets(char *restrict str,
        int count,//通常str长度
        FILE *restrict stream);

//将空终止符结尾的str写入到stream
//返回非负或EOF(置位error)
int fputs(char *restrict str,
        FILE *restrict stream);

int getchar(void);//stdin stream
int putchar(void);//stdout stream

//设置stream的文件位置指示器
//为offset(从origin开始的)
//origin(SEEK_SET,SEEK_CUR,SEEK_END)
//成功返回0,否则非0
int fseek(FILE *stream,
        long offset,
        int origin);
void rewind(FILE *stream);//fseek(stream,0,SEEK_SET);


//返回stream的文件位置指示器
//以b打开的stream返回为与文件首字节的偏移
long ftell(FILE *stream);

//存储stream的文件位置指示器到pos指向的地址内
//成功返回0,否则非0
int fgetpos(FILE *restrict stream,fpos_t *restrict pos);

//以pos设置stream的文件位置指示器和多字节分析状态mbstate_t
//成功返回0,否则非0
int fsetpos(FILE *stream, const fpos_t *pos);

//删除文件
//成功返回0,否则非0
int remove(const char *fname);

//重命名文件
//成功返回0,否则非0
int rename(const char *old_filename,const char *new_filename);


//临时文件wb+,会自动删除
//返回临时文件或空指针
//windows上要求高权限
FILE *tmpfile(void);


```

<font color=#ffa500>

|print说明符 | hh | h | (无) | l | ll | j | z | t | L |
|------------|----|---|------|---|----|---|---|---|---|
| c          |    |   | int  |wint\_t||   |   |   |   |
| s          |    |   | char* |wchar\_t||  |   |   |   |
| d,i        | schar | short | int | long | long long|intmax\_t | size\_t | ptrdiff\_t |  |
| o          | uchar | ushort | uint | ulong | ull | uintmax\_t | size\_t | ptrdiff\_t |  |
| u          | uchar | ushort | uint | ulong | ull | uintmax\_t | size\_t | ptrdiff\_t |  |
| x/X        | uchar | ushort | uint | ulong | ull | uintmax\_t | size\_t | ptrdiff\_t |  |
| f/F        |    |   | double|double||   |   |   | long double  |
| e/E        |    |   | double|double||   |   |   | long double  |
| g/G        |    |   | double |double ||   |   | | long double  |
| a/A        |    |   | double|double||   |   |   | long double  |
| p          |    |   | void* |      ||   |   |   |   |
| n          | schar* | short* | int* | long* | ll* | uintmax\_t* | size\_t* | ptrdiff\_t* | |
| %          |    |   |       |      ||   |   |   |   |


|scan说明符  | hh | h | (无) | l | ll | j | z | t | L |
|------------|----|---|------|---|----|---|---|---|---|
| c          |    |   | char* |wchar\_t* ||  |   |   |   |
| s          |    |   | char* |wchar\_t* ||  |   |   |   |
| []         |    |   | char* |wchar\_t* ||  |   |   |   |
| d,i        | char* | short* | int* | long* | ll* | intmax\_t* | size\_t* | ptrdiff\_t* | |
| o          | char* | short* | int* | long* | ll* | intmax\_t* | size\_t* | ptrdiff\_t* | |
| u          | char* | short* | int* | long* | ll* | intmax\_t* | size\_t* | ptrdiff\_t* | |
| x/X        | char* | short* | int* | long* | ll* | intmax\_t* | size\_t* | ptrdiff\_t* | |
| f/F        |    |   | float* |double* ||   |   |   | long double*  |
| e/E        |    |   | float* |double* ||   |   |   | long double*  |
| g/G        |    |   | float* |double* ||   |   |   | long double*  |
| a/A        |    |   | float* |double* ||   |   |   | long double*  |
| p          |    |   | void* |      ||   |   |   |   |
| n          | char* | short* | int* | long* | ll* | intmax\_t* | size\_t* | ptrdiff\_t* | |
| %          |    |   |       |      ||   |   |   |   |

</font>

## <font color=#39c5bb> :rocket: assert.h :rocket: </font>

```c
//#include <assert.h>//条件编译宏，将参数与零比较

//运行时断言,通过定义NDEBUG在发布版中取消
#ifdef NDEBUG
	#define assert(condition) ((void)0)
#else
	#define assert(condition) /*implementation defined*/
#endif

```

## <font color=#39c5bb> :rocket: complex.h :rocket: </font>

```c
//#include <complex.h>//(C99)	复数运算

#define I /*复数或虚数单位常量sqrt(-1)*/

```

| 函数$(z = x + i y)$ | 公式 |
|---:|:---|
| `CMPLX(x,y)`| $复数构建:x + i\,y$ |
| `creal(z)`| $实部:\Re(z)=x$ |
| `cimag(z)`| $虚部:\Im(z)=y$ |
| `cabs(z)` | $|z|=\sqrt{x^{2}+y^{2}}$ |
| `carg(z)` | $辐角:\arg z=\operatorname{atan2}(\Im z,\Re z)=\operatorname{atan2}(y,x)$ |
| `conj(z)` | $共轭:\overline{z}=x - i\,y$ |
| `cproj(z)`| $黎曼球投影:\displaystyle \operatorname{cproj}(z)=\begin{cases} z, & |z|<\infty,\\[4pt] \infty, & |z|=\infty~(\text{投影到黎曼球上的无穷远点}).\end{cases}$ |

## <font color=#39c5bb> :rocket: ctype.h :rocket: </font>    

```c
#include <ctype.h>//用来确定包含于字符数据中的类型的函数

int isalnum(int ch);//[0,9]['a','z']['A','Z']
int isalpha(int ch);//['a','z']['A','Z']
int islower(int ch);//['a','z']
int isupper(int ch);//['A','Z']
int isdigit(int ch);//[0,9]
int isxdigit(int ch);//[0x0,0xf]
int iscntrl(int ch);//[0x00,0x1f]及0x7f
int isgraph(int ch);//[0x21,0x7e]或其它拥有图像表示的字符
int isspace(int ch);//{0x20' ',0x0a'\n',0x0d'\r',0x09'\t',0x0b'\v'}
int isblank(int ch);//{0x20' ',0x09'\t'}
int isprint(int ch);//[0x20,0x7e]或其它能被打印的字符
int ispunct(int ch);//[!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~]

int tolower(int ch);
int toupper(int ch);
```

## <font color=#39c5bb> :rocket: wctype.h :rocket: </font>    

```c
//#include <wctype.h>//(C95)	用来确定包含于宽字符数据中的类型的函数

int iswalnum(wint_t wc);//[0,9]['a','z']['A','Z']
int iswalpha(wint_t wc);//['a','z']['A','Z']
int iswlower(wint_t wc);//['a','z']
int iswupper(wint_t wc);//['A','Z']
int iswdigit(wint_t wc);//[0,9]
int iswxdigit(wint_t wc);//[0x0,0xf]
int iswcntrl(wint_t wc);//[0x00,0x1f]及0x7f
int iswgraph(wint_t wc);//[0x21,0x7e]或其它拥有图像表示的字符
int iswspace(wint_t wc);//{0x20' ',0x0a'\n',0x0d'\r',0x09'\t',0x0b'\v'}
int iswblank(wint_t wc);//{0x20' ',0x09'\t'}
int iswprint(wint_t wc);//[0x20,0x7e]或其它能被打印的字符
int iswpunct(wint_t wc);//[!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~]

//wctype运行时构造检测
//str可以为:
//"alnum"
//"alpha"
//"blank"
//"cntrl"
//"digit"
//"xdigit"
//"graph"
//"print"
//"space"
//"lower"
//"upper"
//再使用iswctype时就会对应检测与isw系同效
wctype_t wctype(const char* str);
int iswctype(wint_t wc,wctype_t desc);

int towlower(int ch);
int towupper(int ch);

//wctrans运行时构造转换
//str可以为:
//"lower"
//"upper"
//再使用towctrans时就会对应检测与tow系同效
wctrans_t wctrans(const char* str);
int towctrans(wint_t wc,wctype_t desc);

```

## <font color=#39c5bb> :rocket: errno.h :rocket: </font>    

```c
//#include <errno.h>//error number报告错误条件的宏

//展开为可修改左值
#definen errno /*implementation defined*/
typedef int errno_t;
typedef size_t rsize_t; //range size_t表示缓冲区大小

char* strerror(int errnum);//string error返回errnum的错误信息
size_t strerrorlen_s(errno_t errnum);//错误信息的长度

//errnum错误信息写到大小为bufsz的buf中,
//若整个消息完整存储于buf则为零,否则为非零
errno_t strerror_s(char *buf,rsize_t bufsz,errno_t errnum);

//打印errno的错误信息到stderr
//s为附加信息
void perror(const char *s);

```


## <font color=#39c5bb> :rocket: float.h :rocket: </font>    

```c
#include <float.h>//浮点类型的极限
/**********************
FLT_RADIX(*用于表示所有三种浮点数类型的底（整数基）*)

DECIMAL_DIG(*将 long double转换成十进制小数，
再转回 long double 而保持同一值，至少需要 DECIMAL_DIG 位小数：
这是long double 序列化/反序列化所需的十进制精度*)

(将 float/double/long double 转换成十进制小数再转换回同一值，
至少需要 FLT_DECIMAL_DIG/DBL_DECIMAL_DIG/LDBL_DECIMAL_DIG 位小数：
此乃浮点值序列化/反序列化所需的十进制精度。
标准定义至少为 6 、 10 、 10 ，
而 IEEE float 为 9 ， IEEE double 为 17 )
FLT_DECIMAL_DIG
DBL_DECIMAL_DIG
LDBL_DECIMAL_DIG

(*分别为 float 、 double 及 long double 的最小正规正值*)
FLT_MIN
DBL_MIN
LDBL_MIN
 
(*分别为 float 、 double 和 long double 的最小正值*)
FLT_TRUE_MIN
DBL_TRUE_MIN
LDBL_TRUE_MIN
  
(*分别为 float 、 double 和 long double 的最大有限正值*)
FLT_MAX
DBL_MAX
LDBL_MAX
 
(*分别为 1.0 与下一个可表示的float 、 double 和 long double 值之差*)
FLT_EPSILON
DBL_EPSILON
LDBL_EPSILON
 
(*保证能从文本转换为 float/double/long double
再转换回文本，而不会发生改变或上溢出的十进制位数*)
FLT_DIG
DBL_DIG
LDBL_DIG
 
(*分别为 float 、 double 和 long double
所能表示而不损失精度的，浮点尾数中的 FLT_RADIX 底位数*)
FLT_MANT_DIG
DBL_MANT_DIG
LDBL_MANT_DIG
 
(*分别为对应 float、double和long double的最小负整数，
使得 FLT_RADIX 的该数减一次幂为正规*)
FLT_MIN_EXP
DBL_MIN_EXP
LDBL_MIN_EXP
 
(*分别为对应 float、double和long double的最小负整数，
使得 10 的该数减一次幂为正规的*)
FLT_MIN_10_EXP
DBL_MIN_10_EXP
LDBL_MIN_10_EXP
 
(*分别为能够使 FLT_RADIX 的该整数减一次幂为可表示的有限的
float、double 与 long double 的最大正整数*)
FLT_MAX_EXP
DBL_MAX_EXP
LDBL_MAX_EXP
 
(*分别为能够使 10 的该整数减一次幂为可表示的有限的
float、double 与 long double 的最大正整数*)
FLT_MAX_10_EXP
DBL_MAX_10_EXP
LDBL_MAX_10_EXP
 
FLT_ROUNDS(*浮点算术的舍入模式，等于 float_round_style*)

 
FLT_EVAL_METHOD(*中间结果所用的扩展精度：
0 表示不使用，
1 表示用 double 替代 float ，
2 表示使用 long double*)
 
(*类型是否支持非正规数：
 -1 为不确定，
 0 为不支持，
 1 为支持*)
FLT_HAS_SUBNORM
DBL_HAS_SUBNORM
LDBL_HAS_SUBNORM
```

## <font color=#39c5bb> :rocket: inttypes.h :rocket: </font>    

```c
#include <inttypes.h>//(C99)整数类型的格式说明符

/*************
(* 与stdint.h里对应，跨平台fprintf/fscanf类格式说明符 *)

(*超集模板,%c,%lc,%s,%ls不在其中*)
<inttypes> ::= ("PRI" | "SCN") 
         ("d" | "i"  | "u" | "o" | "x" | "X")
         ["LEAST" | "MAX" | "PTR"]
         ["8"|"16"|"32"|"64"]
*************/

```


## <font color=#39c5bb> :rocket: limits.h :rocket: </font>    


```c
//#include <limits.h>//整数类型的范围

/********************
BOOL_WIDTH //_Bool 的位宽
CHAR_BIT //字节的位数
CHAR_WIDTH //char 的位宽，同 CHAR_BIT
MB_LEN_MAX //多字节字符的最大字节数
CHAR_MIN //char 的最小值
CHAR_MAX //char 的最大值

//分别为 signed char、short、int、long 及 long long 的位宽
SCHAR_WIDTH
SHRT_WIDTH
INT_WIDTH
LONG_WIDTH
LLONG_WIDTH
 
//分别是 signed char、short、int、long 和 long long 的最小值
SCHAR_MIN
SHRT_MIN
INT_MIN
LONG_MIN
LLONG_MIN
  
//分别是 signed char、short、int、long 和 long long 的最大值
SCHAR_MAX
SHRT_MAX
INT_MAX
LONG_MAX
LLONG_MAX

//分别为 unsigned char、unsigned short、
//unsigned int、unsigned long 及 unsigned long long 的位宽
UCHAR_WIDTH
USHRT_WIDTH
UINT_WIDTH
ULONG_WIDTH
ULLONG_WIDTH
  
//分别是 unsigned char、unsigned short、unsigned int、
//unsigned long 和 unsigned long long 的最大值
UCHAR_MAX
USHRT_MAX
UINT_MAX
ULONG_MAX
ULLONG_MAX
********************/
```

## <font color=#39c5bb> :rocket: locale.h :rocket: </font>    


```c
#include <locale.h>//本地化工具

//category:LC_XXX macro
//    LC_ALL:选择整个 C 本地环境
//    LC_COLLATE:选择 C 本地环境中的对照类别
//    LC_CTYPE:选择 C 本地环境中的字符分类类别
//    LC_MONETARY:选择 C 本地环境中的货币格式化类别
//    LC_NUMERIC:选择 C 本地环境中的数值格式化类别
//    LC_TIME:选择 C 本地环境中的时间格式化类别
//locale:本地环境标识符,由locale -a列出
//返回应用后的本地c环境或失败时的空指针
char *setlocale(int category,const char* locale);

//获取locale环境的数值和货币格式化规则
struct lconv *localeconv(void);

struct lconv {
    char *decimal_point;    // 小数点符号，例如 "."
    char *thousands_sep;    // 千位分隔符，例如 ","
    char *grouping;         // 千位分组规则，例如"\3"表示每3位一组

    char *int_curr_symbol;  // 国际货币符号，例如 "USD"
    char *currency_symbol;  // 本地货币符号，例如 "$"
    char *mon_decimal_point;// 货币小数点符号
    char *mon_thousands_sep;// 货币千位分隔符
    char *mon_grouping;     // 货币千位分组规则

    char *positive_sign;    // 正数符号，例如 "+"
    char *negative_sign;    // 负数符号，例如 "-"

    char int_frac_digits;   // 国际货币小数位数
    char frac_digits;       // 本地货币小数位数

    char p_cs_precedes;     // 正数货币符号位置（1=前，0=后）
    char p_sep_by_space;    // 正数货币符号与数字间是否空格（1=有空格）
    char n_cs_precedes;     // 负数货币符号位置
    char n_sep_by_space;    // 负数货币符号与数字间是否空格
    char p_sign_posn;       // 正数符号位置（0-4，遵循规范）
    char n_sign_posn;       // 负数符号位置（0-4，遵循规范）

#if __MSVCRT_VERSION__ >= 0xA00 || _WIN32_WINNT >= 0x601
    wchar_t* _W_decimal_point;        // 宽字符小数点
    wchar_t* _W_thousands_sep;        // 宽字符千位分隔符
    wchar_t* _W_int_curr_symbol;      // 宽字符国际货币符号
    wchar_t* _W_currency_symbol;      // 宽字符本地货币符号
    wchar_t* _W_mon_decimal_point;    // 宽字符货币小数点
    wchar_t* _W_mon_thousands_sep;    // 宽字符货币千位分隔符
    wchar_t* _W_positive_sign;        // 宽字符正号
    wchar_t* _W_negative_sign;        // 宽字符负号
#endif
};

```


## <font color=#39c5bb> :rocket: setjmp.h :rocket: </font>    

```c
//#include <setjmp.h>//非局部跳转

typedef /*implementation-defined*/ jmp_buf

//将当前执行环境保存到jmp_buf类型对象env
//初次调用该宏返回0
//跳转后返回longjmp的status,status为0会改为1
//setjmp可出现语境(其它语境则UB)：
//	(if\switch\for\while\do-while的完整控制表达式)
//	(比较或相等运算符的一个运算数，另一个为整数常量表达式)
//	(一元非!运算符的运算数)
//	(setjmp(env);转型到void的完整表达式)
#define setjmp(env) /*implementation-defined*/

//载入setjmp设置的env,status为跳到setjmp后setjmp返回的值
[[noreturn]] void longjmp(jmp_buf env,int status);

```

## <font color=#39c5bb> :rocket: signal.h :rocket: </font>    

```c
#include <signal.h>//信号处理

//设置信号sig的错误处理函数,
//成功时返回之前的信号处理
//失败时返回SIG_ERR
void (*signal(int sig,void (*handler)(int))))(int);

/****sig能为定义值或下列值*******
SIGTERM	发送给程序的终止请求
SIGSEGV	非法内存访问（段错误）
SIGINT	外部中断，通常为用户所发动
SIGILL	非法程序映像，例如非法指令
SIGABRT	异常终止条件，例如 abort() 所起始的
SIGFPE	错误的算术运算，如除以零
********************/

/****handler信号处理函数必须是下列之一*******
#define SIG_DFL /*默认信号处理*/
#define SIG_IGN /*忽略信号*/
void (*)(int sig)类型的函数指针
***********/

int raise(int sig);//发送sig给程序,成功返回0,失败非0

//信号原子,即使缺少信号所做的异步中断，
//亦能作为原子裸体访问的整数类型
//无需原子函数
typedef /* implementation-defined */ sig_atomic_t;
```


## <font color=#39c5bb> :rocket: stddef.h :rocket: </font>    

```c
//#include <stddef.h>常用宏定义

#define /*无符号,能存储类型对象最大大小,implementation-defined*/ size_t
#define NULL /*有二义性implementation-define*/
#define offsetof(struct_union_type,member) /*成员的偏移implementation-defined*/
#define typeof(nullptr) nullptr_t

typedef /*两指针相减结果有符号整数类型,implementation-defined*/ ptrdiff_t;
typedef /*最大对齐类型(long double)implementation-defined*/ max_align_t;

#define unreachable() /*仿函数宏开展成void表达式,执行是UB的,用于优化掉不可能的分支*/

```

## <font color=#39c5bb> :rocket: stdint.h :rocket: </font>    

```c
//#include <stdint.h>//(C99)定宽整数类型

/* intN_t (N:[8,64]) */
/* uintN_t (N:[8,64]) */

/* int_fastN_t (N:[8,64]) */
/* uint_fastN_t (N:[8,64]) */

/* int_leastN_t (N:[8,64]) */
/* uint_leastN_t (N:[8,64]) */

/* intmax_t 至少long long最大宽度整数类型*/
/* uintmax_t */

/* intptr_t 指针大小类型*/
/* uintptr_t */

//宏：
/* INTn_[WIDTH/MIN/MAX] (n:[8,64]) */
/* UINTn_[WIDTH/MIN/MAX] (n:[8,64]) */

/* INT_FASTn_[WIDTH/MIN/MAX] (n:[8,64]) */
/* UINT_FASTn_[WIDTH/MIN/MAX] (n:[8,64]) */

/* INT_LEASTn_[WIDTH/MIN/MAX] (n:[8,64]) */
/* UINT_LEASTn_[WIDTH/MIN/MAX] (n:[8,64]) */

/* INTPTR_[WIDTH/MIN/MAX] */
/* UINTPTR_[WIDTH/MIN/MAX] */

/* INTMAX_[WIDTH/MIN/MAX] */
/* UINTMAX_[WIDTH/MIN/MAX] */

/* INTn_C (n:[8,64]) */
/* UINTn_C (n:[8,64]) */
/* INTMAX_C (n:[8,64]) */
/* UINTMAX_C (n:[8,64]) */

/******************
PTRDIFF_WIDTH(ptrdiff_t 类型对象的位宽)
PTRDIFF_MIN(ptrdiff_t 的最小值)
PTRDIFF_MAX(ptrdiff_t 的最大值)
SIZE_WIDTH(size_t 类型对象的位宽)
SIZE_MAX(size_t 的最大值)
SIG_ATOMIC_WIDTH(sig_atomic_t 类型对象的位宽)
SIG_ATOMIC_MIN(sig_atomic_t 的最小值)
SIG_ATOMIC_MAX(sig_atomic_t 的最大值)
WINT_WIDTH(wint_t 类型对象的位宽)
WINT_MIN(wint_t 的最小值)
WINT_MAX(wint_t 的最大值)
WCHAR_WIDTH(wchar_t 类型对象的位宽)
WCHAR_MIN(wchar_t 的最小值)
WCHAR_MAX(wchar_t 的最大值)
******************/

```


## <font color=#39c5bb> :rocket: stdlib.h :rocket: </font>    

```c
//#include <stdlib.h>//基础工具：内存管理、程序工具、字符串转换、随机数、算法
//_s suffix为边界检查版本

[[noreturn]] void abort(void);//导致程序异常终止
[[noreturn]] void exit(int exit_code);//引发正常终止并清理
[[noreturn]] void quick_exit(int exit_code);//引发正常终止并不完全清理
[[noreturn]] void _Exit(int exit_code);//引发正常终止并不清理

#define EXIT_SUCCESS /*正常退出码implementation defined*/
#define EXIT_FAILURE /*失败退出码implementation defined*/

int atexit(void (*func)(void));//注册func指向函数，在exit()或main()返回时调用
int at_quick_exit(void (*func)(void));//注册func指向函数，在quick_exit()返回时调用

int system(const char *cmd);//调用宿主环境执行cmd，通常返回调用程序返回值

char *getenv(const char *name);//查找name对应的环境变量,非线程安全
errno_t *getenv_s(size_t *restrict len,     //存储环境变量长度
                  char *restrict value,     //缓冲区
                  rsize_t valuesz,          //缓冲区大小
                  const char *restrict name //环境变量名
                  );

void *malloc(size_t size);//memory allocate分配size字节未初始化内存
void *calloc(size_t num,size_t size);//contiguous allocate分配num个size字节的初始化内存

//分配size字节未初始化内存,按alignment对齐,size%alignment==0
void *aligned_alloc(size_t alignment,size_t size);

//reallocate试图扩张收缩ptr区域为new_size字节
//或者分配new_size大小区域并复制旧的内存区域(大小新旧大小中较小的)
//不应该使用旧的ptr(UB)
//返回新的分配内存的指针,失败时为空指针
void *realloc(void *ptr,size_t new_size);

void *free(void *ptr);//解分配之前由malloc,calloc,aligned_alloc,realloc分配的内存

//str指向要转数值的空终止串
//*str_end会指向str中最后被转译字符的后一字符
//base指定进制,0为自动检测,或指定为[2,36]进制
long strtol(const char *restrict str,char **restrict str_end,int base);
unsigned long strtoul(const char *restrict str,char **restrict str_end,int base);
long long strtoll(const char *restrict str,char **restrict str_end,int base);
unsigned long long strtoull(const char *restrict str,char **restrict str_end,int base);
intmax_t strtouimax(const char *restrict str,char **restrict str_end,int base);
uintmax_t strtouimax(const char *restrict str,char **restrict str_end,int base);

//str为10,16进制浮点,INF,NAN,任何其它可由c地环境接受的表达式
float strtof(const char *restrict str,char **restrict str_end);
double strtod(const char *restrict str,char **restrict str_end);
long double strtold(const char *restrict str,char **restrict str_end);

```


## <font color=#39c5bb> :rocket: string.h :rocket: </font>    

```c
#include <string.h>//字符串处理
//_s suffix为边界检查版本

char *strcpy(char *restrict dest,const char *restrict src);//返回dest地址
char *strncpy(char *restrict dest,const char *restrict src,size_t count);
errno_t strcpy_s(char *restrict dest,rsize_t destsz,
           const char *restrict src);
errno_t strncpy_s(char *restrict dest,rsize_t destsz,
            const char *restrict src,rsize_t count);

char *strcat( char *dest, const char *src );//src附加到dest,返回dest地址
char *strcat( char *restrict dest, const char *restrict src );
errno_t strcat_s(char *restrict dest, rsize_t destsz,
           const char *restrict src);

char *strdup(const char *src);//为src dump同malloc必须要free
char *strndup(const char *src,size_t size);//dump size个

size_t strlen(const char *str);//str length
size_t strnlen_s(const char *str,size_t strsz);//strsz为最大能访问长度

int strcmp(const char *lhs,const char *rhs);//lhs-rhs
int strncmp(const char *lhs,const char *rhs,size_t count);

int strcoll(const char *lhs,const char *rhs);//按LC_COLLATE排序比较

//string transform转换为LC_COLLATE排序规则，
//保证strcmp的结果和strcoll一致
size_t strxfrm( char *restrict dest, const char *restrict src,
                size_t count );

//string character正序查找ch
//string reverse character逆序查找ch
//返回ch出现位置的指针或空指针
char *strchr(const char *str,int ch);
char *strrchr(const char *str,int ch);

//string span返回str中属于集合set中字符的最大连续长度
//string complement span，匹配cset补集
size_t strspn(const char *str,const char *set);
size_t strcspn(const char *str,const char *cset);

//string pointer to break
//返回str中首次出现set集合字符的指针
//否则为空指针
char *strpbrk(const char *str,const char *set);

//str中找substr,返回substr出现位置指针或空指针
char *strstr(const char *str,const char *substr);

//以delimeter集合分隔str为token(修改str)
//首次调用后str会在内部静态指针中
//后续str传入空指针调用返回下一个token
//strtok_s中不再用内部静态指针而是ptr
char *strtok(char *restrict str,const char *restrict delim);
char *strtok_s(char *restrict str,const char *restrict strmax,
         const char *restrict delim,char **restrict ptr);

//在ptr的count个中找ch,返回其位置或空指针
char *memchr(const void *ptr,int ch,size_t count);

//lhs-rhs
char *memcmp(const void *lhs,const void *rhs,size_t count);

//以ch值覆盖dest的前count个,返回dest地址
//destsz指定dest大小
void *memset(void *dest,int ch,size_t count);
errno_t memset_s(void *dest,rsize_t destsz,
                     int ch,rsize_t count);

//src复制count个到dest,返回dest地址
//destsz指定dest大小
//memcpy不允许区域重叠，但最快的
//memccpy可以设置c为终止字节
void* memcpy( void *restrict dest, const void *restrict src, size_t count );
errno_t memcpy_s( void *restrict dest, rsize_t destsz,
            const void *restrict src, rsize_t count );
void memccpy(void *restrict dest,const void *restrict src,int c,size_t count);

//src移动count个到dest,返回dest地址
//destsz指定dest大小
//memmove允许区域重叠
void* memmove( void* dest, const void* src, size_t count );
errno_t memmove_s(void *dest, rsize_t destsz,
            const void *src, rsize_t count);

```


## <font color=#39c5bb> :rocket: time.h :rocket: </font>    

```c
//#include <time.h>//时间/日期工具
//365平年,366闰年(x%400==0) || (x%100!=0 && x%4==0)
//Jan31 Feb28/29 Mar31 Apr30 May31 Jun30
//Jul31 Aug31    Sep30 Oct31 Nov30 Dec31

typedef /*1970,1,1,00:00 s*/ time_t;
#define CLOCKS_PER_SEC /*implementation defined*/
typedef /*ticks num*/ clock_t;//除CLOCKS_PER_SEC得秒数

struct tm {
  int tm_sec;//s 0-59
  int tm_min;//m 0-59
  int tm_hour;//h 0-23

  int tm_mday;//1-31
  int tm_mon;//0-11
  int tm_year;//year-1900

  int tm_wday;//0-6

  int tm_yday;//0-365

  //0 !dst
  //>0 daylight saving time
  //spring 23h a day
  //summer 24h a day
  //autumn 25h a day
  //winter 24h a day
  int tm_isdst;//夏令时
};

/*time_t -> struct tm*/
struct tm *gmtime(const time_t *timer);//UTC time
struct tm *localtime(const time_t *timer);

/* struct tm -> time_t */
time_t mktime(struct tm *time);

```

## <font color=#39c5bb> :rocket: wchar.h :rocket: </font>    

```c
//#include <wchar.h>//(C95)	扩展多字节和宽字符工具
/*wchar_t 任何合法宽字符的整数类型*/
/*wint_t 任何合法宽字符并能表示WEOF*/
/*wctrans_t 本地环境的desc*/
/*wctype_t 本地环境的desc*/

//str指向要转数值的空终止串
//*str_end会指向str中最后被转译字符的后一字符
//base指定进制,0为自动检测,或指定为[2,36]进制
long wcstol(const wchar_t *restrict str,wchar_t **restrict str_end,int base);
unsigned long wcstoul(const wchar_t *restrict str,wchar_t **restrict str_end,int base);
long long wcstoll(const wchar_t *restrict str,wchar_t **restrict str_end,int base);
unsigned long long wcstoull(const wchar_t *restrict str,wchar_t **restrict str_end,int base);
intmax_t wcstouimax(const wchar_t *restrict str,wchar_t **restrict str_end,int base);
uintmax_t wcstouimax(const wchar_t *restrict str,wchar_t **restrict str_end,int base);

//str为10,16进制浮点,INF,NAN,任何其它可由c地环境接受的表达式
float wcstof(const wchar_t *restrict str,wchar_t **restrict str_end);
double wcstod(const wchar_t *restrict str,wchar_t **restrict str_end);
long double wcstold(const wchar_t *restrict str,wchar_t **restrict str_end);

wchar_t *wcscpy(wchar_t *restrict dest,const wchar_t *restrict src);//返回dest地址
wchar_t *wcsncpy(wchar_t *restrict dest,const wchar_t *restrict src,size_t count);
errno_t wcscpy_s(wchar_t *restrict dest,rsize_t destsz,
           const wchar_t *restrict src);
errno_t wcsncpy_s(wchar_t *restrict dest,rsize_t destsz,
            const wchar_t *restrict src,rsize_t count);

wchar_t *wcscat( wchar_t *dest, const wchar_t *src );//src附加到dest,返回dest地址
wchar_t *wcscat( wchar_t *restrict dest, const wchar_t *restrict src );
errno_t wcscat_s(wchar_t *restrict dest, rsize_t destsz,
           const wchar_t *restrict src);

size_t wcslen(const wchar_t *str);//str length
size_t wcsnlen_s(const wchar_t *str,size_t strsz);//strsz为最大能访问长度

int wcscmp(const wchar_t *lhs,const wchar_t *rhs);//lhs-rhs
int wcsncmp(const wchar_t *lhs,const wchar_t *rhs,size_t count);

int wcscoll(const wchar_t *lhs,const wchar_t *rhs);//按LC_COLLATE排序比较

//wide character string transform转换为LC_COLLATE排序规则，
//保证wcscmp的结果和wcscoll一致
size_t wcsxfrm( wchar_t *restrict dest, const wchar_t *restrict src,
                size_t count );

//wide character string character正序查找ch
//wide character string reverse character逆序查找ch
//返回ch出现位置的指针或空指针
wchar_t *wcschr(const wchar_t *str,int ch);
wchar_t *wcsrchr(const wchar_t *str,int ch);

//wide character string span返回str中属于集合set中字符的最大连续长度
//wide character string complement span，匹配cset补集
size_t wcsspn(const wchar_t *str,const wchar_t *set);
size_t wcscspn(const wchar_t *str,const wchar_t *cset);

//wide character string pointer to break
//返回str中首次出现set集合字符的指针
//否则为空指针
wchar_t *wcspbrk(const wchar_t *str,const wchar_t *set);

//str中找substr,返回substr出现位置指针或空指针
wchar_t *wcsstr(const wchar_t *str,const wchar_t *substr);

//以delimeter集合分隔str为token(修改str)
//首次调用后str会在内部静态指针中
//后续str传入空指针调用返回下一个token
//wcstok_s中不再用内部静态指针而是ptr
wchar_t *wcstok(wchar_t *restrict str,const wchar_t *restrict delim);
wchar_t *wcstok_s(wchar_t *restrict str,const wchar_t *restrict strmax,
         const wchar_t *restrict delim,wchar_t **restrict ptr);

//在ptr的count个中找ch,返回其位置或空指针
wchar_t *wmemchr(const void *ptr,int ch,size_t count);

//lhs-rhs
wchar_t *wmemcmp(const void *lhs,const void *rhs,size_t count);

//以ch值覆盖dest的前count个,返回dest地址
//destsz指定dest大小
void *wmemset(void *dest,int ch,size_t count);
errno_t wmemset_s(void *dest,rsize_t destsz,
                     int ch,rsize_t count);

//src复制count个到dest,返回dest地址
//destsz指定dest大小
//wmemcpy不允许区域重叠，但最快的
//wmemccpy可以设置c为终止字节
void* wmemcpy( void *restrict dest, const void *restrict src, size_t count );
errno_t wmemcpy_s( void *restrict dest, rsize_t destsz,
            const void *restrict src, rsize_t count );
void wmemccpy(void *restrict dest,const void *restrict src,int c,size_t count);

//src移动count个到dest,返回dest地址
//destsz指定dest大小
//wmemmove允许区域重叠
void* wmemmove( void* dest, const void* src, size_t count );
errno_t wmemmove_s(void *dest, rsize_t destsz,
            const void *src, rsize_t count);
```


## <font color=#39c5bb> :rocket: math.h :rocket: </font>    

| 函数 | 公式 |
|------|------|
| `fabs(x)` | $|x|$ |
| `fmod(x,y)` | $x - n \cdot y,\; n=\text{trunc}(x/y)$ |
| `remainder(x,y)` | $IEEE余数:x - n \cdot y,\; n=\text{round}(x/y)$ |
| `fma(x,y,z)` | $fused\ multiply-add:x \cdot y + z$ |
| `fmax(x,y)` | $\max(x,y)$ |
| `fmin(x,y)` | $\min(x,y)$ |
| `fdim(x,y)` | $floating-point\ dimension:\max(0,x-y)$ |
| `exp(x)` | $e^x$ |
| `exp2(x)` | $2^x$ |
| `expm1(x)` | $e^x-1$ |
| `log(x)` | $\ln x$ |
| `log10(x)` | $\log_{10} x$ |
| `log2(x)` | $\log_{2} x$ |
| `log1p(x)` | $ln\ 1\ plus\ x:\ln(1+x)$ |
| `pow(x,y)` | $power:x^y$ |
| `sqrt(x)` | $square\ root:\sqrt{x}$ |
| `cbrt(x)` | $cube\ root:\sqrt[3]{x}$ |
| `hypot(x,y)` | $\sqrt{x^2+y^2}$ |
| `sin(x)` | $\sin x$ |
| `cos(x)` | $\cos x$ |
| `tan(x)` | $\tan x$ |
| `asin(x)` | $\arcsin x$ |
| `acos(x)` | $\arccos x$ |
| `atan(x)` | $\arctan x$ |
| `atan2(y,x)` | $\arctan\!\left(\tfrac{y}{x}\right)$ |
| `sinh(x)` | $\frac{e^x - e^{-x}}{2}$ |
| `cosh(x)` | $\frac{e^x + e^{-x}}{2}$ |
| `tanh(x)` | $\frac{e^x - e^{-x}}{e^x + e^{-x}},h:hyperbolic$ |
| `asinh(x)` | $\operatorname{arsinh} x$ |
| `acosh(x)` | $\operatorname{arcosh} x$ |
| `atanh(x)` | $\operatorname{artanh} x$ |
| `erf(x)`   | $ error\ function:\tfrac{2}{\sqrt{\pi}} \int_0 ^x e^{-t^2}\,dt$ |
| `erfc(x)` | $erf\ complement:1-\operatorname{erf}(x)$ |
| `tgamma(x)` | $true\ gramma function:\Gamma(x) = \int_0^{\infty} t^{x-1} e^{-t} \, dt(x > 0)$ |
| `lgamma(x)` | $\ln \Gamma(x)$ |
| `ceil(x)` | $ceiling:\lceil x \rceil上取整$ |
| `floor(x)` | $\lfloor x \rfloor下取整$ |
| `trunc(x)` | $\text{truncate}(x)截去小数部分$ |
| `round(x)` | $\text{round}(x)四舍五入$ |
| `rint(x)` | $round\ to\ integer:\text{rint}(x)最近偶数舍入$ |
| `frexp(x,exp)` | $fraction\ exponent:f=\frac{x}{2^{exp}},\;0.5\le fabs(f) <1$ |
| `ldexp(x,exp)` | $load\ exponent:x\cdot2^{exp}$ |
| `modf(x,&inpart)` | $\text{fraction} = x - \text{intpart},取整数和小数部分$ |
| `ilogb(x)` | $\lfloor \log _2 |x| \rfloor$ |
| `copysign(x,y)` | $|x|\cdot\operatorname{sign}(y),fabs(x)乘y符号$ |



<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>



