---
id: calls
title: Calls
---

This is a collection of traversal steps that are associated with call-sites in a graph and the relationships with methods. These allow us to identify and list all the call-sites, their incoming and outgoing edges, methods linked to those call sites, methods that call those call-sites etc. The call traversals can be combined with other steps such as filters to pinpoint specific cases during analysis. For example, it is possible to quickly identify how many times a given method has been called or if it is called recursively. We can also use it to find locations of vulnerable functions in the complete codebase and understand if they are called under certain conditions (if/else blocks) and are called from within certain functions. For example, we can answer the follwing question with a combination of Method and Call traversals:

> _Identify if a sanitization function is called inside a HTTP route handler which has the HTTP request as one of its arguments_

It is important to note that arithmetic and logic operations (`+`, `-`, `*`, `/`, `>`, `<`, `=`, `!=`) are also call-sites themselves and are named as `<operator>` since these operators are identified as methods in the graph. For example, the following statement has 3 call-sites:

```c
x = a + foo(b);
```
Here the callsites are:
 - `<operator>.addition` with arguments as `a` and `foo(b)`
 - `<operator>.assignment` with arguments as `x` and return of `a + foo(b)`
 - `foo` with argument `b`

In a similar manner, functional programming constructs such as closures, arrow functions (in Javascript) and lambdas also have their own call-site representations. For example, in Javascript, an arrow function called within a function is appended with a `:=>` in its name and hence can be searched similarly. Here is a CPGQL query that finds a 2nd level arrow function within in a Javascript function having `foo` in its name and then lists it call-sites:

```scala
joern> cpg.method.name(".*foo.*::=>:=>").callOut.code.l
```
 
### Traversal Steps

| Traversals | Description | Example |
|---|---|---|
| `.call`  | All call-sites in the code |  `cpg.call.name.l` |
| `.callOut` | Return the outgoing call-sites for a given method | `cpg.method.name("main").callOut.name.l` |
| `.callIn` | Return the call-sites of a given method | `cpg.method.name("exit").callIn.code.l` |

### Common Usage Patterns

{{< hint info >}}
Use `cpg.call.<TAB>` to explore more available options
{{< /hint >}}

| Name | Usage |
|--|--|
| Code string | `cpg.call.code.l` | 
| Call name | `cpg.call.name.l` | 
| Location | `cpg.call.name("foo").location.l` |
| Calling method | `cpg.call.name("foo").method.l` |
| Argument | `cpg.call.name("foo").argument.code.l` |
| Filter | `cpg.call.filter(_.argument.code("42")).name.l` |

### Sample Queries

 - List exact code strings of all the call-sites in the graph:

```scala
joern> cpg.call.code.l

res45: List[String] = List(
  "exit(0)",
  "printf(\"What is the meaning of life?\\n\")",
  "exit(42)",
  "fprintf(stderr, \"It depends!\\n\")",
...
)
```

 - Identify if an `exit()` call-site is called from within a conditional block (`if { }`) with a given condition. This can be done via first identifying a call named `exit` and then traversing the AST upwards (via `astParent`) until a `if` control structure is encountered. This only yields the specific `exit` which is withing the `if` block :

```scala
joern> cpg.call.name("exit").where(_.repeat(_.astParent)(_.until(_.isControlStructure.parserTypeName("If.*")))).code.l 

res69: List[String] = List("exit(42)")

```

 - Identify all the call-sites of `exit()` function in code, along with line-number and filename:

```scala
joern> cpg.call.name("exit").map( c=> (c.name, c.location.lineNumber.get, c.location.filename)).l 

res56: List[(String, Integer, String)] = List(
  ("exit", 11, "/tmp/x42/c/X42.c"),
  ("exit", 8, "/tmp/x42/c/X42.c")
)
```

 - List the arguments of all the `exit()` functions called in the code:

```scala
joern> cpg.call.name("exit").map( c=> (c.name, c.start.argument.code.l, c.location.lineNumber.get)).l 

res59: List[(String, List[String], Integer)] = List(
  ("exit", List("0"), 11),
  ("exit", List("42"), 8)
)
```
