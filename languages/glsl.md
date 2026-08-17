
# glsl4.6 in EBNF

```ebnf
<translation_unit> ::= <external_declaration>
	| <translation_unit> <external_declaration>

(***************************************)
<external_declaration> ::= <function_definition>
						| <declaration>
						| <SEMICOLON>

(***************************************)
<function_definition> ::= <function_prototype> <compound_statement_no_new_scope>

<declaration> ::= <function_prototype> <SEMICOLON>
				| <init_declarator_list> <SEMICOLON>

(***************************************)
<function_prototype> ::= <function_declarator> <RIGHT_PAREN>

<compound_statement_no_new_scope> ::= <LEFT_BRACE> <RIGHT_BRACE>
									| <LEFT_BRACE> <statement_list> <RIGHT_BRACE>

<init_declarator_list> ::= <single_declaration>
	| <init_declarator_list> <COMMA> <IDENTIFIER>
	| <init_declarator_list> <COMMA> <IDENTIFIER> <array_specifier>
	| <init_declarator_list> <COMMA> <IDENTIFIER> <array_specifier> <EQUAL> <initializer>
	| <init_declarator_list> <COMMA> <IDENTIFIER> <EQUAL> <initializer>

(***************************************)
<function_declarator> ::= <function_header>
	| <function_header_with_parameters>

<statement_list> ::= <statement>
	| <statement_list> <statement>

<single_declaration> ::= <fully_specified_type>
	| <fully_specified_type> <IDENTIFIER>
	| <fully_specified_type> <IDENTIFIER> <array_specifier>
	| <fully_specified_type> <IDENTIFIER> <array_specifier> <EQUAL> <initializer>
	| <fully_specified_type> <IDENTIFIER> <EQUAL> <initializer>

<array_specifier> ::= <LEFT_BRACKET> <RIGHT_BRACKET>
	| <LEFT_BRACKET> <conditional_expression> <RIGHT_BRACKET>
	| <array_specifier> <LEFT_BRACKET> <RIGHT_BRACKET>
	| <array_specifier> <LEFT_BRACKET> <conditional_expression> <RIGHT_BRACKET>

<initializer> ::= <assignment_expression>
	| <LEFT_BRACE> <initializer_list> <RIGHT_BRACE>
	| <LEFT_BRACE> <initializer_list> <COMMA> <RIGHT_BRACE>

(***************************************)
<function_header> ::= <fully_specified_type> <IDENTIFIER> <LEFT_PAREN>

<function_header_with_parameters> ::= <function_header> <parameter_declaration>
			| <function_header_with_parameters> <COMMA> <parameter_declaration>

<statement> ::= <compound_statement>
			| <simple_statement>

<fully_specified_type> ::= <type_specifier>
		| <type_qualifier> <type_specifier>

<conditional_expression> ::= <logical_or_expression>
	| <logical_or_expression> <QUESTION> <expression> <COLON> <assignment_expression>

<assignment_expression> ::= <conditional_expression>
	| <unary_expression> <assignment_operator> <assignment_expression>

<initializer_list> ::= <initializer>
	| <initializer_list> <COMMA> <initializer>

(***************************************)
<parameter_declaration> ::= <type_qualifier> <parameter_declarator>
						| <parameter_declarator>
						| <type_qualifier> <parameter_type_specifier>
						| <parameter_type_specifier>

<compound_statement> ::= <LEFT_BRACE> <RIGHT_BRACE>
					| <LEFT_BRACE> <statement_list> <RIGHT_BRACE>

<simple_statement> ::= <declaration_statement>
					| <expression_statement>
					| <selection_statement>
					| <switch_statement>
					| <case_label>
					| <iteration_statement>
					| <jump_statement>

<type_specifier> ::= <type_specifier_nonarray>
				| <type_specifier_nonarray> <array_specifier>

<type_qualifier> ::= <single_type_qualifier>
	| <type_qualifier> <single_type_qualifier>

<logical_or_expression> ::= <logical_xor_expression>
						| <logical_or_expression> <OR_OP> <logical_xor_expression>

<expression> ::= <assignment_expression>
	| <expression> <COMMA> <assignment_expression>


<assignment_operator> ::= <EQUAL>
						| <MUL_ASSIGN>
						| <DIV_ASSIGN>
						| <MOD_ASSIGN>
						| <ADD_ASSIGN>
						| <SUB_ASSIGN>
						| <LEFT_ASSIGN>
						| <RIGHT_ASSIGN>
						| <AND_ASSIGN>
						| <XOR_ASSIGN>
						| <OR_ASSIGN>

(***************************************)
<parameter_declarator> ::= <type_specifier> <IDENTIFIER>
						| <type_specifier> <IDENTIFIER> <array_specifier>

<parameter_type_specifier> ::= <type_specifier>

<declaration_statement> ::= <declaration>

<expression_statement> ::= <SEMICOLON>
			| <expression> <SEMICOLON>

<selection_statement> ::= <IF> <LEFT_PAREN> <expression> <RIGHT_PAREN> <selection_rest_statement>

<switch_statement> ::= <SWITCH> <LEFT_PAREN> <expression> <RIGHT_PAREN> <LEFT_BRACE> <switch_statement_list>
					| <RIGHT_BRACE>

<case_label> ::= <CASE> <expression> <COLON>
			| <DEFAULT> <COLON>

<iteration_statement> ::= <WHILE> <LEFT_PAREN> <condition> <RIGHT_PAREN> <statement_no_new_scope>
		| <DO> <statement> <WHILE> <LEFT_PAREN> <expression> <RIGHT_PAREN> <SEMICOLON>
		| <FOR> <LEFT_PAREN> <for_init_statement> <for_rest_statement> <RIGHT_PAREN> <statement_no_new_scope>

<jump_statement> ::= <CONTINUE> <SEMICOLON>
			| <BREAK> <SEMICOLON>
			| <RETURN> <SEMICOLON>
			| <RETURN> <expression> <SEMICOLON>
			| <DISCARD> <SEMICOLON>

<type_specifier_nonarray> ::= ? VOID
FLOAT DOUBLE
INT UINT
BOOL
VEC2 VEC3 VEC4
DVEC2 DVEC3 DVEC4 BVEC2 BVEC3 BVEC4 IVEC2 IVEC3 IVEC4 UVEC2 UVEC3 UVEC4
MAT2 MAT3 MAT4 MAT2X2 MAT2X3 MAT2X4 MAT3X2 MAT3X3 MAT3X4 MAT4X2 MAT4X3 MAT4X4
DMAT2 DMAT3 DMAT4
DMAT2X2 DMAT2X3 DMAT2X4 DMAT3X2 DMAT3X3 DMAT3X4 DMAT4X2 DMAT4X3 DMAT4X4
ATOMIC_UINT
SAMPLER2D SAMPLER3D SAMPLERCUBE SAMPLER2DSHADOW SAMPLERCUBESHADOW SAMPLER2DARRAY
SAMPLER2DARRAYSHADOW SAMPLERCUBEARRAY SAMPLERCUBEARRAYSHADOW
ISAMPLER2D ISAMPLER3D ISAMPLERCUBE ISAMPLER2DARRAY ISAMPLERCUBEARRAY
USAMPLER2D USAMPLER3D USAMPLERCUBE USAMPLER2DARRAY USAMPLERCUBEARRAY
SAMPLER1D SAMPLER1DSHADOW SAMPLER1DARRAY SAMPLER1DARRAYSHADOW
ISAMPLER1D ISAMPLER1DARRAY USAMPLER1D
USAMPLER1DARRAY SAMPLER2DRECT SAMPLER2DRECTSHADOW
ISAMPLER2DRECT USAMPLER2DRECT
SAMPLERBUFFER ISAMPLERBUFFER USAMPLERBUFFER
SAMPLER2DMS ISAMPLER2DMS USAMPLER2DMS
SAMPLER2DMSARRAY ISAMPLER2DMSARRAY USAMPLER2DMSARRAY
IMAGE2D IIMAGE2D UIMAGE2D IMAGE3D IIMAGE3D UIMAGE3D
IMAGECUBE IIMAGECUBE UIMAGECUBE
IMAGEBUFFER IIMAGEBUFFER UIMAGEBUFFER
IMAGE1D IIMAGE1D UIMAGE1D
IMAGE1DARRAY IIMAGE1DARRAY UIMAGE1DARRAY
IMAGE2DRECT IIMAGE2DRECT UIMAGE2DRECT
IMAGE2DARRAY IIMAGE2DARRAY UIMAGE2DARRAY IMAGECUBEARRAY IIMAGECUBEARRAY UIMAGECUBEARRAY
IMAGE2DMS IIMAGE2DMS UIMAGE2DMS
IMAGE2DMSARRAY IIMAGE2DMSARRAY UIMAGE2DMSARRAY
TYPE_NAME ? | <struct_specifier>

<single_type_qualifier> ::= <storage_qualifier>
						| <layout_qualifier>
						| <precision_qualifier>
						| <interpolation_qualifier>
						| <invariant_qualifier>
						| <precise_qualifier>

<logical_xor_expression> ::= <logical_and_expression>
	| <logical_xor_expression> <XOR_OP> <logical_and_expression>

<postfix_expression> ::= <primary_expression>
| <postfix_expression> <LEFT_BRACKET> <integer_expression> <RIGHT_BRACKET>
| <function_call>
| <postfix_expression> <DOT> <FIELD_SELECTION>
| <postfix_expression> <INC_OP>
| <postfix_expression> <DEC_OP>

<unary_expression> ::= <postfix_expression>
			| <INC_OP> <unary_expression>
			| <DEC_OP> <unary_expression>
			| <unary_operator> <unary_expression>

(***************************************)
<selection_rest_statement> ::= <statement> <ELSE> <statement>
							| <statement>

<switch_statement_list> ::= <statement_list>

<condition> ::= <expression> 
	| <fully_specified_type> <IDENTIFIER> <EQUAL> <initializer>

<statement_no_new_scope> ::= <compound_statement_no_new_scope> | <simple_statement>

<for_init_statement> ::= <expression_statement> | <declaration_statement>
<for_rest_statement> ::= <conditionopt> <SEMICOLON> | <conditionopt> <SEMICOLON> <expression>

<storage_qualifier> ::= ? CONST IN OUT INOUT CENTROID PATCH
SAMPLE UNIFORM BUFFER SHARED COHERENT VOLATILE RESTRICT
READONLY WRITEONLY SUBROUTINE SUBROUTINE LEFT_PAREN
 RIGHT_PAREN ? | <type_name_list>

<layout_qualifier> ::= <LAYOUT> <LEFT_PAREN> <layout_qualifier_id_list> <RIGHT_PAREN>

<precision_qualifier> ::= <HIGH_PRECISION>
						| <MEDIUM_PRECISION>
						| <LOW_PRECISION>

<interpolation_qualifier> ::= <SMOOTH>
							| <FLAT>
							| <NOPERSPECTIVE>

<invariant_qualifier> ::= <INVARIANT>

<precise_qualifier> ::= <PRECISE>

<logical_and_expression> ::= <inclusive_or_expression>
	| <logical_and_expression> <AND_OP> <inclusive_or_expression>

<primary_expression> ::= <variable_identifier>
					| <INTCONSTANT>
					| <UINTCONSTANT>
					| <FLOATCONSTANT>
					| <BOOLCONSTANT>
					| <DOUBLECONSTANT>
					| <LEFT_PAREN> <expression> <RIGHT_PAREN>

<integer_expression> ::= <expression>

<function_call> ::= <function_call_or_method>

<unary_operator> ::= <PLUS>
				| <DASH>
				| <BANG>
				| <TILDE>

<struct_specifier> ::= <STRUCT> <IDENTIFIER> <LEFT_BRACE> <struct_declaration_list> <RIGHT_BRACE>
					| <STRUCT> <LEFT_BRACE> <struct_declaration_list> <RIGHT_BRACE>

(***************************************)
<conditionopt> ::= <condition>

<layout_qualifier_id_list> ::= <layout_qualifier_id>
	| <layout_qualifier_id_list> <COMMA> <layout_qualifier_id>

<inclusive_or_expression> ::= <exclusive_or_expression>
	| <inclusive_or_expression> <VERTICAL_BAR> <exclusive_or_expression>

<variable_identifier> ::= <IDENTIFIER>

<function_call_or_method> ::= <function_call_generic>

<type_name_list> ::= <TYPE_NAME>
	| <type_name_list> <COMMA> <TYPE_NAME>

<struct_declaration_list> ::= <struct_declaration>
  | <struct_declaration_list> <struct_declaration>

(***************************************)
<layout_qualifier_id> ::= <IDENTIFIER>
	| <IDENTIFIER> <EQUAL> <constant_expression>
	| <SHARED>

<exclusive_or_expression> ::= <and_expression>
	| <exclusive_or_expression> <CARET> <and_expression>

<function_call_generic> ::= <function_call_header_with_parameters> <RIGHT_PAREN>
						| <function_call_header_no_parameters> <RIGHT_PAREN>

<struct_declaration> ::= <type_specifier> <struct_declarator_list> <SEMICOLON>
	| <type_qualifier> <type_specifier> <struct_declarator_list> <SEMICOLON>

(***************************************)
<constant_expression> ::= <conditional_expression>
						| <PRECISION> <precision_qualifier> <type_specifier> <SEMICOLON>
						| <type_qualifier> <IDENTIFIER> <LEFT_BRACE> <struct_declaration_list> <RIGHT_BRACE> <SEMICOLON>
						| <type_qualifier> <IDENTIFIER> <LEFT_BRACE> <struct_declaration_list> <RIGHT_BRACE> <IDENTIFIER>
						| <SEMICOLON>
						| <type_qualifier> <IDENTIFIER> <LEFT_BRACE> <struct_declaration_list> <RIGHT_BRACE> <IDENTIFIER>
						| <array_specifier> SEMICOLON
						| <type_qualifier> <SEMICOLON>
						| <type_qualifier> <IDENTIFIER> <SEMICOLON>
						| <type_qualifier> <IDENTIFIER> <identifier_list> <SEMICOLON>

<and_expression> ::= <equality_expression>
	| <and_expression> <AMPERSAND> <equality_expression>

<function_call_header_with_parameters> ::= <function_call_header> <assignment_expression>
	| <function_call_header_with_parameters> <COMMA> <assignment_expression>

<function_call_header_no_parameters> ::= <function_call_header> <VOID>
									| <function_call_header>

<struct_declarator_list> ::= <struct_declarator>
	| <struct_declarator_list> <COMMA> <struct_declarator>

(***************************************)
<identifier_list> ::= <COMMA> <IDENTIFIER>
  | <identifier_list> <COMMA> <IDENTIFIER>


<equality_expression> ::= <relational_expression>
	| <equality_expression> <EQ_OP> <relational_expression>
	| <equality_expression> <NE_OP> <relational_expression>

<function_call_header> ::= <function_identifier> <LEFT_PAREN>

<struct_declarator> ::= <IDENTIFIER>
					| <IDENTIFIER> <array_specifier>

(***************************************)

<relational_expression> ::= <shift_expression>
	| <relational_expression> <LEFT_ANGLE> <shift_expression>
	| <relational_expression> <RIGHT_ANGLE> <shift_expression>
	| <relational_expression> <LE_OP> <shift_expression>
	| <relational_expression> <GE_OP> <shift_expression>

<function_identifier> ::= <type_specifier>
						| <postfix_expression>

(***************************************)
<shift_expression> ::= <additive_expression>
	| <shift_expression> <LEFT_OP> <additive_expression>
	| <shift_expression> <RIGHT_OP> <additive_expression>

(***************************************)
<additive_expression> ::= <multiplicative_expression>
	| <additive_expression> <PLUS> <multiplicative_expression>
	| <additive_expression> <DASH> <multiplicative_expression>

(***************************************)
<multiplicative_expression> ::= <unary_expression>
	| <multiplicative_expression> <STAR> <unary_expression>
	| <multiplicative_expression> <SLASH> <unary_expression>
	| <multiplicative_expression> <PERCENT> <unary_expression>

```


# pipeline
```
(element array buffer, draw indirect buffer, vertex buffer object) ->
vertex puller ->
[vertex shader] ->
[tesselation control shader] ->
tesselation primitive Gen. ->
[tesselation eval. shader] ->
[geometry shader] ->
transform feedback -> (transform feedback buffer) ->
rasterization ->
[fragment shader] ->
per-fragment operations ->
framebuffer
```

# preprocessor

* directives

```glsl
#
#define
#undef
#if
#ifdef
#ifndef
#else
#elif
#endif
#error
#pragma
#extension
#version
#line
defined
##
```

* macros
```glsl
__FILE__
__LINE__
__VERSION__
```

* directives args

```glsl
#pragma optimimze(on)
#pragma optimimze(off)
#pragma debug(on)
#pragma debug(off)

//profile_opt: core, compatibility, es
#version <number> <profile_opt>

//behavior: require, enable, warn, disable
#extension <extension_name> : <behavior>
#extension all : <behavior>

#line <line>
#line <line> <source-string-number>
```

* build in macros

```glsl
GL_core_profile
GL_compatibility_profile
GL_es_profile

GL_SPIRV
VULKAN
```

# comment
```glsl
//comment
/*comment*/
```

# keywords

```ebnf
<keyword> ::=
	| "const"| "uniform"| "buffer"| "shared"| "attribute"| "varying"
	| "coherent"| "volatile"| "restrict"| "readonly"| "writeonly"
	| "atomic_uint"
	| "layout"
	| "centroid"| "flat"| "smooth"| "noperspective"
	| "patch"| "sample"
	| "invariant"| "precise"
	| "break"| "continue"| "do"| "for"| "while"| "switch"| "case"| "default"
	| "if"| "else"
	| "subroutine"
	| "in"| "out"| "inout"
	| "int"| "void"| "bool"| "true"| "false"| "float"| "double"
	| "discard"| "return"
	| "vec2"| "vec3"| "vec4"| "ivec2"| "ivec3"| "ivec4"| "bvec2"| "bvec3"| "bvec4"
	| "uint"| "uvec2"| "uvec3"| "uvec4"
	| "dvec2"| "dvec3"| "dvec4"
	| "mat2"| "mat3"| "mat4"
	| "mat2x2"| "mat2x3"| "mat2x4"
	| "mat3x2"| "mat3x3"| "mat3x4"
	| "mat4x2"| "mat4x3"| "mat4x4"
	| "dmat2"| "dmat3"| "dmat4"
	| "dmat2x2"| "dmat2x3"| "dmat2x4"
	| "dmat3x2"| "dmat3x3"| "dmat3x4"
	| "dmat4x2"| "dmat4x3"| "dmat4x4"
	| "lowp"| "mediump"| "highp"| "precision"
	| "sampler1D"| "sampler1DShadow"| "sampler1DArray"| "sampler1DArrayShadow"
	| "isampler1D"| "isampler1DArray"| "usampler1D"| "usampler1DArray"
	| "sampler2D"| "sampler2DShadow"| "sampler2DArray"| "sampler2DArrayShadow"
	| "isampler2D"| "isampler2DArray"| "usampler2D"| "usampler2DArray"
	| "sampler2DRect"| "sampler2DRectShadow"| "isampler2DRect"| "usampler2DRect"
	| "sampler2DMS"| "isampler2DMS"| "usampler2DMS"
	| "sampler2DMSArray"| "isampler2DMSArray"| "usampler2DMSArray"
	| "sampler3D"| "isampler3D"| "usampler3D"
	| "samplerCube"| "samplerCubeShadow"| "isamplerCube"| "usamplerCube"
	| "samplerCubeArray"| "samplerCubeArrayShadow"
	| "isamplerCubeArray"| "usamplerCubeArray"
	| "samplerBuffer"| "isamplerBuffer"| "usamplerBuffer"
	| "image1D"| "iimage1D"| "uimage1D"
	| "image1DArray"| "iimage1DArray"| "uimage1DArray"
	| "image2D"| "iimage2D"| "uimage2D"
	| "image2DArray"| "iimage2DArray"| "uimage2DArray"
	| "image2DRect"| "iimage2DRect"| "uimage2DRect"
	| "image2DMS"| "iimage2DMS"| "uimage2DMS"
	| "image2DMSArray"| "iimage2DMSArray"| "uimage2DMSArray"
	| "image3D"| "iimage3D"| "uimage3D"
	| "imageCube"| "iimageCube"| "uimageCube"
	| "imageCubeArray"| "iimageCubeArray"| "uimageCubeArray"
	| "imageBuffer"| "iimageBuffer"| "uimageBuffer"
	| "struct"

	(* when target vulkan *)
	(*
	| "texture1D"| "texture1DArray"
	| "itexture1D"| "itexture1DArray"| "utexture1D"| "utexture1DArray"
	| "texture2D"| "texture2DArray"
	| "itexture2D"| "itexture2DArray"| "utexture2D"| "utexture2DArray"
	| "texture2DRect"| "itexture2DRect"| "utexture2DRect"
	| "texture2DMS"| "itexture2DMS"| "utexture2DMS"
	| "texture2DMSArray"| "itexture2DMSArray"| "utexture2DMSArray"
	| "texture3D"| "itexture3D"| "utexture3D"
	| "textureCube"| "itextureCube"| "utextureCube"
	| "textureCubeArray"| "itextureCubeArray"| "utextureCubeArray"
	| "textureBuffer"| "itextureBuffer"| "utextureBuffer"
	| "sampler"| "samplerShadow"
	| "subpassInput"| "isubpassInput"| "usubpassInput"
	| "subpassInputMS"| "isubpassInputMS"| "usubpassInputMS"
	*)
	
	(* reserved for future *)
	(*
	| "common"| "partition"| "active"
	| "asm"
	| "class"| "union"| "enum"| "typedef"| "template"| "this"
	| "resource"
	| "goto"
	| "inline"| "noinline"| "public"| "static"| "extern"| "external"| "interface"
	| "long"| "short"| "half"| "fixed"| "unsigned"| "superp"
	| "input"| "output"
	| "hvec2"| "hvec3"| "hvec4"| "fvec2"| "fvec3"| "fvec4"
	| "filter"
	| "sizeof"| "cast"
	| "namespace"| "using"
	| "sampler3DRect"
	*)
```

# layout_qualifier_id

```glsl
//allowed interface: uniform/buffer
shared
packed
std140
std430
row_major
column_major
binding=
offset=
align=

//allowed interface: uniform/buffer(vulkan only)
set=

//allowed interface: uniform(vulkan only)
push_constant
input_attachment_index=

//allowed interface: uniform/buffer and subroutine variables
location=

//allowed interface: all in/out except for compute
location=
component=

//allowed interface: fragment out and subroutine functions
index=

//allowed interface: tessellation evaluation in
triangles
quads
isolines
equal_spacing
fractional_even_spacing
fractional_odd_spacing
cw
ccw
point_mode

//allowed interface: geometry in/out
points

//allowed interface: geometry in
lines
lines_adjacency
triangles
triangles_adjacency
invocations=

//allowed interface: fragment in
origin_upper_left
pixel_center_integer
early_fragment_test

//allowed interface: compute in
local_size_x_id=
local_size_y_id=
local_size_z_id=

//allowed interface: vertex, tessellation, geometry out
xfb_buffer=
xfb_stride=
xfb_offset=

//allowed interface: tessellation control out
vertices=

//allowed interface: geometry out
line_strip
triangle_strip
max_vertices=
stream=

//allowed interface: fragment out
depth_any
depth_greater
depth_less
depth_unchanged

//allowed interface: const (SPIR_V only)
constant_id=

//allowed interface: uniform
rgba32f
rgba16f
rg32f
rg16f
r11f_g11f_b10f
r32f
r16f
rgba16
rgb10_a2
rgba8
rg16
rg8
r16
r8
rgba16_snorm
rgba8_snorm
rg16_snorm
rg8_snorm
r16_snorm
r8_snorm
rgba32i
rgba16i
rgba8i
rg32i
rg16i
rg8i
r32i
r16i
r8i
rgba32ui
rgba16ui
rgb10_a2ui
rgba8ui
rg32ui
rg16ui
rg8ui
r32ui
r16ui
r8ui
```

# component name

```glsl
{x,y,z,w} //vertex coordinate
{r,g,b,a} //color
{s,t,p,q} //texture coordinate
```

# build-in variables
```glsl
//vertex shader special variables
in int gl_VertexID; // only present when not targeting Vulkan
in int gl_InstanceID; // only present when not targeting Vulkan
in int gl_VertexIndex; // only present when targeting Vulkan
in int gl_InstanceIndex; // only present when targeting Vulkan
in int gl_DrawID;
in int gl_BaseVertex;
in int gl_BaseInstance;
out gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
};

//tessellation control shader special varialbles
in gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
} gl_in[gl_MaxPatchVertices];
in int gl_PatchVerticesIn;
in int gl_PrimitiveID;
in int gl_InvocationID;
out gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
} gl_out[];
patch out float gl_TessLevelOuter[4];
patch out float gl_TessLevelInner[2];

//tessellation evaluation shader special variables
in gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
} gl_in[gl_MaxPatchVertices];
in int gl_PatchVerticesIn;
in int gl_PrimitiveID;
in vec3 gl_TessCoord;
patch in float gl_TessLevelOuter[4];
patch in float gl_TessLevelInner[2];
out gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
};

//geometry shader special variables
in gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
} gl_in[];
in int gl_PrimitiveIDIn;
in int gl_InvocationID;
out gl_PerVertex {
	vec4 gl_Position;
	float gl_PointSize;
	float gl_ClipDistance[];
	float gl_CullDistance[];
};
out int gl_PrimitiveID;
out int gl_Layer;
out int gl_ViewportIndex;

//fragment shader special variables
in vec4 gl_FragCoord;
in bool gl_FrontFacing;
in float gl_ClipDistance[];
in float gl_CullDistance[];
in vec2 gl_PointCoord;
in int gl_PrimitiveID;
in int gl_SampleID;
in vec2 gl_SamplePosition;
in int gl_SampleMaskIn[];
in int gl_Layer;
in int gl_ViewportIndex;
in bool gl_HelperInvocation;
out float gl_FragDepth;
out int gl_SampleMask[];

//compute shader special variables
// workgroup dimensions
in uvec3 gl_NumWorkGroups;
const uvec3 gl_WorkGroupSize;
// workgroup and invocation IDs
in uvec3 gl_WorkGroupID;
in uvec3 gl_LocalInvocationID;
// derived variables
in uvec3 gl_GlobalInvocationID;
in uint gl_LocalInvocationIndex;
```

# build-in constants

```glsl
const int gl_MaxVertexAttribs = 16;
const int gl_MaxVertexUniformVectors = 256;
const int gl_MaxVertexUniformComponents = 1024;
const int gl_MaxVertexOutputComponents = 64;
const int gl_MaxVaryingComponents = 60;
const int gl_MaxVaryingVectors = 15;
const int gl_MaxVertexTextureImageUnits = 16;
const int gl_MaxVertexImageUniforms = 0;
const int gl_MaxVertexAtomicCounters = 0;
const int gl_MaxVertexAtomicCounterBuffers = 0;
const int gl_MaxTessPatchComponents = 120;
const int gl_MaxPatchVertices = 32;
const int gl_MaxTessGenLevel = 64;
const int gl_MaxTessControlInputComponents = 128;
const int gl_MaxTessControlOutputComponents = 128;
const int gl_MaxTessControlTextureImageUnits = 16;
const int gl_MaxTessControlUniformComponents = 1024;
const int gl_MaxTessControlTotalOutputComponents = 4096;
const int gl_MaxTessControlImageUniforms = 0;
const int gl_MaxTessControlAtomicCounters = 0;
const int gl_MaxTessControlAtomicCounterBuffers = 0;
const int gl_MaxTessEvaluationInputComponents = 128;
const int gl_MaxTessEvaluationOutputComponents = 128;
const int gl_MaxTessEvaluationTextureImageUnits = 16;
const int gl_MaxTessEvaluationUniformComponents = 1024;
const int gl_MaxTessEvaluationImageUniforms = 0;
const int gl_MaxTessEvaluationAtomicCounters = 0;
const int gl_MaxTessEvaluationAtomicCounterBuffers = 0;
const int gl_MaxGeometryInputComponents = 64;
const int gl_MaxGeometryOutputComponents = 128;
const int gl_MaxGeometryImageUniforms = 0;
const int gl_MaxGeometryTextureImageUnits = 16;
const int gl_MaxGeometryOutputVertices = 256;
const int gl_MaxGeometryTotalOutputComponents = 1024;
const int gl_MaxGeometryUniformComponents = 1024;
const int gl_MaxGeometryVaryingComponents = 64; // deprecated
const int gl_MaxGeometryAtomicCounters = 0;
const int gl_MaxGeometryAtomicCounterBuffers = 0;
const int gl_MaxFragmentImageUniforms = 8;
const int gl_MaxFragmentInputComponents = 128;
const int gl_MaxFragmentUniformVectors = 256;
const int gl_MaxFragmentUniformComponents = 1024;
const int gl_MaxFragmentAtomicCounters = 8;
const int gl_MaxFragmentAtomicCounterBuffers = 1;
const int gl_MaxDrawBuffers = 8;
const int gl_MaxTextureImageUnits = 16;
const int gl_MinProgramTexelOffset = -8;
const int gl_MaxProgramTexelOffset = 7;
const int gl_MaxImageUnits = 8;
const int gl_MaxSamples = 4;
const int gl_MaxImageSamples = 0;
const int gl_MaxClipDistances = 8;
const int gl_MaxCullDistances = 8;
const int gl_MaxViewports = 16;
const int gl_MaxComputeImageUniforms = 8;
const ivec3 gl_MaxComputeWorkGroupCount = { 65535, 65535, 65535 };
const ivec3 gl_MaxComputeWorkGroupSize = { 1024, 1024, 64 };
const int gl_MaxComputeUniformComponents = 1024;
const int gl_MaxComputeTextureImageUnits = 16;
const int gl_MaxComputeAtomicCounters = 8;
const int gl_MaxComputeAtomicCounterBuffers = 8;
const int gl_MaxCombinedTextureImageUnits = 96;
const int gl_MaxCombinedImageUniforms = 48;
const int gl_MaxCombinedImageUnitsAndFragmentOutputs = 8; // deprecated
const int gl_MaxCombinedShaderOutputResources = 16;
const int gl_MaxCombinedAtomicCounters = 8;
const int gl_MaxCombinedAtomicCounterBuffers = 1;
const int gl_MaxCombinedClipAndCullDistances = 8;
const int gl_MaxAtomicCounterBindings = 1;
const int gl_MaxAtomicCounterBufferSize = 32;
const int gl_MaxTransformFeedbackBuffers = 4;
const int gl_MaxTransformFeedbackInterleavedComponents = 64;
const highp int gl_MaxInputAttachments = 1; // only present when targeting Vulkan

```

# build-in uniform state
```glsl
struct gl_DepthRangeParameters {
  float near; // n
  float far; // f
  float diff; // f - n
};
uniform gl_DepthRangeParameters gl_DepthRange;
uniform int gl_NumSamples;
```

# build-in functions
```glsl

radians(degrees) 
degrees(radians)
sin(angle)
cos(angle)
tan(angle)
asin(x)
acos(x)
atan(y,x)
atan(y_over_x)
sinh(x) //(exp(x)-exp(-x))/2, sinh(ix)=i*sin(x)
cosh(x) //(exp(x)+exp(-x))/2, cosh(ix)=cos(x)
tanh(x)
asinh(x)
acosh(x)
atanh(x)

pow(x,y)      //exp2(y*log2(x)) = x^y
exp(x)        //e^x
log(x)        //log_e(x)
exp2(x)       //2^x
log2(x)       //log_2(x)
inversesqrt(x)//2ULP(Unit of Least precision)
sqrt(x)       //1.0/inversesqrt() = sqrt()

abs(x)        // |x|
sign(x)       // if(x>0)return 1;if(x==0)return 0;if(x<0)return -1;
floor(x)      // round down
trunc(x)      // truncate the fractional part
round(x)      // the neareast integer to x, 0.5 direction by impl.
roundEven(x)  // the neareast integer to x, 0.5 direction toward the even integer
ceil(x)       // that the neareast integer is greater than or equal to x.
fract(x)      // x - floor(x)
mod(x,y)      // x - y*floor(x/y)
modf(x,i)     // x = fract(x), i=trunc(x)
min(x,y)      //minimal
max(x,y)      //maximal
clamp(x,minval,maxval)  //min(max(x,minval),maxval)
mix(x,y,a)    // x*(1-a)+y*a = a*(y-x)+x=fma(a,y-x,x)
step(edge,x)  // if(edge>x)return 0.0;else return 1.0
smoothstep(edge0,edge1,x) //clamp((x-edge0)/(edge1-edge0),0,1);
isnan(x)      //is not a number
isinf(x)      //is infinite
floatBitsToInt(value)
floatBitsToUint(value)
intBitsToFloat(value)
uintBitsToFloat(value)

fma(a,b,c)         //a*b+c
frexp(x,exp)       //significant in [0.5,1.0), x = significant * exp2(exp), write exp and return significant
ldexp(significant,exp) //return significant * exp2(exp)


packUnorm2x16(v)
packSnorm2x16(v)
packUnorm4x8(v)
packSnorm4x8(v)
unpackUnorm2x16(p)
unpackSnorm2x16(p)
unpackUnorm4x8(p)
unpackSnorm4x8(p)

packHalf2x16(v)
unpackHalf2x16(p)

packDouble2x32(v)
unpackDouble2x32(p)

length(x)       //sqrt(x_0^2+x_1^2+...)
distance(p0,p1) //length(p0-p1)
dot(x,y)
cross(x,y)
normalize(x)   // x/length(x)
faceforward(N,I,Nref) // if(dot(I,Nref)<0)return N;else return -N;
reflect(I,N)      // I-2*dot(I,N)*N
refract(I,N,eta)  //k=1.0-eta*eta*(1.0-dot(N,I)*dot(N,I)), if(k<0.0)return 0.0;else reuturn eta*I-(eta*dot(N,I)+sqrt(k))*N

matrixCompMult(x,y) //ret[i][j] = x[i][j] * y[i][j]
outerProduct(c,r)   //ret[j][j] = c[i] * r[j]
transpose(m)        //ret[i][j] = m[j][j]
inverse(m)          //m^{-1}
determinant(m)

lessThan(x,y)         // component-wise, x<y
lessThanEqual(x,y)    // component-wise, x<=y
greaterThan(x,y)      // component-wise, x>y
greaterThanEqual(x,y) // component-wise, x>=y
equal(x,y)            // component-wise, x==y
notEqual(x,y)         // component-wise, x!=y
any(x)                // if any component of x is true
all(x)                // if all component of x is true
not(x)                // logical complement of x

uaddCarry(x,y,carry)
usubBorrow(x,y,borrow)
umulExtended(x,y,msb,lsb)
imulExtended(x,y,msb,lsb)
bitfieldExtract(value,offset,bits)
bitfieldInsert(base,insert,offset,bits)
bitfieldReverse(value)
bitCount(value)
findLSB(value)

textureSize(sampler,lod)
textureQueryLod(sampler,P)
textureQueryLevels(sampler)
textureSamples(sampler)
texture(sampler,P[,bias])
textureProj(sampler,P[,bias])
textureLod(sampler,P,lod)
textureOffset(sampler,P,offset[,bias])
texelFetch(sampler,P,lod)
texelFetchOffset(sampler,P,lod,offset)
textureProjOffset(sampler,P,offset[,bias])
textureLodOffset(sampler,P,lod,offset)
textureProjLod(sampler,P,lod)
textureProjLodOffset(sampler,P,lod,offset)
textureGrad(sampler,P,dPdx,dPdy)
textureGradOffset(sampler,P,dPdx,dPdy,offset)
textureProjGrad(sampler,P,dPdx,dPdy)
textureProjGradOffset(sampler,P,dPdx,dPdy,offset)
textureGrather(sampler,P[,comp])
textureGratherOffsets(sampler,P,offset[,comp])

atomicCounterIncrement(c)
atomicCounterDecrement(c)
atomicCounter(c)
atomicCounterAdd(c,data)
atomicCounterSubtract(c,data)
atomicCounterMin(c,data)
atomicCounterMax(c,data)
atomicCounterAnd(c,data)
atomicCounterOr(c,data)
atomicCounterXor(c,data)
atomicCounterExchange(c,data)
atomicCounterCompSwap(c,data)

atomicAdd(mem,data)
atomicMin(mem,data)
atomicMax(mem,data)
atomicAnd(mem,data)
atomicOr(mem,data)
atomicXor(mem,data)
atomicExchange(mem,data)
atomicCompSwap(mem,data)

imageSize(image)
imageSamples(image)
imageLoad(IMAGE_PARAMS)
imageStore(IMAGE_PARAMS,data)
imageAtomicAdd(IMAGE_PARAMS,data)
imageAtomicMin(IMAGE_PARAMS,data)
imageAtomicMax(IMAGE_PARAMS,data)
imageAtomicAnd(IMAGE_PARAMS,data)
imageAtomicOr(IMAGE_PARAMS,data)
imageAtomicXor(IMAGE_PARAMS,data)
imageAtomicExchange(IMAGE_PARAMS,compare,data)
imageAtomicCompSwap(IMAGE_PARAMS,compare,data)

//geometry shader functions
EmitStreamVertex(stream)
EmitStreamPrimitive(stream)
EmitVertex()
EndPrimitive()

//fragment processing functions
dFdx(p)
dFdy(p)
dFdxFine(p)
dFdyFine(p)
dFdxCoarse(p)
dFdyCoarse(p)
fwidth(p)
fwidthFine(p)
fwidthCoarse(p)

interpolateAtCentroid(interpolant)
interpolateAtSample(interpolant, sample)
interpolateAtOffset(interpolant, offset)

//Noise
noise1(x)
noise2(x)
noise3(x)
noise4(x)

//mem
barrier()
memoryBarrier()
memoryBarrierAtomicCounter()
memoryBarrierBuffer()
memoryBarrierShared()
memoryBarrierImage()
groupMemoryBarrier()

subpassLoad(subpass[,sample])

anyInvocation(value)
allInvocations(value)
allInvocationsEqual(value)
```



