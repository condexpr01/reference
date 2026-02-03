# <font color=#ffe211>:star: LUA </font>

## <font color=#ffe211>:sparkles: LUA in EBNF </font>

```ebnf
<chunk> ::= <block>

(***************************************)
<block> ::= {<stat>} [<retstat>]

(***************************************)
<stat> ::=  ‘;’ | 
	 <varlist> ‘=’ <explist> | 
	 <functioncall> | 
	 <label> | 
	 "break" | 
	 "goto" <Name> | 
	 "do" <block> "end" | 
	 "while" <exp> "do" <block> "end" | 
	 "repeat" <block> "until" <exp> | 
	 "if" <exp> "then" <block> {"elseif" <exp> "then" <block>} ["else" <block>] "end" | 
	 "for" <Name> ‘=’ <exp> ‘,’ <exp> [‘,’ <exp>] "do" <block> "end" | 
	 "for" <namelist> "in" <explist> "do" <block> "end" | 
	 "function" <funcname> <funcbody> | 
	 "local" "function" <Name> <funcbody> | 
	 "local" <attnamelist> [‘=’ <explist>] 

<retstat> ::= "return" [<explist>] [‘;’]

(***************************************)
<varlist> ::= <var> {‘,’ <var>}
<explist> ::= <exp> {‘,’ <exp>}
<functioncall> ::=  <prefixexp> <args> | <prefixexp> ‘:’ <Name> <args> 
<label> ::= ‘::’ <Name> ‘::’

<Name> ::= ? name ?

<exp> ::=  "nil" | "false" | "true"
         | <Numeral> | <LiteralString>
         | ‘...’ | <functiondef>  
         | <prefixexp> | <tableconstructor>
         | <exp> <binop> <exp> | <unop> <exp> 

<namelist> ::= <Name> {‘,’ <Name>}

<funcname> ::= <Name> {‘.’ <Name>} [‘:’ <Name>]
<funcbody> ::= ‘(’ [<parlist>] ‘)’ <block> "end"

<attnamelist> ::=  <Name> <attrib> {‘,’ <Name> <attrib>}

(***************************************)
<var> ::=  <Name> | <prefixexp> ‘[’ <exp> ‘]’ | <prefixexp> ‘.’ <Name> 

<prefixexp> ::= <var> | <functioncall> | ‘(’ <exp> ‘)’
<args> ::=  ‘(’ [<explist>] ‘)’ | <tableconstructor> | <LiteralString> 

<Numeral> ::= ? Numeral ?
<LiteralString> ::= ? string literal ?

<functiondef> ::= <function> <funcbody>

<tableconstructor> ::= ‘{’ [<fieldlist>] ‘}’

<binop> ::=  ‘+’ | ‘-’ | ‘*’ | ‘/’ | ‘//’ | ‘^’ | ‘%’
           | ‘&’ | ‘~’ | ‘|’ | ‘>>’ | ‘<<’ | ‘..’
           | ‘<’ | ‘<=’ | ‘>’ | ‘>=’ | ‘==’ | ‘~=’
           | "and" | "or"

<unop> ::= ‘-’ | "not" | ‘#’ | ‘~’

<parlist> ::= <namelist> [‘,’ ‘...’] | ‘...’

<attrib> ::= [‘<’ <Name> ‘>’]

(***************************************)
<fieldlist> ::= <field> {<fieldsep> <field>} [<fieldsep>]

(***************************************)
<field> ::= ‘[’ <exp> ‘]’ ‘=’ <exp> 
           | <Name> ‘=’ <exp> 
           | <exp>

<fieldsep> ::= ‘,’ | ‘;’

```

## <font color=#ffe211>:sparkles: grammar details </font>

### <font color=#ffe211>:rocket: 基本类型 </font>

> <font color=#39c5bb> type函数可以查看 </font>

<font color=#ffa500>

|nil|空类型|
|:---:|:---|
|boolean|true或false|
|number|实数(浮点的)|
|string|8bit字节串,<br>""中,转义和c/cpp相同,除左右中括号也要转义<br>[[]]中无需转义|
|userdata|用户数据|
|function|函数|
|thread|线程|
|table|表|

</font>

### <font color=#ffe211>:rocket: 注释 </font>

```lua
--[[
注释串 
]]

--单行注释
```

### <font color=#ffe211>:rocket: 优先级 </font>

<font color=#ffa500>

|`^`|次方|
|:---|:---|
|`not` `-`|非,负|
|`*` `/`|乘除|
|`+` `-`|加减|
|`..`|串连接|
|`<` `>` `<=` `>=` `~=` `==`|关系|
|`and`|与|
|`or`|或|

</font>


### <font color=#ffe211>:rocket: 块</font>

> chunk,block的规则规定了return只能写在末尾
> 或者以do return end的形式出现在非末尾

```ebnf
<chunk> ::= <block>

(***************************************)
<block> ::= {<stat>} [<retstat>]

(***************************************)
<retstat> ::= "return" [<explist>] [‘;’]

```

### <font color=#ffe211>:rocket: 语句</font>

```ebnf
<stat> ::=  ‘;’ (* 空语句,并非c/cpp里的结尾,但常用做分隔 *)
	 | <varlist> ‘=’ <explist> (* 多去除,少补nil *) 
	 | <functioncall> (* 函数调用 *)
	 | <label> (* 标签 *)
	 | "break" (* 打断循环 *)
	 | "goto" <Name> (* 跳转到标签 *)
	 | "do" <block> "end" (* 块语句 *)
	 | "while" <exp> (* while循环,只有false和nil才停止 *)
	   "do" <block> "end" 
	 | "repeat" <block> 
	   "until" <exp> (* repeat循环,只有false和nil才停止 *)
	 | "if" <exp> "then" <block> (* if语句,只有false和nil为假 *)
      {"elseif" <exp> "then" <block>} 
	  ["else" <block>] 
	   "end"
	 | "for" <Name> ‘=’ <exp> ‘,’ <exp> [‘,’ <exp>] (* 数值for,exp分别为起始和结尾及步长 *)
	   "do" <block> "end" 
	 | "for" <namelist> "in" <explist> (* explist含f,t,i;namelist=f(t,i) *)
	   "do" <block> "end" 
	 | "function" <funcname> <funcbody> (* 函数定义 *)
	 | "local" "function" <Name> <funcbody> (* 局部函数 *)
	 | "local" <attnamelist> [‘=’ <explist>] (* 局部带属性变量 *)
```

### <font color=#ffe211>:rocket: 语句细节</font>

```ebnf
<varlist> ::= <var> {‘,’ <var>}
<explist> ::= <exp> {‘,’ <exp>}

(* ':'调用将对象作为首个参数,面向对象的调用方法 *)
(* 等价于prefixexp.name(prefixexp,args) *)
<functioncall> ::=  <prefixexp> <args> 
                  | <prefixexp> ‘:’ <Name> <args> 

<label> ::= ‘::’ <Name> ‘::’

<Name> ::= ? name ?

(* function,table可通过exp给var获名 *)
<exp> ::=  "nil" | "false" | "true"
         | <Numeral> | <LiteralString>
         | ‘...’ | <functiondef>  
         | <prefixexp> | <tableconstructor>
         | <exp> <binop> <exp> | <unop> <exp> 

<namelist> ::= <Name> {‘,’ <Name>}

(* '.'表示表成员 *)
(* ':'表示方法成员,类似于cpp类成员函数,用self表示this *)
(* function t:f(x)等价于function t.f(self,x) *)
<funcname> ::= <Name> {‘.’ <Name>} [‘:’ <Name>]

<funcbody> ::= ‘(’ [<parlist>] ‘)’ <block> "end"

(* 带属性的变量 *)
(* <const>表常量 *)
(* <close>作用域结束调用__close元方法 *)
<attnamelist> ::=  <Name> <attrib> {‘,’ <Name> <attrib>}

(***************************************)
(* table成员可以[]下标访问,也可以.访问 *)
<var> ::=  <Name> 
         | <prefixexp> ‘[’ <exp> ‘]’ 
         | <prefixexp> ‘.’ <Name> 

<prefixexp> ::= <var> | <functioncall> | ‘(’ <exp> ‘)’

(* 单参数tableconstructor,LiteralString时可不要左右括号 *)
<args> ::=  ‘(’ [<explist>] ‘)’ 
           | <tableconstructor> 
           | <LiteralString> 

<Numeral> ::= ? Numeral ?
<LiteralString> ::= ? string literal ?

<functiondef> ::= <function> <funcbody>

<tableconstructor> ::= ‘{’ [<fieldlist>] ‘}’

<binop> ::=  ‘+’ | ‘-’ | ‘*’ | ‘/’ | ‘//’ | ‘^’ | ‘%’
           | ‘&’ | ‘~’ | ‘|’ | ‘>>’ | ‘<<’ | ‘..’
           | ‘<’ | ‘<=’ | ‘>’ | ‘>=’ | ‘==’ | ‘~=’
           | "and" | "or"

<unop> ::= ‘-’ | "not" | ‘#’ | ‘~’

(* 参数列表 *)
<parlist> ::= <namelist> [‘,’ ‘...’] | ‘...’

(* <const>表常量 *)
(* <close>作用域结束调用__close元方法 *)
<attrib> ::= [‘<’ <Name> ‘>’]

(***************************************)
(* 表的域列表 *)
<fieldlist> ::= <field> {<fieldsep> <field>} [<fieldsep>]

(***************************************)
(* 域成员可以下标方式定义 *)
(* 也可以用名字 *)
(* 用exp加入function,table等 *)
<field> ::= ‘[’ <exp> ‘]’ ‘=’ <exp> 
           | <Name> ‘=’ <exp> 
           | <exp>

(* 域分隔符 *)
<fieldsep> ::= ‘,’ | ‘;’

```

### <font color=#ffe211>:rocket: 函数 </font>
> <font color=#39c5bb> 所有函数都是lambda以引用捕获的函数,和upvalue(外部局部变量)有关 </font>    
> <font color=#39c5bb> 单参数时可选'(',')',类似cpp调用构造函数 </font>    
> <font color=#39c5bb> 可返回多个值，不足补nil，超出则舍去 </font>    
> <font color=#39c5bb> 用(f())可以强制使调用只返回一个值 </font>    
> <font color=#39c5bb> table.unpack可以解包使表变为元素列表 </font>    
> <font color=#39c5bb> 形参中...可以直接用...表示,类似cpp形参包,可如pairs({...})写法 </font>    


# <font color=#ffe211>:star: 标准库 </font>

## <font color=#ffe211>:sparkles: basic </font>

```lua
-- Loads the given module.
-- The function starts by looking into the package.loaded table
-- to determine whether modname is already loaded.
-- If it is, then require returns the value stored at package.loaded[modname].
-- (The absence of a second result in this case signals 
-- that this call did not have to load the module.)
-- Otherwise, it tries to find a loader for the module.
-- To find a loader, require is guided by the table package.searchers.
-- Each item in this table is a search function,
-- that searches for the module in a particular way.
-- By changing this table, we can change how require looks for a module.
-- The following explanation is based on the default configuration for package.searchers.
-- First require queries package.preload[modname].
-- If it has a value, this value (which must be a function) is the loader.
-- Otherwise require searches for a Lua loader using the path stored in package.path.
-- If that also fails, it searches for a C loader using the path stored in package.cpath.
-- If that also fails, it tries an all-in-one loader (see package.searchers).
-- Once a loader is found, require calls the loader with two arguments:
-- modname and an extra value, a loader data, also returned by the searcher.
-- The loader data can be any value useful to the module;
-- for the default searchers, it indicates where the loader was found.
-- (For instance, if the loader came from a file, this extra value is the file path.)
-- If the loader returns any non-nil value,
-- require assigns the returned value to package.loaded[modname].
-- If the loader does not return a non-nil value 
-- and has not assigned any value to package.loaded[modname],
-- then require assigns true to this entry.
-- In any case, require returns the final value of package.loaded[modname].
-- Besides that value, require also returns 
-- as a second result the loader data returned by the searcher,
-- which indicates how require found the module.
-- If there is any error loading or running the module,
-- or if it cannot find any loader for the module, then require raises an error.
require(modname)

-- Raises an error if the value of its argument v is false (i.e., nil or false);
-- otherwise, returns all its arguments.
-- In case of error, message is the error object;
-- when absent, it defaults to "assertion failed!"
assert (v [, message])

-- This function is a generic interface to the garbage collector.
-- It performs different functions according to its first argument, opt:
-- --"collect": Performs a full garbage-collection cycle. This is the default option.
-- --"stop": Stops automatic execution of the garbage collector.
-- --        The collector will run only when explicitly invoked,
-- --        until a call to restart it.
-- --"restart": Restarts automatic execution of the garbage collector.
-- -- "count": Returns the total memory in use by Lua in Kbytes.
-- --          The value has a fractional part,
-- --          so that it multiplied by 1024 gives 
-- --          the exact number of bytes in use by Lua.
-- --"step": Performs a garbage-collection step. The step "size" is controlled by arg.
-- --        With a zero value, the collector will perform one basic (indivisible) step.
-- --        For non-zero values, the collector will perform 
-- --        as if that amount of memory (in Kbytes) had been allocated by Lua.
-- --        Returns true if the step finished a collection cycle.
-- --"isrunning": Returns a boolean that tells
-- --             whether the collector is running (i.e., not stopped).
-- --"incremental": Change the collector mode to incremental.
-- --               This option can be followed by three numbers:
-- --               the garbage-collector pause, the step multiplier, and the step size.
-- --               A zero means to not change that value.
-- --"generational": Change the collector mode to generational.
-- --                This option can be followed by two numbers:
-- --                the garbage-collector minor multiplier and the major multiplier.
-- --                A zero means to not change that value.
collectgarbage ([opt [, arg]])

-- Opens the named file and executes its content as a Lua chunk.
-- When called without arguments,
-- dofile executes the content of the standard input (stdin).
-- Returns all values returned by the chunk.
-- In case of errors, dofile propagates the error to its caller.
-- (That is, dofile does not run in protected mode.)
dofile ([filename])



-- Raises an error with message as the error object.
-- This function never returns.
-- Usually, error adds some information about the error position
-- at the beginning of the message, if the message is a string.
-- The level argument specifies how to get the error position.
-- With level 1 (the default),
-- the error position is where the error function was called.
-- Level 2 points the error to where the function 
-- that called error was called; and so on.
-- Passing a level 0 avoids the addition of error position information to the message.
error (message [, level])


-- A global variable (not a function) that holds the global environment.
-- Lua itself does not use this variable;
-- changing its value does not affect any environment, nor vice versa.
_G


-- If object does not have a metatable, returns nil.
-- Otherwise, if the object's metatable has a __metatable field,
-- returns the associated value.
-- Otherwise, returns the metatable of the given object.
getmetatable (object)


-- Returns three values
-- (an iterator function, the table t, and 0)
-- so that the construction
ipairs (t)



-- Loads a chunk.
-- If chunk is a string, the chunk is this string.
-- If chunk is a function, load calls it repeatedly to get the chunk pieces.
-- Each call to chunk must return a string that concatenates with previous results.
-- A return of an empty string, nil, or no value signals the end of the chunk.
--
-- If there are no syntactic errors, load returns the compiled chunk as a function;
-- otherwise, it returns fail plus the error message.
--
-- When you load a main chunk,
-- the resulting function will always have exactly one upvalue,the _ENV variable.
-- However, when you load a binary chunk created from a function (see string.dump),
-- the resulting function can have an arbitrary number of upvalues,
-- and there is no guarantee that its first upvalue will be the _ENV variable.
-- (A non-main function may not even have an _ENV upvalue.)
--
-- Regardless, if the resulting function has any upvalues,
-- its first upvalue is set to the value of env,
-- if that parameter is given, or to the value of the global environment.
-- Other upvalues are initialized with nil.
-- All upvalues are fresh, that is, they are not shared with any other function.
--
-- chunkname is used as the name of the chunk for error messages and debug information.
-- When absent, it defaults to chunk, if chunk is a string, or to "=(load)" otherwise.
--
-- The string mode controls whether the chunk can be text or binary
-- (that is, a precompiled chunk).
-- It may be the string
-- "b" (only binary chunks),
-- "t" (only text chunks),
-- or "bt" (both binary and text).
-- The default is "bt".
--
-- It is safe to load malformed binary chunks;
-- load signals an appropriate error.
-- However, Lua does not check the consistency
-- of the code inside binary chunks;
-- running maliciously crafted bytecode can crash the interpreter.
load (chunk [, chunkname [, mode [, env]]])

-- Similar to load, but gets the chunk from file filename
-- or from the standard input, if no file name is given.
loadfile ([filename [, mode [, env]]])

-- Allows a program to traverse all fields of a table.
-- Its first argument is a table 
-- and its second argument is an index in this table.
-- A call to next returns the next index 
-- of the table and its associated value.
-- When called with nil as its second argument,
-- next returns an initial index and its associated value.
-- When called with the last index,
-- or with nil in an empty table,
-- next returns nil.
-- If the second argument is absent, then it is interpreted as nil.
-- In particular, you can use next(t) to check whether a table is empty.
--
-- The order in which the indices are enumerated is not specified,
-- even for numeric indices.
-- (To traverse a table in numerical order, use a numerical for.)
--
-- You should not assign any value to 
-- a non-existent field in a table during its traversal.
-- You may however modify existing fields.
-- In particular, you may set existing fields to nil.
next (table [, index])

-- If t has a metamethod __pairs,
-- calls it with t as argument 
-- and returns the first three results from the call.
-- Otherwise, returns three values:
-- the next function, the table t, and nil, so that the construction
pairs (t)

-- Calls the function f with the given arguments in protected mode.
-- This means that any error inside f is not propagated;
-- instead, pcall catches the error and returns a status code.
-- Its first result is the status code (a boolean),
-- which is true if the call succeeds without errors.
-- In such case, pcall also returns all results from the call,
-- after this first result.
-- In case of any error, pcall returns false plus the error object.
-- Note that errors caught by pcall do not call a message handler.
pcall (f [, arg1, ···])

-- Receives any number of arguments and prints their values to stdout,
-- converting each argument to a string following the same rules of tostring.
-- The function print is not intended for formatted output,
-- but only as a quick way to show a value,for instance for debugging.
-- For complete control over the output, use string.format and io.write.
print (···)

-- Checks whether v1 is equal to v2,
-- without invoking the __eq metamethod. Returns a boolean.
rawequal (v1, v2)

-- Gets the real value of table[index],
-- without using the __index metavalue.
-- table must be a table; index may be any value.
rawget (table, index)

-- Returns the length of the object v,
-- which must be a table or a string,
-- without invoking the __len metamethod.
-- Returns an integer.
rawlen (v)

-- Sets the real value of table[index] to value,
-- without using the __newindex metavalue.
-- table must be a table,
-- index any value different from nil and NaN,
-- and value any Lua value.
-- This function returns table.
rawset (table, index, value)

-- If index is a number,
-- returns all arguments after argument number index;
-- a negative number indexes from the end (-1 is the last argument).
-- Otherwise, index must be the string "#",
-- and select returns the total number of extra arguments it received.
select (index, ···)

-- Sets the metatable for the given table.
-- If metatable is nil, removes the metatable of the given table.
-- If the original metatable has a __metatable field,
-- raises an error.
-- This function returns table.
-- To change the metatable of other types from Lua code,
-- you must use the debug library.
setmetatable (table, metatable)


-- When called with no base,
-- tonumber tries to convert its argument to a number.
-- If the argument is already a number or a string convertible to a number,
-- then tonumber returns this number;
-- otherwise, it returns fail.
-- The conversion of strings can result in integers or floats,
-- according to the lexical conventions of Lua.
-- The string may have leading and trailing spaces and a sign.
-- When called with base,
-- then e must be a string to be interpreted as an integer numeral in that base.
-- The base may be any integer between 2 and 36, inclusive.
-- In bases above 10, the letter 'A' (in either upper or lower case) represents 10,
-- 'B' represents 11, and so forth, with 'Z' representing 35.
-- If the string e is not a valid numeral in the given base, the function returns fail.
tonumber (e [, base])

-- Receives a value of any type and converts it to a string in a human-readable format.
-- If the metatable of v has a __tostring field,
-- then tostring calls the corresponding value with v as argument,
-- and uses the result of the call as its result.
-- Otherwise, if the metatable of v has a __name field with a string value,
-- tostring may use that string in its final result.
-- For complete control of how numbers are converted, use string.format.
tostring (v)

-- Returns the type of its only argument,
-- coded as a string.
-- The possible results of this function are "nil"
-- (a string, not the value nil),
-- "number", "string", "boolean",
-- "table", "function", "thread",
-- and "userdata".
type (v)

-- A global variable (not a function)
-- that holds a string containing the running Lua version.
-- The current value of this variable is "Lua 5.4".
_VERSION

-- Emits a warning with a message 
-- composed by the concatenation of all its arguments
-- (which should be strings).
-- By convention, a one-piece message starting with '@'
-- is intended to be a control message,
-- which is a message to the warning system itself.
-- In particular, the standard warning function in lua
-- recognizes the control messages "@off",
-- to stop the emission of warnings,
-- and "@on", to (re)start the emission;
-- it ignores unknown control messages.
warn (msg1, ···)

-- This function is similar to pcall,
-- except that it sets a new message handler msgh.
xpcall (f, msgh [, arg1, ···])
```

## <font color=#ffe211>:sparkles: coroutine</font>

```lua
coroutine
coroutine.close
coroutine.create
coroutine.isyieldable
coroutine.resume
coroutine.running
coroutine.status
coroutine.wrap
coroutine.yield
```

## <font color=#ffe211>:sparkles: debug</font>

```lua
debug.debug
debug.gethook
debug.getinfo
debug.getlocal
debug.getmetatable
debug.getregistry
debug.getupvalue
debug.getuservalue
debug.sethook
debug.setlocal
debug.setmetatable
debug.setupvalue
debug.setuservalue
debug.traceback
debug.upvalueid
debug.upvaluejoin
```

## <font color=#ffe211>:sparkles: io</font>

```lua
io.close
io.flush
io.input
io.lines
io.open
io.output
io.popen
io.read
io.stderr
io.stdin
io.stdout
io.tmpfile
io.type
io.write
file:close
file:flush
file:lines
file:read
file:seek
file:setvbuf
file:write
```

 
## <font color=#ffe211>:sparkles: math</font>

```lua
math.abs
math.acos
math.asin
math.atan
math.ceil
math.cos
math.deg
math.exp
math.floor
math.fmod
math.huge
math.log
math.max
math.maxinteger
math.min
math.mininteger
math.modf
math.pi
math.rad
math.random
math.randomseed
math.sin
math.sqrt
math.tan
math.tointeger
math.type
math.ult
```

## <font color=#ffe211>:sparkles: os</font>

```lua
os.clock
os.date
os.difftime
os.execute
os.exit
os.getenv
os.remove
os.rename
os.setlocale
os.time
os.tmpname
```

## <font color=#ffe211>:sparkles: package</font>

```lua
package.config
package.cpath
package.loaded
package.loadlib
package.path
package.preload
package.searchers
package.searchpath
```

## <font color=#ffe211>:sparkles: string</font>

```lua
string.byte
string.char
string.dump
string.find
string.format
string.gmatch
string.gsub
string.len
string.lower
string.match
string.pack
string.packsize
string.rep
string.reverse
string.sub
string.unpack
string.upper
```

## <font color=#ffe211>:sparkles: table</font>

```lua
table.concat
table.insert
table.move
table.pack
table.remove
table.sort
table.unpack
```

## <font color=#ffe211>:sparkles: utf8</font>

```lua
utf8.char
utf8.charpattern
utf8.codepoint
utf8.codes
utf8.len
utf8.offset
```

## <font color=#ffe211>:sparkles: metamethods</font>

```lua
__add
__band
__bnot
__bor
__bxor
__call
__close
__concat
__div
__eq
__gc
__idiv
__index
__le
__len
__lt
__metatable
__mod
__mode
__mul
__name
__newindex
__pairs
__pow
__shl
__shr
__sub
__tostring
__unm
```

## <font color=#ffe211>:sparkles: environment variables</font>

```lua
LUA_CPATH
LUA_CPATH_5_4
LUA_INIT
LUA_INIT_5_4
LUA_PATH
LUA_PATH_5_4
```

## <font color=#ffe211>:sparkles: c api</font>

```lua
lua_Alloc
lua_CFunction
lua_Debug
lua_Hook
lua_Integer
lua_KContext
lua_KFunction
lua_Number
lua_Reader
lua_State
lua_Unsigned
lua_WarnFunction
lua_Writer

lua_absindex
lua_arith
lua_atpanic
lua_call
lua_callk
lua_checkstack
lua_close
lua_closeslot
lua_compare
lua_concat
lua_copy
lua_createtable
lua_dump
lua_error
lua_gc
lua_getallocf
lua_getextraspace
lua_getfield
lua_getglobal
lua_gethook
lua_gethookcount
lua_gethookmask
lua_geti
lua_getinfo
lua_getiuservalue
lua_getlocal
lua_getmetatable
lua_getstack
lua_gettable
lua_gettop
lua_getupvalue
lua_insert
lua_isboolean
lua_iscfunction
lua_isfunction
lua_isinteger
lua_islightuserdata
lua_isnil
lua_isnone
lua_isnoneornil
lua_isnumber
lua_isstring
lua_istable
lua_isthread
lua_isuserdata
lua_isyieldable
lua_len
lua_load
lua_newstate
lua_newtable
lua_newthread
lua_newuserdatauv
lua_next
lua_numbertointeger
lua_pcall
lua_pcallk
lua_pop
lua_pushboolean
lua_pushcclosure
lua_pushcfunction
lua_pushfstring
lua_pushglobaltable
lua_pushinteger
lua_pushlightuserdata
lua_pushliteral
lua_pushlstring
lua_pushnil
lua_pushnumber
lua_pushstring
lua_pushthread
lua_pushvalue
lua_pushvfstring
lua_rawequal
lua_rawget
lua_rawgeti
lua_rawgetp
lua_rawlen
lua_rawset
lua_rawseti
lua_rawsetp
lua_register
lua_remove
lua_replace
lua_resetthread
lua_resume
lua_rotate
lua_setallocf
lua_setfield
lua_setglobal
lua_sethook
lua_seti
lua_setiuservalue
lua_setlocal
lua_setmetatable
lua_settable
lua_settop
lua_setupvalue
lua_setwarnf
lua_status
lua_stringtonumber
lua_toboolean
lua_tocfunction
lua_toclose
lua_tointeger
lua_tointegerx
lua_tolstring
lua_tonumber
lua_tonumberx
lua_topointer
lua_tostring
lua_tothread
lua_touserdata
lua_type
lua_typename
lua_upvalueid
lua_upvalueindex
lua_upvaluejoin
lua_version
lua_warning
lua_xmove
lua_yield
lua_yieldk
```

## <font color=#ffe211>:sparkles: auxiliary library</font>

```lua
luaL_Buffer
luaL_Reg
luaL_Stream

luaL_addchar
luaL_addgsub
luaL_addlstring
luaL_addsize
luaL_addstring
luaL_addvalue
luaL_argcheck
luaL_argerror
luaL_argexpected
luaL_buffaddr
luaL_buffinit
luaL_buffinitsize
luaL_bufflen
luaL_buffsub
luaL_callmeta
luaL_checkany
luaL_checkinteger
luaL_checklstring
luaL_checknumber
luaL_checkoption
luaL_checkstack
luaL_checkstring
luaL_checktype
luaL_checkudata
luaL_checkversion
luaL_dofile
luaL_dostring
luaL_error
luaL_execresult
luaL_fileresult
luaL_getmetafield
luaL_getmetatable
luaL_getsubtable
luaL_gsub
luaL_len
luaL_loadbuffer
luaL_loadbufferx
luaL_loadfile
luaL_loadfilex
luaL_loadstring
luaL_newlib
luaL_newlibtable
luaL_newmetatable
luaL_newstate
luaL_openlibs
luaL_opt
luaL_optinteger
luaL_optlstring
luaL_optnumber
luaL_optstring
luaL_prepbuffer
luaL_prepbuffsize
luaL_pushfail
luaL_pushresult
luaL_pushresultsize
luaL_ref
luaL_requiref
luaL_setfuncs
luaL_setmetatable
luaL_testudata
luaL_tolstring
luaL_traceback
luaL_typeerror
luaL_typename
luaL_unref
luaL_where
```

## <font color=#ffe211>:sparkles: standard library</font>

```lua
luaopen_base
luaopen_coroutine
luaopen_debug
luaopen_io
luaopen_math
luaopen_os
luaopen_package
luaopen_string
luaopen_table
luaopen_utf8
```

## <font color=#ffe211>:sparkles: constants</font>

```lua
LUA_ERRERR
LUA_ERRFILE
LUA_ERRMEM
LUA_ERRRUN
LUA_ERRSYNTAX
LUA_HOOKCALL
LUA_HOOKCOUNT
LUA_HOOKLINE
LUA_HOOKRET
LUA_HOOKTAILCALL
LUA_LOADED_TABLE
LUA_MASKCALL
LUA_MASKCOUNT
LUA_MASKLINE
LUA_MASKRET
LUA_MAXINTEGER
LUA_MININTEGER
LUA_MINSTACK
LUA_MULTRET
LUA_NOREF
LUA_OK
LUA_OPADD
LUA_OPBAND
LUA_OPBNOT
LUA_OPBOR
LUA_OPBXOR
LUA_OPDIV
LUA_OPEQ
LUA_OPIDIV
LUA_OPLE
LUA_OPLT
LUA_OPMOD
LUA_OPMUL
LUA_OPPOW
LUA_OPSHL
LUA_OPSHR
LUA_OPSUB
LUA_OPUNM
LUA_PRELOAD_TABLE
LUA_REFNIL
LUA_REGISTRYINDEX
LUA_RIDX_GLOBALS
LUA_RIDX_MAINTHREAD
LUA_TBOOLEAN
LUA_TFUNCTION
LUA_TLIGHTUSERDATA
LUA_TNIL
LUA_TNONE
LUA_TNUMBER
LUA_TSTRING
LUA_TTABLE
LUA_TTHREAD
LUA_TUSERDATA
LUA_USE_APICHECK
LUA_YIELD
LUAL_BUFFERSIZE
```


<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>

