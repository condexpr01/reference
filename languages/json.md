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

# <font color=39c5bb> simdjson/simdjson(readonly) </font>

* ondemand, dom
```cpp
//simdjson有两套api:
//  dom: document object model会全部处理, parser用parse成员
//  ondemand: 按需处理, parser用iterate成员
```

* simdjson_result

```cpp
//The result of a simdjson operation that could fail.
//Gives the option of reading error codes, or throwing an exception by casting to the desired result.

template<typename T>
struct simdjson_result : public internal::simdjson_result_base<T> {

  simdjson_inline simdjson_result() noexcept;
  simdjson_inline simdjson_result(T &&value) noexcept;
  simdjson_inline simdjson_result(error_code error_code) noexcept;
  simdjson_inline simdjson_result(T &&value, error_code error) noexcept;

  simdjson_inline void tie(T &value, error_code &error) && noexcept;

  simdjson_warn_unused simdjson_inline error_code get(T &value) && noexcept;

  template <typename U = T>
  simdjson_warn_unused simdjson_inline error_code get(std::string &value) && noexcept {
    static_assert(std::is_same<U, std::string_view>::value, "SFINAE");
    std::string_view v;
    error_code error = std::forward<simdjson_result<T>>(*this).get(v);
    if (!error) {
      value.assign(v.data(), v.size());
    }
    return error;
  }

  simdjson_inline error_code error() const noexcept;

#if SIMDJSON_EXCEPTIONS
  using internal::simdjson_result_base<T>::operator*;
  using internal::simdjson_result_base<T>::operator->;

  simdjson_inline T& value() & noexcept(false);
  simdjson_inline T&& value() && noexcept(false);
  simdjson_inline T&& take_value() && noexcept(false);
  simdjson_inline operator T&&() && noexcept(false);
#endif // SIMDJSON_EXCEPTIONS

  simdjson_inline const T& value_unsafe() const& noexcept;
  simdjson_inline T&& value_unsafe() && noexcept;

  using value_type = T;
  using error_type = error_code;
};
```

* padded_string
```cpp
//simdjson::padded_string带填充，用simd指令的操作更安全
//simdjson::padded_string含load，可以加载json文件
```

* document
```cpp
//simdjson::ondemand::document和simdjson::dom::document
//对应ondemand和dom的parser解析的json
//可以用来访问json里的东西
```

* value
```cpp
//每个value都可以调用type(),枚举值为:
enum class json_type {
    unknown=0,
    // Start at 1 to catch uninitialized / default values more easily
    array=1, ///< A JSON array   ( [ 1, 2, 3 ... ] )
    object,  ///< A JSON object  ( { "a": 1, "b" 2, ... } )
    number,  ///< A JSON number  ( 1 or -2.3 or 4.5e6 ...)
    string,  ///< A JSON string  ( "a" or "hello world\n" ...)
    boolean, ///< A JSON boolean (true or false)
    null     ///< A JSON null    (null)
};

//每个value都可以调用is_<type> get_<type>来检测和转换json文本

```

* example
```cpp
ondemand::parser parser;
simdjson_result<padded_string> json{padded_string::load("compile_commands.json")};
simdjson_result<ondemand::document> document{parser.iterate(json)};
//now can access document
```


# <font color=39c5bb> Tencent/rapidjson </font>

```cpp
//含SAX DOM两种风格的api
//SAX: simpile api for xml
//DOM: document object model

//document.h : 将json转换成DOM Document对象
//reader.h : 将json转换成SAX Reader对象
//writer.h : 将DOM Document对象转换成json

//IsType/GetType 判断和获取
//SetXxx/AddMember 构建与修改

//Stringify需要使用Writer,StringBuffer对象和Document的Accept成员
```



<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>


