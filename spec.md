# XPath Next (draft)
## Introduction
It comes from "_XPath N extended_" and it stands for the XPath polyfills/shim project.

The idea is to have some dynamic mecanism to load some bunch of XPath extensions that are themselves written in XPath or XPath + host language

There might some dependencies on external dependencies like EXPath File or whatever and it needs to be clear

It can be loaded dynamically from some trusted source by the system


We can create any function we like as soon we can implement it with 
* level 1 : full XPath (depends on the XPath version then) and other fnext definitions : for example, fnext:is-odd(n) defined as n%2=1
* level 2 : everything in level 1 and a set of EXPath extensions : for example something working with EXPath File
* level 3 : everything in level 2 and some extra code dependant on the host language (XSLT, XQuery, XProc, etc.)
* level 4 : everything in level 3 and some extra code dependant on a generic agreed language (Javascript ?)
* level 5 : everything in level 4 and some extra code dependant on the language implementation of the product (JDK, DotNet, ERlang, etc.)  
## Namespaces
The module defined by this document defines functions and errors in the namespace [http://expath.org/ns/file](https://xmlprague.cz/ns/next). In this document, the **next** prefix is bound to this namespace URI.
We will also use the following prefixes
* *xpath1* to reference  last XPath 1.x Specification (TODO link)
* *xpath2* to reference  last XPath 2.x Specification (TODO link)
* *xpath3* to reference last XPath 3.x specification (TODO link)
* *xpath4* to reference last XPath 4.x specification (TODO link)

## Function definition

```xml
<define 
   name = NCName
   depends = NCName+>
  (param*)
  content
</define>
```

```xml
<param name = NCName />
```


## Extension mechanism
In order for this to work in all XPath bounded language, we propose an extension attribute call **@extends-fn-with** with declare the namespace we want to extends 
This mecanism is not needed to use those extension, because people could just continue to use the direct naming extension mecanism 
## Examples 
### Examples of Level 1
Let's define a function that give **true** is a value evaluated as a number if
```xml
<define name="is-odd" depends="xpath1" xmlns:next="https://xmlprague.cz/ns/next">
  <param name="n"/>
  $n%2=1
</define>
```

### Examples of Level 2
Let's define a function that give **true** if a file is of size odd.
```xml
<define name="is-file" depends="xpath1 file" xmlns:next="https://xmlprague.cz/ns/next" xmlns:file="http://expath.org/ns/file">
  <param name="file"/>
  file:size($file)%2=1
</define>
```

It follows up from a discussion on slack of xml.com https://xmlcom.slack.com/archives/C01GVC3JLHE/p1669017333666309?thread_ts=1668720735.717849&cid=C01GVC3JLHE
