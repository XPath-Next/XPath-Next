# XPath Next (draft) : Extension for XPath Derived Languages

## TODO
* ADD the discussion around Atomization, Effective Boolean Value (EBV)

## Introduction
It comes from "_XPath N extended_" and it stands for the XPath polyfills/shim project.

The idea is to have some dynamic mecanism to load some bunch of XPath extensions that are themselves written in XPath completed by other extensions if needed.

There might some dependencies on external dependencies like EXPath File or whatever and it needs to be explicit.

It can be loaded dynamically from some trusted source by the system if we manage to make the mechanism sufficiently easy.

We can create any function we like as soon we can implement it with :
* level 0 : full XPath (depends on the XPath version then) : for example, _fnext:is-odd(n)_ defined as _$n mod 2=1_
* level 1 : everything in level 0 and other fnext definitions : for example, _fnext:is-even(n)_ defined as _not(fnext:is-odd($n))_
* level 2 : everything in level 1 and a set of EXPath extensions : for example is the size of file an odd number of bytes, using EXPath File
* level 3 : everything in level 2 and some extra code dependant on the host language (XSLT, XQuery, XProc, Schematron, etc.)
* level 4 : everything in level 3 and some extra code dependant on a generic agreed language (Javascript, Lua, Haxe ?)
* level 5 : everything in level 4 and some extra code dependant on the language implementation of the product (JDK, DotNet, ERlang, etc.) 

## Goals and non goals
To provide a way to allow people to add some extension **without** having to pay the cost of writing a spec.

Of course the spec of the building blocks are around, but that's not mandatory to make every snippet of code dependant on the existence of a SPEC.

Allow to extends XPath Derived Languages with some cross knowledge from one language to the other.

It should not replace EXPath or any other attemps to standardise way more complex extensions to XPath (things that can't be done from within XPath).

On the other hand, it should minimize the number of functions needed in any extension since every extension that can be done with a minimal complexe function can be proposed separately.

It can be seen as generalized (not only XQuery related) crowd sourced version of [FunctX](http://www.xqueryfunctions.com/xq/)

## Namespaces
The module defined by this document defines functions and errors in the namespace [https://xmlprague.cz/ns/fnext](https://xmlprague.cz/ns/fnext). In this document, the **fnext** prefix is bound to this namespace URI.
We will also use the following prefixes
* *xpath1* to reference last XPath 1.x Specification (https://www.w3.org/TR/xpath-10/)
* *xpath2* to reference last XPath 2.x Specification (https://www.w3.org/TR/xpath-20/)
* *xpath3* to reference last XPath 3.x Specification (https://www.w3.org/TR/xpath-3/)
* *xpath4* to reference last XPath 4.x Specification (https://qt4cg.org/specifications/xquery-40/xpath-40.html)

## Type definition 

Being able to define types and being able to reuse them is a key component 

```xml
<define-type 
   name = QName
   depends = NCName+
   as?= QName>
   result
</define-type>
```

## Function definition

Being able to define a function and reuse it

```xml
<define-function 
   name = QName
   depends = NCName+
   as?= QName>
  (param*, result)
</define-function>
```

```xml
<param 
    name = NCName 
    as?= QName />
```

```xml
<result 
    select? = text>
   any?
</result>
```


## Extension mechanism
In order for this to work in all XPath bounded language, we propose an extension attribute call **@extends-fn-with** with declare the namespace we want to extends 
This mecanism is not needed to use those extension, because people could just continue to use the direct naming extension mecanism.

## Examples 
### Examples of Level 0 
#### Function
Let's define a function that give **true** is a value evaluated as a number is odd
```xml
<define name="fnext:is-odd" depends="xpath1" xmlns:fnext="https://xmlprague.cz/ns/fnext">
  <param name="n"/>
  <result select="$n mod 2=1"/>
</define>
```
### Examples of Level 1
#### Function
Let's define a function that give **true** is a value evaluated as a number is even
```xml
<define name="fnext:is-even" depends="xpath1 fnext" xmlns:fnext="https://xmlprague.cz/ns/fnext">
  <param name="n"/>
  <result select="not(fnext:is-odd($n))"/>
</define>
```
### Examples of Level 2
#### Function
Let's define a function that give **true** if a file is of size odd.
```xml
<define name="fnext:is-file" depends="xpath1 fnext file" xmlns:fnext="https://xmlprague.cz/ns/fnext" xmlns:file="http://expath.org/ns/file">
  <param name="file"/>
  <result select="fnext:is-odd(file:size($file))"/>
</define>
```

## Illustration
### Where we are now
![Alt text](https://g.gravizo.com/source/custom_mark10?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)

### Where we are heading
![Alt text](https://g.gravizo.com/source/custom_mark11?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)

## Tools
We may need to have some tools to convert the define files into XSLT Function, XQuery functions or XProc functions, Schematron

## Notes
It follows up from a discussion on slack of xml.com https://xmlcom.slack.com/archives/C01GVC3JLHE/p1669017333666309?thread_ts=1668720735.717849&cid=C01GVC3JLHE

It is also based on the Lockett & Retter presentation https://www.evolvedbinary.com/publications/task-abstraction-for-xpdls_february-2019.pdf

Also it has been revived by discussion on issue https://github.com/qt4cg/qtspecs/issues/397

## Annexe
### Images in GraphViz
![Alt text](https://g.gravizo.com/source/custom_mark10?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)
ORIGINE

```
![Alt text](https://g.gravizo.com/source/custom_mark10?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)
<details> 
<summary></summary>
custom_mark10
  digraph G {
  subgraph schemas {
      node [fontname="Helvetica,Arial,sans-serif"; color=blue; fillcolor=lightgrey]
    node [shape=Msquare] "XML Schema";
    node [shape=Msquare] "JSON Schema";
    node [shape=Msquare] "Relax NG";
    node [shape=Msquare] "Schematron";
    node [shape=Msquare] "NVDL";
    label = "Schemas";
  }
  
   subgraph transformations {
      node [fontname="Helvetica,Arial,sans-serif"; color=pink;]
    node [shape=Mdiamond] XProc;
    node [shape=Mdiamond] XSLT;
    node [shape=Mdiamond] XQuery;
    label = "Transformations";
    fillcolor = blue;
  }

  "Relax NG" -> XProc
  "XML Schema" -> XSLT
 
  "XPath" ->  XSLT
  XPathFunctionsAndOperators -> XSLT
  EXPath -> XSLT
  XPath -> XQuery
  EXQuery -> XQuery
  EXPath -> XQuery
  XPathFunctionsAndOperators -> XQuery
  XPathFunctionsAndOperators -> XProc
  XPath -> XProc
  XPathFunctionsAndOperators -> Schematron
  XPath -> Schematron
  
  XSLT -> XProc
  XQuery -> XProc
  Schematron -> XProc
  "XML Schema" -> XProc
  "JSON Schema" -> XProc
  "Relax NG" -> XProc
  NVDL -> XProc 
    }
custom_mark10
</details>
```


![Alt text](https://g.gravizo.com/source/custom_mark11?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)

```
![Alt text](https://g.gravizo.com/source/custom_mark11?https%3A%2F%2Fraw.githubusercontent.com%2FXPath-Next%2FXPath-Next%2Ffirst-draft%2Fspec.md)
<details> 
<summary></summary>
custom_mark11
  digraph G {
  subgraph schemas {
      node [fontname="Helvetica,Arial,sans-serif"; color=blue; fillcolor=lightgrey]
    node [shape=Msquare] "XML Schema";
    node [shape=Msquare] "JSON Schema";
    node [shape=Msquare] "Relax NG";
    node [shape=Msquare] "Schematron";
    node [shape=Msquare] "NVDL";
    label = "Schemas";
  }
  
   subgraph transformations {
      node [fontname="Helvetica,Arial,sans-serif"; color=pink;]
    node [shape=Mdiamond] XProc;
    node [shape=Mdiamond] XSLT;
    node [shape=Mdiamond] XQuery;
    label = "Transformations";
    fillcolor = blue;
  }
  
    subgraph xpdl {
     node [fontname="Helvetica,Arial,sans-serif"; color=red;]
     node [shape=mcircle] XPathWithCustomizableTypesAndFunctions;
   }


  "Relax NG" -> XPathWithCustomizableTypesAndFunctions
  "XML Schema" -> XPathWithCustomizableTypesAndFunctions
  "JSON Schema" -> XPathWithCustomizableTypesAndFunctions
  "XPath" -> XPathWithCustomizableTypesAndFunctions
  XPathWithCustomizableTypesAndFunctions -> XSLT
  XPathFunctionsAndOperators -> XSLT
  EXPath -> XSLT
  XPathWithCustomizableTypesAndFunctions -> XQuery
  EXQuery -> XQuery
  EXPath -> XQuery
  XPathFunctionsAndOperators -> XQuery
  XPathFunctionsAndOperators -> XProc
  XPathWithCustomizableTypesAndFunctions -> XProc
  XPathFunctionsAndOperators -> Schematron
  XPathWithCustomizableTypesAndFunctions -> Schematron
  
  XSLT -> XProc
  XQuery -> XProc
  Schematron -> XProc
  "XML Schema" -> XProc
  "JSON Schema" -> XProc
  "Relax NG" -> XProc
  NVDL -> XProc 
    }
custom_mark11
</details>
```
