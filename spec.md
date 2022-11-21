# XPath Next (draft)
It comes from "_XPath N extended_" and it stands for the XPath polyfills/shim project.

The idea is to have some dynamic mecanism to load some bunch of XPath extensions that are themselves written in XPath or XPath + host language

There might some dependencies on external dependencies like EXPath File or whatever and it needs to be clear

It can be loaded dynamically from some trusted source by the system

It shall use the next=https://xmlprague.cz/ns/next namespace

We can create any function we like as soon we can implement it with 
* level 1 : full XPath (depends on the XPath version then) and other fnext definitions : for example, fnext:is-odd(n) defined as n%2=1
* level 2 : everything in level 1 and a set of EXPath extensions : for example something working with EXPath File
* level 3 : everything in level 2 and some extra code dependant on the host language (XSLT, XQuery, XProc, etc.)
* level 4 : everything in level 3 and some extra code dependant on a generic agreed language (Javascript ?)
* level 5 : everything in level 4 and some extra code dependant on the language implementation of the product (JDK, DotNet, ERlang, etc.)  


It follows up from a discussion on slack of xml.com https://xmlcom.slack.com/archives/C01GVC3JLHE/p1669017333666309?thread_ts=1668720735.717849&cid=C01GVC3JLHE

