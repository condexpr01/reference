> 通常编译器有masm(machine masm)选项可以控制为att或者intel风格    

# AT&T asm

```ebnf
(* 左源 *)
(* op src, dst # dst = dst op src *)

(* (GNU Assembler / GAS) *)

<Program> ::= { <Line> }

(***************************************)
<Line> ::= [ <Label> ":" ] ( <Instruction> | <Directive> ) [ <Comment> ]

(***************************************)
<Label> ::= <Identifier>

(* 机器指令 *)
<Instruction> ::= <Mnemonic> [ <SizeSuffix> ] [ <OperandList> ]

(* 指导命令/as伪指令 *)
<Directive> ::= "." <Identifier> [ <OperandList> ]

<Comment> ::= "#" { <any_character> }

(***************************************)
<Identifier> ::= ( <Letter> | "_" | "." ) { <Letter> | <Digit> | "_" | "." }

(* 所有 x86 指令助记符 *)
<Mnemonic> ::= "mov" | "add" | "sub" | "cmp" | "jmp" | "call" | "ret"
			| "push" | "pop" | "lea" | "xor" | "and" | "or" | "shl" | "shr"
			| "inc" | "dec" | "imul" | "idiv" | "neg" | "not"
			| ...

(* b=字节, w=字(16位), l=双字(32位/64位浮点), q=四字(64位), s=单精度浮点, t=扩展精度浮点 *)
<SizeSuffix> ::= "b" | "w" | "l" | "q" | "s" | "t"

<OperandList> ::= <Operand> { "," <Operand> }

(***************************************)

<Letter> ::= "A" | "B" | ... | "Z" | "a" | "b" | ... | "z"
<Digit> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

<Operand> ::= <Immediate> | <Register> | <Memory>

(***************************************)

(* 立即数：$ 开头 *)
<Immediate> ::= "$" ( <Number> | <Identifier> | <Expression> )

(* 寄存器：% 开头 *)
<Register> ::= "%"  ( "rax" | "eax" | "ax" | "al"
					| "rbx" | "ebx" | "bx" | "bl"
					| "rcx" | "ecx" | "cx" | "cl"
					| "rdx" | "edx" | "dx" | "dl"
					| "rsi" | "esi" | "si" | "sil"
					| "rdi" | "edi" | "di" | "dil"
					| "rbp" | "ebp" | "bp" | "bpl"
					| "rsp" | "esp" | "sp" | "spl"
					| "rip" | "eip" | "ip"
					| "r8"  | "r8b"  | "r8w"  | "r8d"
					| ...   (* 直到 r15 *)
					| "st(0)" ... "st(7)"
					| "mm0" ... "mm7"
					| "xmm0" ... "xmm31"
					| "ymm0" ... "ymm31"
					| "zmm0" ... "zmm31"
					| "cr0" ... "cr15"
					| "dr0" ... "dr15" )

(* 内存地址 = Displacement + Base + Index * Scale *)
(* 除了lea这样的不解引用，通常操作内存地址会解引用 *)
<Memory> ::= [ <Segment> ":" ] [ <Displacement> ] ["(" [ <Base> ][ "," <Index> ][ "," <Scale> ]")"]

(***************************************)

<Segment> ::= <Identifier>   (* fs, gs, es, ds, ss *)
<Displacement> ::= <Number> | <Identifier> | <Expression>
<Base> ::= <Register>
<Index> ::= <Register>
<Scale> ::= "1" | "2" | "4" | "8"

(***************************************)
<Number> ::= [ "-" ] <Digit> { <Digit> }
			| "0x" <HexDigit> { <HexDigit> }
			| "0b" ( "0" | "1" ) { ( "0" | "1" ) }

<Expression> ::= <Term> { ( "+" | "-" | "*" | "/" ) <Term> }

<HexDigit> ::= <Digit> | "A" | "B" | "C" | "D" | "E" | "F"
						| "a" | "b" | "c" | "d" | "e" | "f"

(***************************************)
<Term> ::= <Number>
		| <Identifier>
		| <Identifier> "+" <Number>
		| <Identifier> "-" <Number>
		| "(" <Expression> ")"
```


# INTEL asm

```ebnf
(* 右源 *)
(* op dst, src # dst = dst op src *)

(* MASM (Microsoft Macro Assembler) *)

(* note: masm里有变量系统, `mov rax, var`和`mov rax, [var]`是等价的都会解引用 *)
(* note: 在masm里.data/.data?变量编译时取地址用的是mov rax, OFFSET var *)
(* note: 在masm里invoke 里用addr取地址(会实现为lea(load effective address)) *)
(* note: 在masm里取段地址用seg *)

<Program> ::= { <Line> }

(***************************************)

<Line> ::= [ <Label> [ ":" ] ] ( <Instruction> | <Directive> | <Macro> ) [ <Comment> ]
			| <Comment>

(***************************************)
<Label>              ::= <Identifier>

<Instruction>        ::= [ <Prefix> ] <Mnemonic> [ <OperandList> ]
<Directive>          ::= <DataDir> | <SegmentDir> | <ProcedureDir> | <ControlDir>
						| <AssumeDir> | <OptionDir> | <ConditionalDir> | <ListingDir>
						| <PrototypeDir>

<Macro>              ::= <MacroDef> | <MacroCall> | <MacroFunc>

<Comment>            ::= ";" { <any_character> }

(***************************************)
<Identifier> ::= ( <Alpha> | "@" | "_" | "$" | "?" ) { <Alpha> | <Digit> | "@" | "_" | "$" | "?" }

<Prefix> ::= "REP" | "REPE" | "REPZ" | "REPNE" | "REPNZ" | "LOCK" | "BOUND"
			| "WAIT" | <SegmentOverride>

<Mnemonic> ::= "mov" | "add" | "sub" | "cmp" | "jmp" | "je" | "call" | "ret"
			| "push" | "pop" | "lea" | "xor" | "and" | "or" | "shl" | "shr"
			| "inc" | "dec" | "imul" | "idiv" | "neg" | "not"
			| ...  (* 所有 x86/x64 指令助记符 *)

<OperandList> ::= <Operand> { "," <Operand> }

<DataDir> ::= "DB" <InitList>      (* 1 字节 *)
			| "DW" <InitList>      (* 2 字节 *)
			| "DD" <InitList>      (* 4 字节 *)
			| "DQ" <InitList>      (* 8 字节 *)
			| "DT" <InitList>      (* 10 字节 *)
			| "DF" <InitList>      (* 6 字节 *)
			| "REAL4" <InitList>   (* 4 字节浮点 *)
			| "REAL8" <InitList>   (* 8 字节浮点 *)
			| "REAL10" <InitList>  (* 10 字节浮点 *)

<SegmentDir> ::=  ".CODE" [ <Identifier> ]
				| ".DATA" [ <Identifier> ]
				| ".DATA?" (* 未初始化数据 *)
				| ".CONST" (* 常量数据 *)
				| ".STACK" [ <Expression> ]
				| <Segment> <SegmentAttrs> <SegmentBody> <SegmentEnd>

<ProcedureDir> ::= <ProcStart> <ProcBody> <ProcEnd>

<ControlDir>  ::=     ".IF" <Expression> <DirectiveList>
					{ ".ELSEIF" <Expression> <DirectiveList> }
					[ ".ELSE" <DirectiveList> ]
					  ".ENDIF"
				| ".WHILE" <Expression> <DirectiveList> ".ENDW"
				| ".REPEAT" <DirectiveList> ".UNTIL" <Expression>
				| ".REPEAT" <DirectiveList> ".UNTILCXZ" [ <Expression> ]
				| ".BREAK" [ ".IF" <Expression> ]
				| ".CONTINUE" [ ".IF" <Expression> ]

<AssumeDir> ::= "ASSUME" <AssumeList> ";;"
			| "ASSUME" "NOTHING" ";;"

<OptionDir> ::= "OPTION" <OptionList>

<ConditionalDir>  ::= "IF" <Expression> <DirectiveList>
					{ "ELSEIF" <Expression> <DirectiveList> }
					[ "ELSE" <DirectiveList> ]
					  "ENDIF"
				| "IFDEF" <Identifier> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"
				| "IFNDEF" <Identifier> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"
				| "IFB" <Arg> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"
				| "IFNB" <Arg> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"
				| "IFIDN" <Arg> "," <Arg> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"
				| "IFDIF" <Arg> "," <Arg> <DirectiveList> [ "ELSE" <DirectiveList> ] "ENDIF"

<ListingDir> ::=  ".LIST" | ".NOLIST" | ".XLIST" | ".LFCOND" | ".NOLFCOND"
				| ".TFCOND" | ".NOTFCOND" | ".CREF" | ".NOCREF" [ <Identifier> ]
				| ".LALL" | ".SALL" | ".XALL"

<PrototypeDir> ::= <Label> "PROTO" [ <Distance> ] [ <LangType> ] [ <ParamProtoList> ]

<MacroDef>           ::= <Identifier> "MACRO" [ <MacroParamList> ] <MacroBody> "ENDM"
<MacroCall>          ::= <Identifier> [ <ArgList> ]
<MacroFunc>          ::= <Identifier> "EXITM" [ <Expression> ]
									| "EXITM" [ <Expression> ]

(***************************************)
<Alpha> ::= "A" | "B" | ... | "Z" | "a" | "b" | ... | "z"
<Digit> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

<SegmentOverride> ::= "CS" | "DS" | "ES" | "SS" | "FS" | "GS"

<Operand> ::= <Register> | <Immediate> | <Memory> | <Variable>

<InitList> ::= <InitValue> { "," <InitValue> }

<Expression> ::= <Term> { ( "+" | "-" | "*" | "/" | "MOD" | "SHL" | "SHR" ) <Term> }

<Segment>            ::= "SEGMENT" | "SEG"
<SegmentAttrs>       ::= [ <AlignType> ] [ <CombineType> ] [ <Class> ]
<SegmentEnd>         ::= <Segment> "ENDS"
<SegmentBody>        ::= { <Line> }

<ProcStart>          ::= <Identifier> "PROC" [ <ProcAttrs> ] [ <LangType> ] [ <ParamList> ] [ <UsesList> ]

<ProcBody> ::= { <Line> }

<ProcEnd>            ::= <Identifier> "ENDP"

<DirectiveList>      ::= { <Line> }

<Arg>                ::= <Expression> | <String> | <Identifier>
<ArgList>            ::= <Arg> { "," <Arg> }

<MacroParamList>     ::= <MacroParam> { "," <MacroParam> }
<MacroBody>          ::= { <Line> }

<AssumeList>         ::= <AssumeRegister> { "," <AssumeRegister> }

<OptionList>         ::= <OptionItem> { "," <OptionItem> }

<Distance> ::= "NEAR" | "FAR"          (* 仅在 16 位模式下使用，用于覆盖默认调用距离 *)
<ParamProtoList> ::= "," <ParamProto> { "," <ParamProto> }

(***************************************)
<Register>::= "RAX" | "RBX" | "RCX" | "RDX" | "RSI" | "RDI" | "RBP" | "RSP" | "RIP"
			| "EAX" | "EBX" | "ECX" | "EDX" | "ESI" | "EDI" | "EBP" | "ESP" | "EIP"
			| "AX" | "BX" | "CX" | "DX" | "SI" | "DI" | "BP" | "SP"
			| "AL" | "AH" | "BL" | "BH" | "CL" | "CH" | "DL" | "DH"
			| "R8B" | "R8W" | "R8D" | "R8"  ...  "R15"
			| "ST(0)" ... "ST(7)"
			| "MM0" ... "MM7"
			| "XMM0" ... "XMM15"
			| "YMM0" ... "YMM15"

<Immediate>          ::= <Number> | <Character> | <String> | <Expression>
<Memory>             ::= [ <SegmentOverride> ] <MemoryOffset>
<Variable>           ::= <Identifier>
<InitValue>          ::= <Expression> | <String> | "?" | <DupSpec>

<Term> ::= <Number>
		| <Identifier>
		| "(" <Expression> ")"
		| <UnaryOp> <Term>

<AlignType>          ::= "BYTE" | "WORD" | "DWORD" | "PARA" | "PAGE"
<CombineType>        ::= "PUBLIC" | "PRIVATE" | "COMMON" | "STACK" | "MEMORY"
<Class>              ::= "'" <Identifier> "'"

<ProcAttrs>          ::= "NEAR" | "FAR" | "PRIVATE" | "PUBLIC" | "EXPORT"
<LangType>           ::= "C" | "SYSCALL" | "STDCALL" | "PASCAL" | "FORTRAN" | "BASIC"
<ParamList>          ::= <Param> { "," <Param> }
<UsesList>           ::= "USES" <Register> { <Register> }

<String>             ::= "'" { <any_character> } "'" | '"' { <any_character> } '"'

<MacroParam>         ::= <Identifier> [ ":" <QualifiedType> ] [ "=" <Expression> ]
					   | <Identifier> ":VARARG"

<AssumeRegister>     ::= <Register> ":" <AssumeVal>

<OptionItem>  ::= "CASEMAP" ":" ("NONE" | "NOTPUBLIC" | "ALL")
				| "DOTNAME" | "NODOTNAME"
				| "EMULATOR" | "NOEMULATOR"
				| "EPILOGUE" ":" <Identifier>
				| "EXPR16" | "EXPR32"
				| "LANGUAGE" ":" <LangType>
				| "M510" | "NOM510"
				| "NOKEYWORD" ":" <Identifier>
				| "NOSIGNEXTEND"
				| "OFFSET" ":" ("SEGMENT" | "GROUP")
				| "OLDMACROS" | "NOLDMACROS"
				| "OS_EXT" | "NOOS_EXT"
				| "PROLOGUE" ":" <Identifier>
				| "READONLY" | "NOREADONLY"
				| "SCOPED" | "NOSCOPED"
				| "SEGMENT" ":" <SegmentSize>
				| "WARN" ":" <WarnLevel>

(* proto配合使用invoke指令调用函数的检查 *)
<ParamProto> ::= [ <Identifier> ] ":" <QualifiedType>
			| [ <Identifier> ] ":" "VARARG"   (* 表示可变参数 *)

(***************************************)
<Number>             ::= [ <RadixOverride> ] <Digits>
<Character>          ::= "'" <any_character> "'"

<MemoryOffset>       ::= <DirectMemory> | <IndirectMemory>

<DupSpec>            ::= <Expression> "DUP" "(" <InitValue> { "," <InitValue> } ")"

<UnaryOp> ::= "+" | "-" | "NOT" | "HIGH" | "LOW" | "HIGHWORD" | "LOWWORD"
			| "TYPE" | "SIZE" | "LENGTH" | "OFFSET" | "SEG"

<Param>              ::= <Identifier> ":" <QualifiedType> [ "=" <Expression> ]

<QualifiedType>::= "BYTE" | "WORD" | "DWORD" | "QWORD" | "TBYTE" | "SBYTE" | "SWORD" | "SDWORD"
				 | "NEAR" | "FAR" | "PROC" | <Identifier>

<AssumeVal>          ::= <Identifier> | "NOTHING" | "ERROR"

<SegmentSize> ::= "USE16" | "USE32" | "USE64" | "FLAT"

<WarnLevel> ::= "1" | "2" | "3" | "4" | "5" | "ERROR" | "NONE"

(***************************************)
<RadixOverride>      ::= <Digit> "r"   (* 如 10r = 十进制 *)
						| "0x"          (* 十六进制前缀 *)
						| "0t"          (* 八进制前缀 *)
						| "0y"          (* 二进制前缀 *)

<Digits>             ::= <Digit> { <Digit> } [ <RadixSuffix> ]

<DirectMemory>       ::= "[" <Expression> "]"
						| <Identifier>            (* 变量名直接代表其地址 *)
						| <Number>

<IndirectMemory>  ::= "[" [ <Register> ] [ "+" ] [ <Register> ] [ "*" <Scale> ] [ "+" <Displacement> ] "]"
					| "[" <Register> "]"
					| "[" <Register> "+" <Displacement> "]"
					| "[" <Register> "+" <Register> "*" <Scale> "]"

(***************************************)

<RadixSuffix> ::= "h" | "H"    (* 十六进制 *)
				| "o" | "O"    (* 八进制 *)
				| "d" | "D"    (* 十进制 *)
				| "b" | "B"    (* 二进制 *)
				| "t" | "T"    (* 十进制（同 d） *)
				| "y" | "Y"    (* 二进制（同 b） *)

<Displacement>       ::= <Number> | <Identifier> | <Expression>
<Scale>              ::= 1 | 2 | 4 | 8
```
