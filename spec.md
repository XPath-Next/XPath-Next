# XPath Next (draft)
## Introduction
It comes from "_XPath N extended_" and it stands for the XPath polyfills/shim project.

The idea is to have some dynamic mecanism to load some bunch of XPath extensions that are themselves written in XPath or XPath + host language

There might some dependencies on external dependencies like EXPath File or whatever and it needs to be clear

It can be loaded dynamically from some trusted source by the system


We can create any function we like as soon we can implement it with 
* level 0 : full XPath (depends on the XPath version then) : for example, fnext:is-odd(n) defined as n%2=1
* level 1 : everything in level 0 and other fnext definitions : for example, fnext:is-even(n) defined as not(fnext:is-odd)
* level 2 : everything in level 1 and a set of EXPath extensions : for example is the size of file an odd number of bytes, using EXPath File
* level 3 : everything in level 2 and some extra code dependant on the host language (XSLT, XQuery, XProc, etc.)
* level 4 : everything in level 3 and some extra code dependant on a generic agreed language (Javascript ?)
* level 5 : everything in level 4 and some extra code dependant on the language implementation of the product (JDK, DotNet, ERlang, etc.) 

## Goals and non goals
To provide a way to allow people to add some extension **without** having to pay the cost of writing a spec
Of course the spec of the building blocks are around, but that's not mandatory to make every snippet of code dependant on the existence of a SPEC

Allow to extends XPath based languages with some cross knowledge from one language to the other

It should not replace EXPath or any other attemps to standardise way more complex extensions to XPath (things that can't be done from within XPath)
But it should minimize the number of functions needed
It can be seen as crowd sourced version of FunctX (add links)

## Namespaces
The module defined by this document defines functions and errors in the namespace [https://xmlprague.cz/ns/fnext](https://xmlprague.cz/ns/fnext). In this document, the **fnext** prefix is bound to this namespace URI.
We will also use the following prefixes
* *xpath1* to reference  last XPath 1.x Specification (https://www.w3.org/TR/xpath-10/)
* *xpath2* to reference  last XPath 2.x Specification (https://www.w3.org/TR/xpath-20/)
* *xpath3* to reference last XPath 3.x specification (https://www.w3.org/TR/xpath-3/)
* *xpath4* to reference last XPath 4.x specification (TODO link)

## Function definition

```xml
<define 
   name = QName
   depends = NCName+
   as?= QName>
  (param*)
  content
</define>
```

```xml
<param 
    name = NCName 
    as?= QName />
```


## Extension mechanism
In order for this to work in all XPath bounded language, we propose an extension attribute call **@extends-fn-with** with declare the namespace we want to extends 
This mecanism is not needed to use those extension, because people could just continue to use the direct naming extension mecanism 
## Examples 
### Examples of Level 0
Let's define a function that give **true** is a value evaluated as a number is odd
```xml
<define name="fnext:is-odd" depends="xpath1" xmlns:fnext="https://xmlprague.cz/ns/fnext">
  <param name="n"/>
  $n%2=1
</define>
```
### Examples of Level 1
Let's define a function that give **true** is a value evaluated as a number is even
```xml
<define name="fnext:is-even" depends="xpath1 fnext" xmlns:fnext="https://xmlprague.cz/ns/fnext">
  <param name="n"/>
  not(fnext:is-odd($n))
</define>
```

### Examples of Level 2
Let's define a function that give **true** if a file is of size odd.
```xml
<define name="fnext:is-file" depends="xpath1 fnext file" xmlns:fnext="https://xmlprague.cz/ns/fnext" xmlns:file="http://expath.org/ns/file">
  <param name="file"/>
  fnext:is-odd(file:size($file))
</define>
```
## Tools
We may need to have some tools to convert the define files into XSLT Function, XQuery functions or XProc functions

## Notes
It follows up from a discussion on slack of xml.com https://xmlcom.slack.com/archives/C01GVC3JLHE/p1669017333666309?thread_ts=1668720735.717849&cid=C01GVC3JLHE

