
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

```shell
# condition
在括号内,
一元测试如 COMMAND、POLICY、TARGET、TEST、EXISTS、
IS_READABLE、IS_WRITABLE、IS_EXECUTABLE、
IS_DIRECTORY、IS_SYMLINK、IS_ABSOLUTE 、 DEFINED。
二进制检验如EQUAL、LESS、LESS_EQUAL、GREATER、GREATER_EQUAL、
STREQUAL、STRLESS、STRLESS_EQUAL、STRGREATER、STRGREATER_EQUAL、
VERSION_EQUAL、VERSION_LESS、VERSION_LESS_EQUAL、VERSION_GREATER、
VERSION_GREATER_EQUAL、PATH_EQUAL、IN_LIST、IS_NEWER_THAN，MATCHES。
一元逻辑运算符NOT。
二元逻辑算符 AND 和 OR，从左到右， 没有短路。

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

# 函数
function(<name> [<arg1> ...])
  <commands>
endfunction()

# 值（总是串）,可用'$' [<scope>] '{' <identifier> '}'引用
# 列表以';'号分隔
# 普通变量用在当前及子目录,CACHE在所有cmake会话,ENV在外部环境

set(<variable> <value>... [PARENT_SCOPE])
set(CACHE{<variable>} [TYPE <type>] [HELP <helpstring>...] [FORCE] VALUE [<value>...])
set(ENV{<variable>} [<value>])

unset(<variable> [PARENT_SCOPE])
unset(CACHE{<variable>})
unset(ENV{<variable>})

# 块
block([SCOPE_FOR [POLICIES] [VARIABLES]] [PROPAGATE <var-name>...])
  <commands>
endblock()

```


# Commands

```shell
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

cmake_minimum_required(VERSION <min>[...<policy_max>] [FATAL_ERROR])

cmake_parse_arguments(<prefix> <options> <one_value_keywords>
                      <multi_value_keywords> <args>...)

cmake_parse_arguments(PARSE_ARGV <N> <prefix> <options>
                      <one_value_keywords> <multi_value_keywords>)

Decomposition
  cmake_path(GET <path-var> ROOT_NAME <out-var>)
  cmake_path(GET <path-var> ROOT_DIRECTORY <out-var>)
  cmake_path(GET <path-var> ROOT_PATH <out-var>)
  cmake_path(GET <path-var> FILENAME <out-var>)
  cmake_path(GET <path-var> EXTENSION [LAST_ONLY] <out-var>)
  cmake_path(GET <path-var> STEM [LAST_ONLY] <out-var>)
  cmake_path(GET <path-var> RELATIVE_PART <out-var>)
  cmake_path(GET <path-var> PARENT_PATH <out-var>)

Query
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

Comparison
  cmake_path(COMPARE <input1> <op> <input2> <out-var>)

Modification
  cmake_path(SET <path-var> [NORMALIZE] <input>)
  cmake_path(APPEND <path-var> [<input>...] [OUTPUT_VARIABLE <out-var>])
  cmake_path(APPEND_STRING <path-var> [<input>...] [OUTPUT_VARIABLE <out-var>])
  cmake_path(REMOVE_FILENAME <path-var> [OUTPUT_VARIABLE <out-var>])
  cmake_path(REPLACE_FILENAME <path-var> <input> [OUTPUT_VARIABLE <out-var>])
  cmake_path(REMOVE_EXTENSION <path-var> [LAST_ONLY] [OUTPUT_VARIABLE <out-var>])
  cmake_path(REPLACE_EXTENSION <path-var> [LAST_ONLY] <input> [OUTPUT_VARIABLE <out-var>])

Generation
  cmake_path(NORMAL_PATH <path-var> [OUTPUT_VARIABLE <out-var>])
  cmake_path(RELATIVE_PATH <path-var> [BASE_DIRECTORY <input>] [OUTPUT_VARIABLE <out-var>])
  cmake_path(ABSOLUTE_PATH <path-var> [BASE_DIRECTORY <input>] [NORMALIZE] [OUTPUT_VARIABLE <out-var>])

Native Conversion
  cmake_path(NATIVE_PATH <path-var> [NORMALIZE] <out-var>)
  cmake_path(CONVERT <input> TO_CMAKE_PATH_LIST <out-var> [NORMALIZE])
  cmake_path(CONVERT <input> TO_NATIVE_PATH_LIST <out-var> [NORMALIZE])

Hashing
  cmake_path(HASH <path-var> <out-var>)

cmake_pkg_config(EXTRACT <package> [<version>] [...])
cmake_pkg_config(POPULATE <package> [<version>] [...])
cmake_pkg_config(IMPORT <package> [<version>] [...])

cmake_policy(VERSION <min>[...<max>])

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

Reading
  file(READ <filename> <out-var> [...])
  file(STRINGS <filename> <out-var> [...])
  file(<HASH> <filename> <out-var>)
  file(TIMESTAMP <filename> <out-var> [...])

Writing
  file({WRITE | APPEND} <filename> <content>...)
  file({TOUCH | TOUCH_NOCREATE} <file>...)
  file(GENERATE OUTPUT <output-file> [...])
  file(CONFIGURE OUTPUT <output-file> CONTENT <content> [...])

Filesystem
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

Path Conversion
  file(REAL_PATH <path> <out-var> [BASE_DIRECTORY <dir>] [EXPAND_TILDE])
  file(RELATIVE_PATH <out-var> <directory> <file>)
  file({TO_CMAKE_PATH | TO_NATIVE_PATH} <path> <out-var>)

Transfer
  file(DOWNLOAD <url> [<file>] [...])
  file(UPLOAD <file> <url> [...])

Locking
  file(LOCK <path> [...])

Archiving
  file(ARCHIVE_CREATE OUTPUT <archive> PATHS <paths>... [...])
  file(ARCHIVE_EXTRACT INPUT <archive> [...])

Handling Runtime Binaries
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

include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>]
                      [NO_POLICY_SCOPE])
include_guard([DIRECTORY|GLOBAL])


Reading
  list(LENGTH <list> <out-var>)
  list(GET <list> <element index> [<index> ...] <out-var>)
  list(JOIN <list> <glue> <out-var>)
  list(SUBLIST <list> <begin> <length> <out-var>)

Search
  list(FIND <list> <value> <out-var>)

Modification
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

Ordering
  list(REVERSE <list>)
  list(SORT <list> [...])

load_cache(<build-dir> READ_WITH_PREFIX <prefix> <entry>...)

mark_as_advanced([CLEAR|FORCE] <var1> ...)

math(EXPR <variable> "<expression>" [OUTPUT_FORMAT <format>])

General messages
  message([<mode>] "message text" ...)

Reporting checks
  message(<checkState> "message text" ...)

Configure Log
  message(CONFIGURE_LOG <text>...)

option(<variable> "<help_text>" [value])

return([PROPAGATE <var-name>...])

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

Search and Replace
  string(FIND <string> <substring> <out-var> [...])
  string(REPLACE <match-string> <replace-string> <out-var> <input>...)
  string(REGEX MATCH <match-regex> <out-var> <input>...)
  string(REGEX MATCHALL <match-regex> <out-var> <input>...)
  string(REGEX REPLACE <match-regex> <replace-expr> <out-var> <input>...)

Manipulation
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

Comparison
  string(COMPARE <op> <string1> <string2> <out-var>)

Hashing
  string(<HASH> <out-var> <input>)

Generation
  string(ASCII <number>... <out-var>)
  string(HEX <string> <out-var>)
  string(CONFIGURE <string> <out-var> [...])
  string(MAKE_C_IDENTIFIER <string> <out-var>)
  string(RANDOM [<option>...] <out-var>)
  string(TIMESTAMP <out-var> [<format string>] [UTC])
  string(UUID <out-var> ...)

JSON
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

add_compile_definitions(<definition> ...)
add_compile_options(<option> ...)
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

add_definitions(-DFOO -DBAR ...)
add_dependencies(<target> <target-dependency>...)
add_executable(<name> <options>... <sources>...)
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)
add_link_options(<option> ...)
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL] [SYSTEM])
add_test(NAME <name> COMMAND <command> [<arg>...]
         [CONFIGURATIONS <config>...]
         [WORKING_DIRECTORY <dir>]
         [COMMAND_EXPAND_LISTS])
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
enable_testing()

export(TARGETS <target>... [...])
export(EXPORT <export-name> [...])
export(PACKAGE <PackageName>)
export(SETUP <export-name> [...])

fltk_wrap_ui(resultingLibraryName source1
             source2 ... sourceN)

get_source_file_property(<variable> <file>
                         [DIRECTORY <dir> | TARGET_DIRECTORY <target>]
                         <property>)

get_target_property(<variable> <target> <property>)

get_test_property(<test> <property> [DIRECTORY <dir>] <variable>)

include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])

include_external_msproject(projectname location
                           [TYPE projectTypeGUID]
                           [GUID projectGUID]
                           [PLATFORM platformName]
                           dep1 dep2 ...)

include_regular_expression(regex_match [regex_complain])

install(TARGETS <target>... [...])
install(IMPORTED_RUNTIME_ARTIFACTS <target>... [...])
install({FILES | PROGRAMS} <file>... [...])
install(DIRECTORY <dir>... [...])
install(SCRIPT <file> [...])
install(CODE <code> [...])
install(EXPORT <export-name> [...])
install(PACKAGE_INFO <package-name> [...])
install(RUNTIME_DEPENDENCY_SET <set-name> [...])

link_directories([AFTER|BEFORE] directory1 [directory2 ...])

link_libraries([item1 [item2 [...]]]
               [[debug|optimized|general] <item>] ...)

project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [COMPAT_VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [SPDX_LICENSE <license-string>]
        [DESCRIPTION <description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])

remove_definitions(-DFOO -DBAR ...)

set_source_files_properties(<files> ...
                            [DIRECTORY <dirs> ...]
                            [TARGET_DIRECTORY <targets> ...]
                            PROPERTIES <prop1> <value1>
                            [<prop2> <value2>] ...)

set_target_properties(<targets> ...
                      PROPERTIES <prop1> <value1>
                      [<prop2> <value2>] ...)

set_tests_properties(<tests>...
                     [DIRECTORY <dir>]
                     PROPERTIES <prop1> <value1>
                     [<prop2> <value2>]...)

source_group(<name> [FILES <src>...] [REGULAR_EXPRESSION <regex>])
source_group(TREE <root> [PREFIX <prefix>] [FILES <src>...]

target_compile_definitions(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_compile_features(<target> <PRIVATE|PUBLIC|INTERFACE> <feature> [...])

target_compile_options(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_link_directories(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_link_libraries(<target>
  {INTERFACE|PUBLIC|PRIVATE} <item>...
  [{INTERFACE|PUBLIC|PRIVATE} <item>...]...)

target_link_options(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

target_precompile_headers(<target>
  <INTERFACE|PUBLIC|PRIVATE> [header1...]
  [<INTERFACE|PUBLIC|PRIVATE> [header2...] ...])

target_sources(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])



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






