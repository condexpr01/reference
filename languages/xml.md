# XML(1.1)
>  extensible markup language

```ebnf
<document> ::= <prolog> <element> <Misc>* - <Char>* <RestrictedChar> <Char>*

<extParsedEnt>      ::= <TextDecl>? <content> - <Char>* <RestrictedChar> <Char>*
<Names>             ::= <Name> (#x20 <Name>)*
<extSubset>         ::= <TextDecl>? <extSubsetDecl>
<STag>              ::= '<' <Name> (<S> <Attribute>)* <S>? '>'
<ETag>              ::= '</' <Name> <S>? '>'

(***************************************)
<prolog>  ::= <XMLDecl> <Misc>* (<doctypedecl> <Misc>*)?
<element> ::= <EmptyElemTag>
<Misc>    ::= <Comment> | <PI> | <S>
/* any Unicode character, excluding the surrogate blocks, FFFE, and FFFF. */
<Char>    ::= [#x1-#xD7FF] 
			| [#xE000-#xFFFD]
			| [#x10000-#x10FFFF]
<RestrictedChar> ::= [#x1-#x8] | [#xB-#xC] | [#xE-#x1F] | [#x7F-#x84] | [#x86-#x9F]

<TextDecl>          ::= '<?xml' <VersionInfo>? <EncodingDecl> <S>? '?>'
<content>           ::= <CharData>? ((<element> | <Reference> | <CDSect> | <PI> | <Comment>) <CharData>?)*
<extSubsetDecl>     ::= ( <markupdecl> | <conditionalSect> | <DeclSep>)*

(***************************************)
<XMLDecl>       ::= '<?xml' <VersionInfo> <EncodingDecl>? <SDDecl>? <S>? '?>'
<doctypedecl>   ::= '<!DOCTYPE' <S> <Name> (<S> <ExternalID>)? <S>? ('[' <intSubset> ']' <S>?)? '>'
<EmptyElemTag>  ::= '<' <Name> (<S> <Attribute>)* <S>? '/>'
<Comment>       ::= '<!--' ((<Char> - '-') | ('-' (<Char> - '-')))* '-->'
<PI>            ::= '<?' <PITarget> (<S> (<Char>* - (<Char>* '?>' <Char>*)))? '?>'
<S>             ::= (#x20 | #x9 | #xD | #xA)+

<CDSect> ::= <CDStart> <CData> <CDEnd>
<CharData> ::= [^<&]* - ([^<&]* ']]>' [^<&]*)
<conditionalSect>   ::= <includeSect> | <ignoreSect>

(***************************************)
<VersionInfo>  ::= <S> 'version' <Eq> ("'" <VersionNum> "'" | '"' <VersionNum> '"')
<EncodingDecl> ::= <S> 'encoding' <Eq> ('"' <EncName> '"' | "'" <EncName> "'" )
<SDDecl>       ::= #x20+ 'standalone' <Eq> (("'" ('yes' | 'no') "'") | ('"' ('yes' | 'no') '"'))
<ExternalID>   ::= 'SYSTEM' <S> <SystemLiteral> | 'PUBLIC' <S> <PubidLiteral> <S> <SystemLiteral>
<intSubset>    ::= (<markupdecl> | <DeclSep>)*
<Name>         ::= <NameStartChar> (<NameChar>)*
<Attribute>    ::= <Name> <Eq> <AttValue>
<PITarget> ::= <Name> - (('X' | 'x') ('M' | 'm') ('L' | 'l'))

<CDStart>           ::= '<![' <CDATA> '['
<CDEnd>             ::= ']]>'
<CData>             ::= (<Char>* - (<Char>* ']]>' <Char>*))
<includeSect>       ::= '<![' <S>? 'INCLUDE' <S>? '[' <extSubsetDecl> ']]>'
<ignoreSect>        ::= '<![' <S>? 'IGNORE' <S>? '[' <ignoreSectContents>* ']]>'


(***************************************)
<VersionNum>        ::= '1.1'
<Eq>                ::= <S>? '=' <S>?
<EncName>           ::= [A-Za-z] ([A-Za-z0-9._] | '-')*
<SystemLiteral>     ::= ('"' [^"]* '"')
						| ("'" [^']* "'")
<PubidLiteral>      ::= '"' <PubidChar>* '"'
						| "'" (<PubidChar> - "'")* "'"
<markupdecl>        ::= <elementdecl> | <AttlistDecl> | <EntityDecl> | <NotationDecl> | <PI> | <Comment>
<DeclSep>           ::= <PEReference> | <S>

<NameStartChar>     ::= ":" | [A-Z] | "_" | [a-z] 
				| [#xC0-#xD6] | [#xD8-#xF6] | [#xF8-#x2FF]
				| [#x370-#x37D] | [#x37F-#x1FFF] | [#x200C-#x200D]
				| [#x2070-#x218F] | [#x2C00-#x2FEF] | [#x3001-#xD7FF]
				| [#xF900-#xFDCF] | [#xFDF0-#xFFFD] | [#x10000-#xEFFFF]
<NameChar>          ::= <NameStartChar> | "-" | "." | [0-9] | #xB7 | [#x0300-#x036F] | [#x203F-#x2040]
<AttValue>          ::= '"' ([^<&"] | <Reference>)* '"' | "'" ([^<&'] | <Reference>)* "'"

<ignoreSectContents>::= <Ignore> ('<![' <ignoreSectContents> ']]>' <Ignore>)*

(***************************************)
<PubidChar>         ::= #x20 | #xD | #xA | [a-zA-Z0-9] | [-'()+,./:=?;!*#@$_%]
<elementdecl>       ::= '<!ELEMENT' <S> <Name> <S> <contentspec> <S>? '>'
<AttlistDecl>       ::= '<!ATTLIST' <S> <Name> <AttDef>* <S>? '>'
<EntityDecl>        ::= <GEDecl> | <PEDecl>
<NotationDecl>      ::= '<!NOTATION' <S> <Name> <S> (<ExternalID> | <PublicID>) <S>? '>'
<PEReference>       ::= '%' <Name> ';'
<Reference>         ::= <EntityRef> | <CharRef>

<Ignore>            ::= <Char>* - (<Char>* ('<![' | ']]>') <Char>*)

(***************************************)
<contentspec>       ::= 'EMPTY' | 'ANY' | <Mixed> | <children>
<AttDef>            ::= <S> <Name> <S> <AttType> <S> <DefaultDecl>
<GEDecl>            ::= '<!ENTITY' <S> <Name> <S> <EntityDef> <S>? '>'
<PEDecl>            ::= '<!ENTITY' <S> '%' <S> <Name> <S> <PEDef> <S>? '>'
<PublicID>          ::= 'PUBLIC' <S> <PubidLiteral>
<EntityRef>         ::= '&' <Name> ';'
<CharRef>           ::= '&#' [0-9]+ ';' | '&#x' [0-9a-fA-F]+ ';'

(***************************************)
<Mixed>             ::= '(' <S>? '#PCDATA' (<S>? '|' <S>? <Name>)* <S>? ')*' | '(' <S>? '#PCDATA' <S>? ')'
<children>          ::= (<choice> | <seq>) ('?' | '*' | '+')?
<AttType>           ::= <StringType> | <TokenizedType> | <EnumeratedType>
<DefaultDecl>       ::= '#REQUIRED' | '#IMPLIED' | (('#FIXED' <S>)? <AttValue>)
<EntityDef>         ::= <EntityValue>| (<ExternalID> <NDataDecl>?)
<PEDef>             ::= <EntityValue> | <ExternalID>

(***************************************)
<StringType>        ::= 'CDATA'
<TokenizedType>     ::= 'ID' | 'IDREF' | 'IDREFS' | 'ENTITY' | 'ENTITIES' | 'NMTOKEN' | 'NMTOKENS'
<EnumeratedType>    ::= <NotationType> | <Enumeration>
<choice>            ::= '(' <S>? <cp> ( <S>? '|' <S>? <cp> )+ <S>? ')'
<seq>               ::= '(' <S>? <cp> ( <S>? ',' <S>? <cp> )* <S>? ')'

(***************************************)
<EntityValue>       ::= '"' ([^%&"] | <PEReference> | <Reference>)* '"'
						| "'" ([^%&'] | <PEReference> | <Reference>)* "'"
<NotationType>      ::= 'NOTATION' <S> '(' <S>? <Name> (<S>? '|' <S>? <Name>)* <S>? ')'
<NDataDecl>         ::= <S> 'NDATA' <S> <Name>
<cp>                ::= (<Name> | <choice> | <seq>) ('?' | '*' | '+')?
<Enumeration>       ::= '(' <S>? <Nmtoken> (<S>? '|' <S>? <Nmtoken>)* <S>? ')'

(***************************************)
<Nmtoken> ::= (<NameChar>)+
<Nmtokens> ::= <Nmtoken> (#x20 <Nmtoken>)*
```

