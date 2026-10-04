
# CMakeLists.txt

> Version: 4.2.3

```ebnf
<file>         ::= {<file_element>}

(***************************************)
<file_element> ::= <command_invocation> <line_ending>
				| {(<bracket_comment> | <space>)} <line_ending>

(***************************************)

<command_invocation>  ::= {<space>} <identifier> {<space>} '(' <arguments> ')'

<bracket_comment> ::= '#' <bracket_argument>

<line_ending>  ::= [<line_comment>] <newline>
<space>        ::= ? match '[ \t]+' ?

(***************************************)

<identifier>   ::= ? match '[A-Za-z_][A-Za-z0-9_]*' ?
<arguments>    ::= [<argument>] {<separated_arguments>}

<newline>      ::= ? match '\n' ?

(***************************************)

<argument> ::= <bracket_argument> | <quoted_argument> | <unquoted_argument>

<separated_arguments> ::= <separation> {<separation>} [<argument>] 
						| {<separation>} '(' <arguments> ')'

(***************************************)
<separation>          ::= <space> | <line_ending>

<quoted_argument>  ::= '"' {<quoted_element>} '"'

<unquoted_argument> ::= <unquoted_element> {<unquoted_element>} 
					| <unquoted_legacy>

(***************************************)
<bracket_argument> ::= <bracket_open> <bracket_content> <bracket_close>

<quoted_element>      ::= ? any character except '\' or '"' ?
						| <escape_sequence>
						| <quoted_continuation>

<unquoted_element>  ::= ? any character except whitespace or one of '()#"\'? 
						| <escape_sequence>

<unquoted_legacy>   ::= <see note in text>

(***************************************)

(* 作者说从lua [[]]得到的灵感 *)
<bracket_open>     ::= '[' {'='} '['
<bracket_close>    ::= ']' {'='} ']'
<bracket_content>  ::= ? any text not containing a bracket_close with
                     the same number of '=' as the bracket_open ?

<quoted_continuation> ::= '\' <newline>

<escape_sequence>  ::= <escape_identity> | <escape_encoded> | <escape_semicolon>

(***************************************)
<escape_identity>  ::= '\' ? match '[^A-Za-z0-9;]' ?
<escape_encoded>   ::= '\t' | '\r' | '\n'
<escape_semicolon> ::= '\;'

```


# 控制结构

```cmake
# condition
#在括号内,
#一元测试如 COMMAND、POLICY、TARGET、TEST、EXISTS、
#IS_READABLE、IS_WRITABLE、IS_EXECUTABLE、
#IS_DIRECTORY、IS_SYMLINK、IS_ABSOLUTE 、 DEFINED。
#二进制检验如EQUAL、LESS、LESS_EQUAL、GREATER、GREATER_EQUAL、
#STREQUAL、STRLESS、STRLESS_EQUAL、STRGREATER、STRGREATER_EQUAL、
#VERSION_EQUAL、VERSION_LESS、VERSION_LESS_EQUAL、VERSION_GREATER、
#VERSION_GREATER_EQUAL、PATH_EQUAL、IN_LIST、IS_NEWER_THAN，MATCHES。
#一元逻辑运算符NOT。
#二元逻辑算符 AND 和 OR，从左到右， 没有短路。

# 条件
if(<condition>)
  <commands>
elseif(<condition>) # optional block, can be repeated
  <commands>
else()              # optional block
  <commands>
endif()

# foreach循环
foreach(<loop_var> <items>)
## items: RANGE <stop>
## items: RANGE <start> <stop> [<step>]
## items: IN [LISTS [<lists>]] [ITEMS [<items>]]
  <commands>
endforeach()

# while循环
while(<condition>)
  <commands>
endwhile()

# break/continue
break()/continue()

# 宏
macro(<name> [<arg1> ...])
  <commands>
endmacro()

# 函数(ARGC:参数个数,ARGV:所有参数的列表,ARGN:额外参数的列表,ARGV0,ARGV1...:索引)
function(<name> [<arg1> ...])
  <commands>
endfunction()

return([PROPAGATE <var-name>...])

# CMAKELANG的所有变量都是字符串或列表
# 值（总是串）,可用'$' [<scope>] '{' <identifier> '}'引用
# 列表以';'号分隔
# 普通变量用在当前及子目录,CACHE在所有cmake会话,ENV在外部环境
# type: BOOL, FILEPATH, PATH, STRING, INTERNAL
set(<variable> <value>... [PARENT_SCOPE])
set(CACHE{<variable>} [TYPE <type>] [HELP <helpstring>...] [FORCE] VALUE [<value>...])
set(ENV{<variable>} [<value>])

option(<variable> "<help_text>" [value])

unset(<variable> [PARENT_SCOPE])
unset(CACHE{<variable>})
unset(ENV{<variable>})

# 块
block([SCOPE_FOR [POLICIES] [VARIABLES]] [PROPAGATE <var-name>...])
  <commands>
endblock()

#包含cmakelang文件
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>] [NO_POLICY_SCOPE])
include_guard([DIRECTORY|GLOBAL])

#风格检测
#通过CMAKE_<LANG>_COMPILER_FRONTEND_VARIANT
#非空时可能值：GNU, MSVC

#cmake_pkg_config会设置PKGCONFIG_<package>_FOUND指示是否找到包
#生成新目标的名字叫PkgConfig::<package>
```


# Commands

```cmake
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [COMPAT_VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [SPDX_LICENSE <license-string>]
        [DESCRIPTION <description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])

cmake_minimum_required(VERSION <min>[...<policy_max>] [FATAL_ERROR])

cmake_policy(VERSION <min>[...<max>])

add_executable(<name> <options>... <sources>...)
add_executable(<name> IMPORTED [GLOBAL])
add_executable(<name> ALIAS <target>)

target_sources(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
target_sources(<target>
		[{INTERFACE|PUBLIC|PRIVATE}
		[FILE_SET <set> [TYPE <type>] [BASE_DIRS <dirs>...] [FILES <files>...]]...]...)

#type: SHARED(.so,.dll,.dylib), STATIC(.a), MODULE(allow undef symble, plugin...), OBJECT(.o), INTERFACE(logical)
#BUILD_SHARED_LIBS control beahavior if not type specified
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)
add_library(<name> OBJECT <sources>...)
add_library(<name> INTERFACE)
add_library(<name> INTERFACE [EXCLUDE_FROM_ALL] <sources>...)
add_library(<name> INTERFACE SYMBOLIC)
add_library(<name> <type> IMPORTED [GLOBAL])
add_library(<name> ALIAS <target>)

target_link_libraries(<target>
  {INTERFACE|PUBLIC|PRIVATE} <item>...
  [{INTERFACE|PUBLIC|PRIVATE} <item>...]...)
target_link_libraries(<target> <item>...)

add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL] [SYSTEM])

message([<mode>] "message text" ...)
message(<checkState> "message text" ...)
message(CONFIGURE_LOG <text>...)

#';'分号分隔的串
#Reading
list(LENGTH <list> <out-var>)
list(GET <list> <element index> [<index> ...] <out-var>)
list(JOIN <list> <glue> <out-var>)
list(SUBLIST <list> <begin> <length> <out-var>)
#Search
list(FIND <list> <value> <out-var>)
#Modification
list(APPEND <list> [<element>...])
list(FILTER <list> {INCLUDE | EXCLUDE} REGEX <regex>)
list(INSERT <list> <index> [<element>...])
list(POP_BACK <list> [<out-var>...])
list(POP_FRONT <list> [<out-var>...])
list(PREPEND <list> [<element>...])
list(REMOVE_ITEM <list> <value>...)
list(REMOVE_AT <list> <index>...)
list(REMOVE_DUPLICATES <list>)
list(TRANSFORM <list> <ACTION> [...])
#Ordering
list(REVERSE <list>)
list(SORT <list> [...])

#feature: cxx_std_YY ...
target_compile_features(<target> <PRIVATE|PUBLIC|INTERFACE> <feature> [...])

target_compile_definitions(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

get_target_property(<variable> <target> <property>)

set_target_properties(<targets> ...
		PROPERTIES <prop1> <value1>
		[<prop2> <value2>] ...)

target_compile_options(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_precompile_headers(<target>
  <INTERFACE|PUBLIC|PRIVATE> [header1...]
  [<INTERFACE|PUBLIC|PRIVATE> [header2...] ...])

target_link_options(<target> [BEFORE]
	<INTERFACE|PUBLIC|PRIVATE> [items1...]
	[<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_link_directories(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

add_custom_command(OUTPUT output1 [output2 ...]
                   COMMAND command1 [ARGS] [args1...]
                   [COMMAND command2 [ARGS] [args2...] ...]
                   [MAIN_DEPENDENCY depend]
                   [DEPENDS [depends...]]
                   [BYPRODUCTS [files...]]
                   [IMPLICIT_DEPENDS <lang1> depend1
                                    [<lang2> depend2] ...]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment]
                   [DEPFILE depfile]
                   [JOB_POOL job_pool]
                   [JOB_SERVER_AWARE <bool>]
                   [VERBATIM] [APPEND] [USES_TERMINAL]
                   [CODEGEN]
                   [COMMAND_EXPAND_LISTS]
                   [DEPENDS_EXPLICIT_ONLY])
add_custom_target(Name [ALL] [command1 [args1...]]
                  [COMMAND command2 [args2...] ...]
                  [DEPENDS depend depend depend ...]
                  [BYPRODUCTS [files...]]
                  [WORKING_DIRECTORY dir]
                  [COMMENT comment]
                  [JOB_POOL job_pool]
                  [JOB_SERVER_AWARE <bool>]
                  [VERBATIM] [USES_TERMINAL]
                  [COMMAND_EXPAND_LISTS]
                  [SOURCES src1 [src2...]])

add_compile_definitions(<definition> ...)
add_compile_options(<option> ...)

add_definitions(-DFOO -DBAR ...)
add_dependencies(<target> <target-dependency>...)

add_link_options(<option> ...)

enable_testing()

add_test(NAME <name> COMMAND <command> [<arg>...]
         [CONFIGURATIONS <config>...]
         [WORKING_DIRECTORY <dir>]
         [COMMAND_EXPAND_LISTS])


#RUNTIME ${CMAKE_INSTALL_BINDIR} bin
#LIBRARY ${CMAKE_INSTALL_LIBDIR} lib
#ARCHIVE ${CMAKE_INSTALL_LIBDIR} lib
#PRIVATE_HEADER ${CMAKE_INSTALL_INCLUDEDIR} include
#PUBLIC_HEADER ${CMAKE_INSTALL_INCLUDEDIR} include
#FILE_SET (类型 HEADERS) ${CMAKE_INSTALL_INCLUDEDIR} include
install(TARGETS <target>... [...])
install(IMPORTED_RUNTIME_ARTIFACTS <target>... [...])
install({FILES | PROGRAMS} <file>... [...])
install(DIRECTORY <dir>... [...])
install(SCRIPT <file> [...])
install(CODE <code> [...])
install(EXPORT <export-name> [...])
install(PACKAGE_INFO <package-name> [...])
install(RUNTIME_DEPENDENCY_SET <set-name> [...])

#Reading
file(READ <filename> <out-var> [...])
file(STRINGS <filename> <out-var> [...])
file(<HASH> <filename> <out-var>)
file(TIMESTAMP <filename> <out-var> [...])
#Writing
file({WRITE | APPEND} <filename> <content>...)
file({TOUCH | TOUCH_NOCREATE} <file>...)
file(GENERATE OUTPUT <output-file> [...])
file(CONFIGURE OUTPUT <output-file> CONTENT <content> [...])
#Filesystem
file({GLOB | GLOB_RECURSE} <out-var> [...] <globbing-expr>...)
file(MAKE_DIRECTORY <directories>... [...])
file({REMOVE | REMOVE_RECURSE } <files>...)
file(RENAME <oldname> <newname> [...])
file(COPY_FILE <oldname> <newname> [...])
file({COPY | INSTALL} <file>... DESTINATION <dir> [...])
file(SIZE <filename> <out-var>)
file(READ_SYMLINK <linkname> <out-var>)
file(CREATE_LINK <original> <linkname> [...])
file(CHMOD <files>... <directories>... PERMISSIONS <permissions>... [...])
file(CHMOD_RECURSE <files>... <directories>... PERMISSIONS <permissions>... [...])
#Path Conversion
file(REAL_PATH <path> <out-var> [BASE_DIRECTORY <dir>] [EXPAND_TILDE])
file(RELATIVE_PATH <out-var> <directory> <file>)
file({TO_CMAKE_PATH | TO_NATIVE_PATH} <path> <out-var>)
#Transfer
file(DOWNLOAD <url> [<file>] [...])
file(UPLOAD <file> <url> [...])
#Locking
file(LOCK <path> [...])
#Archiving
file(ARCHIVE_CREATE OUTPUT <archive> PATHS <paths>... [...])
file(ARCHIVE_EXTRACT INPUT <archive> [...])
#Handling Runtime Binaries
file(GET_RUNTIME_DEPENDENCIES [...])

find_file (
          <VAR>
          name | NAMES name1 [name2 ...]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED|OPTIONAL]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

find_library (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED|OPTIONAL]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

find_package(<PackageName> [<version>] [REQUIRED] [COMPONENTS <components>...])

find_path (
          <VAR>
          name | NAMES name1 [name2 ...]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED|OPTIONAL]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

find_program (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]...]
          [PATHS [path | ENV var]...]
          [REGISTRY_VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [VALIDATOR function]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED|OPTIONAL]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [NO_CMAKE_INSTALL_PREFIX]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )

cmake_pkg_config(EXTRACT <package> [<version>]
                [REQUIRED] [EXACT] [QUIET]
                [SYSTEM_INCLUDE_DIRS <path>...]
                [SYSTEM_LIBRARY_DIRS <path>...]
                [ALLOW_SYSTEM_INCLUDES <bool>]
                [ALLOW_SYSTEM_LIBS <bool>]
                [STRICTNESS <mode>]
                [ENV_MODE <mode>]
                [PC_LIBDIR <path>...]
                [PC_PATH <path>...]
                [DISABLE_UNINSTALLED <bool>]
                [PC_SYSROOT_DIR <path>]
                [TOP_BUILD_DIR <path>])

cmake_pkg_config(POPULATE <package> [<version>]
                [REQUIRED] [EXACT] [QUIET]
                [PREFIX <prefix>]
                [BIND_PC_REQUIRES <<name>=<target>>...]
                [STRICTNESS <mode>]
                [ENV_MODE <mode>]
                [PC_LIBDIR <path>...]
                [PC_PATH <path>...]
                [DISABLE_UNINSTALLED <bool>]
                [PC_SYSROOT_DIR <path>]
                [TOP_BUILD_DIR <path>])

cmake_pkg_config(IMPORT <package> [<version>]
                [REQUIRED] [EXACT] [QUIET]
                [NAME <name>]
                [PREFIX <prefix>]
                [BIND_PC_REQUIRES <<name>=<target>>...]
                [STRICTNESS <mode>]
                [ENV_MODE <mode>]
                [PC_LIBDIR <path>...]
                [PC_PATH <path>...]
                [DISABLE_UNINSTALLED <bool>]
                [PC_SYSROOT_DIR <path>]
                [TOP_BUILD_DIR <path>])

cmake_host_system_information(RESULT <variable>
                              QUERY WINDOWS_REGISTRY <key> [VALUE_NAMES|SUBKEYS|VALUE <name>]
                              [VIEW (64|32|64_32|32_64|HOST|TARGET|BOTH)]
                              [SEPARATOR <separator>]
                              [ERROR_VARIABLE <result>])

cmake_language(CALL <command> [<arg>...])
cmake_language(EVAL CODE <code>...)
cmake_language(DEFER <options>... CALL <command> [<arg>...])
cmake_language(SET_DEPENDENCY_PROVIDER <command> SUPPORTED_METHODS <methods>...)
cmake_language(GET_MESSAGE_LOG_LEVEL <out-var>)
cmake_language(EXIT <exit-code>)
cmake_language(TRACE <boolean> ...)


cmake_parse_arguments(<prefix> <options> <one_value_keywords>
                      <multi_value_keywords> <args>...)

cmake_parse_arguments(PARSE_ARGV <N> <prefix> <options>
                      <one_value_keywords> <multi_value_keywords>)

#Decomposition
  cmake_path(GET <path-var> ROOT_NAME <out-var>)
  cmake_path(GET <path-var> ROOT_DIRECTORY <out-var>)
  cmake_path(GET <path-var> ROOT_PATH <out-var>)
  cmake_path(GET <path-var> FILENAME <out-var>)
  cmake_path(GET <path-var> EXTENSION [LAST_ONLY] <out-var>)
  cmake_path(GET <path-var> STEM [LAST_ONLY] <out-var>)
  cmake_path(GET <path-var> RELATIVE_PART <out-var>)
  cmake_path(GET <path-var> PARENT_PATH <out-var>)

#Query
  cmake_path(HAS_ROOT_NAME <path-var> <out-var>)
  cmake_path(HAS_ROOT_DIRECTORY <path-var> <out-var>)
  cmake_path(HAS_ROOT_PATH <path-var> <out-var>)
  cmake_path(HAS_FILENAME <path-var> <out-var>)
  cmake_path(HAS_EXTENSION <path-var> <out-var>)
  cmake_path(HAS_STEM <path-var> <out-var>)
  cmake_path(HAS_RELATIVE_PART <path-var> <out-var>)
  cmake_path(HAS_PARENT_PATH <path-var> <out-var>)
  cmake_path(IS_ABSOLUTE <path-var> <out-var>)
  cmake_path(IS_RELATIVE <path-var> <out-var>)
  cmake_path(IS_PREFIX <path-var> <input> [NORMALIZE] <out-var>)

#Comparison
  cmake_path(COMPARE <input1> <op> <input2> <out-var>)

#Modification
  cmake_path(SET <path-var> [NORMALIZE] <input>)
  cmake_path(APPEND <path-var> [<input>...] [OUTPUT_VARIABLE <out-var>])
  cmake_path(APPEND_STRING <path-var> [<input>...] [OUTPUT_VARIABLE <out-var>])
  cmake_path(REMOVE_FILENAME <path-var> [OUTPUT_VARIABLE <out-var>])
  cmake_path(REPLACE_FILENAME <path-var> <input> [OUTPUT_VARIABLE <out-var>])
  cmake_path(REMOVE_EXTENSION <path-var> [LAST_ONLY] [OUTPUT_VARIABLE <out-var>])
  cmake_path(REPLACE_EXTENSION <path-var> [LAST_ONLY] <input> [OUTPUT_VARIABLE <out-var>])

#Generation
  cmake_path(NORMAL_PATH <path-var> [OUTPUT_VARIABLE <out-var>])
  cmake_path(RELATIVE_PATH <path-var> [BASE_DIRECTORY <input>] [OUTPUT_VARIABLE <out-var>])
  cmake_path(ABSOLUTE_PATH <path-var> [BASE_DIRECTORY <input>] [NORMALIZE] [OUTPUT_VARIABLE <out-var>])

#Native Conversion
  cmake_path(NATIVE_PATH <path-var> [NORMALIZE] <out-var>)
  cmake_path(CONVERT <input> TO_CMAKE_PATH_LIST <out-var> [NORMALIZE])
  cmake_path(CONVERT <input> TO_NATIVE_PATH_LIST <out-var> [NORMALIZE])

#Hashing
  cmake_path(HASH <path-var> <out-var>)



configure_file(<input> <output>
               [NO_SOURCE_PERMISSIONS | USE_SOURCE_PERMISSIONS |
                FILE_PERMISSIONS <permissions>...]
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF]])

execute_process(COMMAND <cmd1> [<arguments>]
                [COMMAND <cmd2> [<arguments>]]...
                [WORKING_DIRECTORY <directory>]
                [TIMEOUT <seconds>]
                [RESULT_VARIABLE <variable>]
                [RESULTS_VARIABLE <variable>]
                [OUTPUT_VARIABLE <variable>]
                [ERROR_VARIABLE <variable>]
                [INPUT_FILE <file>]
                [OUTPUT_FILE <file>]
                [ERROR_FILE <file>]
                [OUTPUT_QUIET]
                [ERROR_QUIET]
                [COMMAND_ECHO <where>]
                [OUTPUT_STRIP_TRAILING_WHITESPACE]
                [ERROR_STRIP_TRAILING_WHITESPACE]
                [ENCODING <name>]
                [ECHO_OUTPUT_VARIABLE]
                [ECHO_ERROR_VARIABLE]
                [COMMAND_ERROR_IS_FATAL <ANY|LAST|NONE>])


get_cmake_property(<variable> <property>)
get_directory_property(<variable> [DIRECTORY <dir>] <prop-name>)
get_filename_component(<var> <FileName> <mode> [CACHE])
get_property(<variable>
             <GLOBAL             |
              DIRECTORY [<dir>]  |
              TARGET    <target> |
              SOURCE    <source>
                        [DIRECTORY <dir> | TARGET_DIRECTORY <target>] |
              INSTALL   <file>   |
              TEST      <test>
                        [DIRECTORY <dir>] |
              CACHE     <entry>  |
              VARIABLE           >
             PROPERTY <name>
             [SET | DEFINED | BRIEF_DOCS | FULL_DOCS])




load_cache(<build-dir> READ_WITH_PREFIX <prefix> <entry>...)

mark_as_advanced([CLEAR|FORCE] <var1> ...)

math(EXPR <variable> "<expression>" [OUTPUT_FORMAT <format>])




separate_arguments(<variable> <mode> [PROGRAM [SEPARATE_ARGS]] <args>)

set_directory_properties(PROPERTIES <prop1> <value1> [<prop2> <value2>] ...)

set_property(<GLOBAL                      |
              DIRECTORY [<dir>]           |
              TARGET    [<target1> ...]   |
              SOURCE    [<src1> ...]
                        [DIRECTORY <dirs> ...]
                        [TARGET_DIRECTORY <targets> ...] |
              INSTALL   [<file1> ...]     |
              TEST      [<test1> ...]
                        [DIRECTORY <dir>] |
              CACHE     [<entry1> ...]    >
             [APPEND] [APPEND_STRING]
             PROPERTY <name> [<value1> ...])

site_name(variable)

#Search and Replace
  string(FIND <string> <substring> <out-var> [...])
  string(REPLACE <match-string> <replace-string> <out-var> <input>...)
  string(REGEX MATCH <match-regex> <out-var> <input>...)
  string(REGEX MATCHALL <match-regex> <out-var> <input>...)
  string(REGEX REPLACE <match-regex> <replace-expr> <out-var> <input>...)

#Manipulation
  string(APPEND <string-var> [<input>...])
  string(PREPEND <string-var> [<input>...])
  string(CONCAT <out-var> [<input>...])
  string(JOIN <glue> <out-var> [<input>...])
  string(TOLOWER <string> <out-var>)
  string(TOUPPER <string> <out-var>)
  string(LENGTH <string> <out-var>)
  string(SUBSTRING <string> <begin> <length> <out-var>)
  string(STRIP <string> <out-var>)
  string(GENEX_STRIP <string> <out-var>)
  string(REPEAT <string> <count> <out-var>)
  string(REGEX QUOTE <out-var> <input>...)

#Comparison
  string(COMPARE <op> <string1> <string2> <out-var>)

#Hashing
  string(<HASH> <out-var> <input>)

#Generation
  string(ASCII <number>... <out-var>)
  string(HEX <string> <out-var>)
  string(CONFIGURE <string> <out-var> [...])
  string(MAKE_C_IDENTIFIER <string> <out-var>)
  string(RANDOM [<option>...] <out-var>)
  string(TIMESTAMP <out-var> [<format string>] [UTC])
  string(UUID <out-var> ...)

#JSON
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         {GET | TYPE | LENGTH | REMOVE}
         <json-string> <member|index> [<member|index> ...])
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         MEMBER <json-string>
         [<member|index> ...] <index>)
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         SET <json-string>
         <member|index> [<member|index> ...] <value>)
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         EQUAL <json-string1> <json-string2>)

variable_watch(<variable> [<command>])

aux_source_directory(<dir> <variable>)

build_command(<variable>
              [CONFIGURATION <config>]
              [PARALLEL_LEVEL <parallel>]
              [TARGET <target>]
              [PROJECT_NAME <projname>] # legacy, causes warning
             )

cmake_file_api(
  QUERY
  API_VERSION <version>
  [CODEMODEL <versions>...]
  [CACHE <versions>...]
  [CMAKEFILES <versions>...]
  [TOOLCHAINS <versions>...]
)
cmake_instrumentation(
  API_VERSION <version>
  DATA_VERSION <version>
  [HOOKS <hooks>...]
  [OPTIONS <options>...]
  [CALLBACK <callback>]
  [CUSTOM_CONTENT <name> <type> <content>]
)
create_test_sourcelist(<sourceListName> <driverName> <test>... <options>...)
define_property(<GLOBAL | DIRECTORY | TARGET | SOURCE |
                 TEST | VARIABLE | CACHED_VARIABLE>
                 PROPERTY <name> [INHERITED]
                 [BRIEF_DOCS <brief-doc> [docs...]]
                 [FULL_DOCS <full-doc> [docs...]]
                 [INITIALIZE_FROM_VARIABLE <variable>])
enable_language(<lang>... [OPTIONAL])

export(TARGETS <target>... [...])
export(EXPORT <export-name> [...])
export(PACKAGE <PackageName>)
export(SETUP <export-name> [...])

fltk_wrap_ui(resultingLibraryName source1
             source2 ... sourceN)

get_source_file_property(<variable> <file>
                         [DIRECTORY <dir> | TARGET_DIRECTORY <target>]
                         <property>)


get_test_property(<test> <property> [DIRECTORY <dir>] <variable>)

include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])

include_external_msproject(projectname location
                           [TYPE projectTypeGUID]
                           [GUID projectGUID]
                           [PLATFORM platformName]
                           dep1 dep2 ...)

include_regular_expression(regex_match [regex_complain])


link_directories([AFTER|BEFORE] directory1 [directory2 ...])

link_libraries([item1 [item2 [...]]]
               [[debug|optimized|general] <item>] ...)


remove_definitions(-DFOO -DBAR ...)

set_source_files_properties(<files> ...
                            [DIRECTORY <dirs> ...]
                            [TARGET_DIRECTORY <targets> ...]
                            PROPERTIES <prop1> <value1>
                            [<prop2> <value2>] ...)


set_tests_properties(<tests>...
                     [DIRECTORY <dir>]
                     PROPERTIES <prop1> <value1>
                     [<prop2> <value2>]...)

source_group(<name> [FILES <src>...] [REGULAR_EXPRESSION <regex>])
source_group(TREE <root> [PREFIX <prefix>] [FILES <src>...])

try_compile(<compileResultVar> PROJECT <projectName>
            SOURCE_DIR <srcdir>
            [BINARY_DIR <bindir>]
            [TARGET <targetName>]
            [LOG_DESCRIPTION <text>]
            [NO_CACHE]
            [NO_LOG]
            [CMAKE_FLAGS <flags>...]
            [OUTPUT_VARIABLE <var>])

try_run(<runResultVar> <compileResultVar>
        [SOURCES_TYPE <type>]
        <SOURCES <srcfile...>                 |
         SOURCE_FROM_CONTENT <name> <content> |
         SOURCE_FROM_VAR <name> <var>         |
         SOURCE_FROM_FILE <name> <path>       >...
        [LOG_DESCRIPTION <text>]
        [NO_CACHE]
        [NO_LOG]
        [CMAKE_FLAGS <flags>...]
        [COMPILE_DEFINITIONS <defs>...]
        [LINK_OPTIONS <options>...]
        [LINK_LIBRARIES <libs>...]
        [COMPILE_OUTPUT_VARIABLE <var>]
        [COPY_FILE <fileName> [COPY_FILE_ERROR <var>]]
        [<LANG>_STANDARD <std>]
        [<LANG>_STANDARD_REQUIRED <bool>]
        [<LANG>_EXTENSIONS <bool>]
        [RUN_OUTPUT_VARIABLE <var>]
        [RUN_OUTPUT_STDOUT_VARIABLE <var>]
        [RUN_OUTPUT_STDERR_VARIABLE <var>]
        [WORKING_DIRECTORY <var>]
        [ARGS <args>...]
        )

ctest_build([BUILD <build-dir>] [APPEND]
            [CONFIGURATION <config>]
            [PARALLEL_LEVEL <parallel>]
            [FLAGS <flags>]
            [PROJECT_NAME <project-name>]
            [TARGET <target-name>]
            [NUMBER_ERRORS <num-err-var>]
            [NUMBER_WARNINGS <num-warn-var>]
            [RETURN_VALUE <result-var>]
            [CAPTURE_CMAKE_ERROR <result-var>]
            )

ctest_configure([BUILD <build-dir>] [SOURCE <source-dir>] [APPEND]
                [OPTIONS <options>] [RETURN_VALUE <result-var>] [QUIET]
                [CAPTURE_CMAKE_ERROR <result-var>])

ctest_coverage([BUILD <build-dir>] [APPEND]
               [LABELS <label>...]
               [RETURN_VALUE <result-var>]
               [CAPTURE_CMAKE_ERROR <result-var>]
               [QUIET]
               )

ctest_empty_binary_directory(<directory>)

ctest_memcheck([BUILD <build-dir>] [APPEND]
               [START <start-number>]
               [END <end-number>]
               [STRIDE <stride-number>]
               [EXCLUDE <exclude-regex>]
               [INCLUDE <include-regex>]
               [EXCLUDE_LABEL <label-exclude-regex>]
               [INCLUDE_LABEL <label-include-regex>]
               [EXCLUDE_FIXTURE <regex>]
               [EXCLUDE_FIXTURE_SETUP <regex>]
               [EXCLUDE_FIXTURE_CLEANUP <regex>]
               [PARALLEL_LEVEL <level>]
               [RESOURCE_SPEC_FILE <file>]
               [TEST_LOAD <threshold>]
               [SCHEDULE_RANDOM <ON|OFF>]
               [STOP_ON_FAILURE]
               [STOP_TIME <time-of-day>]
               [RETURN_VALUE <result-var>]
               [CAPTURE_CMAKE_ERROR <result-var>]
               [REPEAT <mode>:<n>]
               [OUTPUT_JUNIT <file>]
               [DEFECT_COUNT <defect-count-var>]
               [QUIET]
               )

ctest_read_custom_files(<directory>...)

ctest_run_script([NEW_PROCESS] script_file_name script_file_name1
            script_file_name2 ... [RETURN_VALUE var])

ctest_sleep(<seconds>)

ctest_start(<model> [<source> [<binary>]] [GROUP <group>] [QUIET])

ctest_start([<model> [<source> [<binary>]]] [GROUP <group>] APPEND [QUIET])

ctest_submit([PARTS <part>...] [FILES <file>...]
             [SUBMIT_URL <url>]
             [BUILD_ID <result-var>]
             [HTTPHEADER <header>]
             [RETRY_COUNT <count>]
             [RETRY_DELAY <delay>]
             [RETURN_VALUE <result-var>]
             [CAPTURE_CMAKE_ERROR <result-var>]
             [QUIET]
             )

ctest_test([BUILD <build-dir>] [APPEND]
           [START <start-number>]
           [END <end-number>]
           [STRIDE <stride-number>]
           [EXCLUDE <exclude-regex>]
           [INCLUDE <include-regex>]
           [EXCLUDE_LABEL <label-exclude-regex>]
           [INCLUDE_LABEL <label-include-regex>]
           [EXCLUDE_FROM_FILE <filename>]
           [INCLUDE_FROM_FILE <filename>]
           [EXCLUDE_FIXTURE <regex>]
           [EXCLUDE_FIXTURE_SETUP <regex>]
           [EXCLUDE_FIXTURE_CLEANUP <regex>]
           [PARALLEL_LEVEL [<level>]]
           [RESOURCE_SPEC_FILE <file>]
           [TEST_LOAD <threshold>]
           [SCHEDULE_RANDOM <ON|OFF>]
           [STOP_ON_FAILURE]
           [STOP_TIME <time-of-day>]
           [RETURN_VALUE <result-var>]
           [CAPTURE_CMAKE_ERROR <result-var>]
           [REPEAT <mode>:<n>]
           [OUTPUT_JUNIT <file>]
           [QUIET]
           )

ctest_update([SOURCE <source-dir>]
             [RETURN_VALUE <result-var>]
             [CAPTURE_CMAKE_ERROR <result-var>]
             [QUIET])

ctest_upload(FILES <file>... [QUIET] [CAPTURE_CMAKE_ERROR <result-var>])

```




# cmake args

```shell
Usage

  cmake [options] <path-to-source>
  cmake [options] <path-to-existing-build>
  cmake [options] -S <path-to-source> -B <path-to-build>

Specify a source directory to (re-)generate a build system for it in the
current working directory.  Specify an existing build directory to
re-generate its build system.

Options
  -S <path-to-source>          = Explicitly specify a source directory.
  -B <path-to-build>           = Explicitly specify a build directory.
  -C <initial-cache>           = Pre-load a script to populate the cache.
  -D <var>[:<type>]=<value>    = Create or update a cmake cache entry.
  -U <globbing_expr>           = Remove matching entries from CMake cache.
  -G <generator-name>          = Specify a build system generator.
  -T <toolset-name>            = Specify toolset name if supported by
                                 generator.
  -A <platform-name>           = Specify platform name if supported by
                                 generator.
  --toolchain <file>           = Specify toolchain file
                                 [CMAKE_TOOLCHAIN_FILE].
  --install-prefix <directory> = Specify install directory
                                 [CMAKE_INSTALL_PREFIX].
  --project-file <project-file-name>
                               = Specify an alternate project file name.
  -W<category>                 = Enable the specified category of warnings.
  -Wno-<category>              = Suppress the specified category of warnings.
  -Werror=<category>           = Make the specified category of warnings
                                 errors.
  -Wno-error=<category>        = Make the specified category of warnings not
                                 errors.
  --preset <preset>,--preset=<preset>
                               = Specify a configure preset.
  --presets-file <file>,--presets-file=<file>
                               = Specify the path to a presets file.
  --list-presets[=<type>]      = List available presets.
  --workflow [<options>]       = Run a workflow preset.
  -E                           = CMake command mode.  Run "cmake -E" for a
                                 summary of commands.
  -L[A][H]                     = List non-advanced cached variables.
  -LR[A][H] <regex>            = Show cached variables that match the regex.
  --fresh                      = Configure a fresh build tree, removing any
                                 existing cache file.
  --build <dir>                = Build a CMake-generated project binary tree.
                                 Run "cmake --build" to see compatible
                                 options and a quick help.
  --install <dir>              = Install a CMake-generated project binary
                                 tree.  Run "cmake --install" to see
                                 compatible options and a quick help.
  --open <dir>                 = Open generated project in the associated
                                 application.
  -N                           = View mode only.
  -P <file>                    = Process script mode.
  --find-package               = Legacy pkg-config like mode.  Do not use.
  --graphviz=<file>            = Generate graphviz of dependencies, see
                                 CMakeGraphVizOptions.cmake for more.
  --system-information [file]  = Dump information about this system.
  --print-config-dir           = Print CMake config directory for user-wide
                                 FileAPI queries.
  --log-level=<ERROR|WARNING|NOTICE|STATUS|VERBOSE|DEBUG|TRACE>
                               = Set the verbosity of messages from CMake
                                 files.  --loglevel is also accepted for
                                 backward compatibility reasons.
  --log-context                = Prepend log messages with context, if given
  --debug-trycompile           = Do not delete the try_compile build tree.
                                 Only useful on one try_compile at a time.
  --debug-output               = Put cmake in a debug mode.
  --debug-find                 = Put cmake find in a debug mode.
  --debug-find-pkg=<pkg-name>[,...]
                               = Limit cmake debug-find to the
                                 comma-separated list of packages
  --debug-find-var=<var-name>[,...]
                               = Limit cmake debug-find to the
                                 comma-separated list of result variables
  --trace                      = Put cmake in trace mode.
  --trace-expand               = Put cmake in trace mode with variable
                                 expansion.
  --trace-format=<human|json-v1>
                               = Set the output format of the trace.
  --trace-source=<file>        = Trace only this CMake file/module.  Multiple
                                 options allowed.
  --trace-redirect=<file>      = Redirect trace output to a file instead of
                                 stderr.
  --check-system-vars          = Find problems with variable usage in system
                                 files.
  --compile-no-warning-as-error= Ignore COMPILE_WARNING_AS_ERROR property and
                                 CMAKE_COMPILE_WARNING_AS_ERROR variable.
  --link-no-warning-as-error   = Ignore LINK_WARNING_AS_ERROR property and
                                 CMAKE_LINK_WARNING_AS_ERROR variable.
  --profiling-format=<fmt>     = Output data for profiling CMake scripts.
                                 Supported formats: google-trace
  --profiling-output=<file>    = Select an output path for the profiling data
                                 enabled through --profiling-format.
  -h,-H,--help,-help,-usage,/? = Print usage information and exit.
  --version[=json-v1],-version[=json-v1],/V[=json-v1],/version[=json-v1] [<file>]
                               = Print version number and exit.
  --help <keyword> [<file>]    = Print help for one keyword and exit.
  --help-full [<file>]         = Print all help manuals and exit.
  --help-manual <man> [<file>] = Print one help manual and exit.
  --help-manual-list [<file>]  = List help manuals available and exit.
  --help-command <cmd> [<file>]= Print help for one command and exit.
  --help-command-list [<file>] = List commands with help available and exit.
  --help-commands [<file>]     = Print cmake-commands manual and exit.
  --help-diagnostic <diag> [<file>]
                               = Print help for one diagnostic and exit.
  --help-diagnostic-list [<file>]
                               = List diagnostics with help available and
                                 exit.
  --help-diagnostics [<file>]  = Print cmake-diagnostics manual and exit.
  --help-module <mod> [<file>] = Print help for one module and exit.
  --help-module-list [<file>]  = List modules with help available and exit.
  --help-modules [<file>]      = Print cmake-modules manual and exit.
  --help-policy <cmp> [<file>] = Print help for one policy and exit.
  --help-policy-list [<file>]  = List policies with help available and exit.
  --help-policies [<file>]     = Print cmake-policies manual and exit.
  --help-property <prop> [<file>]
                               = Print help for one property and exit.
  --help-property-list [<file>]= List properties with help available and
                                 exit.
  --help-properties [<file>]   = Print cmake-properties manual and exit.
  --help-variable var [<file>] = Print help for one variable and exit.
  --help-variable-list [<file>]= List variables with help available and exit.
  --help-variables [<file>]    = Print cmake-variables manual and exit.

Generators

The following generators are available on this platform (* marks default):
  Green Hills MULTI            = Generates Green Hills MULTI files
                                 (experimental, work-in-progress).
* Unix Makefiles               = Generates standard UNIX makefiles.
  Ninja                        = Generates build.ninja files.
  Ninja Multi-Config           = Generates build-<Config>.ninja files.
  FASTBuild                    = Generates fbuild.bff files.
  Watcom WMake                 = Generates Watcom WMake makefiles.
  CodeBlocks - Ninja           = Generates CodeBlocks project files
                                 (deprecated).
  CodeBlocks - Unix Makefiles  = Generates CodeBlocks project files
                                 (deprecated).
  CodeLite - Ninja             = Generates CodeLite project files
                                 (deprecated).
  CodeLite - Unix Makefiles    = Generates CodeLite project files
                                 (deprecated).
  Eclipse CDT4 - Ninja         = Generates Eclipse CDT 4.0 project files
                                 (deprecated).
  Eclipse CDT4 - Unix Makefiles= Generates Eclipse CDT 4.0 project files
                                 (deprecated).
  Kate - Ninja                 = Generates Kate project files (deprecated).
  Kate - Ninja Multi-Config    = Generates Kate project files (deprecated).
  Kate - Unix Makefiles        = Generates Kate project files (deprecated).
  Sublime Text 2 - Ninja       = Generates Sublime Text 2 project files
                                 (deprecated).
  Sublime Text 2 - Unix Makefiles
                               = Generates Sublime Text 2 project files
                                 (deprecated).

```


