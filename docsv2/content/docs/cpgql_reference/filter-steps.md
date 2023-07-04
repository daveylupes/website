---
id: filter-steps
title: Filter Steps
---

Filter Steps are CPGQL Steps which filter nodes in a traversal according to a criterion. Joern supports four _Generic Filter Steps_ which can be added to any other step, and number of specific filter steps called _Property Filter Steps_ which can be used on nodes of a certain type.
The _Generic Filter Steps_ are `filter`, `filterNot`, `where` and `whereNot`. The _Property Filter Steps_ for each node type correspond to the _Property Directives_ it has defined.

We will look at the behaviour of each of these steps while analyzing a simple program named `X42`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]) {
  if (argc > 1 && strcmp(argv[1], "42") == 0) {
    fprintf(stderr, "It depends!\n");
    exit(42);
  }
  printf("What is the meaning of life?\n");
  exit(0);
}
```

### Property Filter Steps

_Property Filter Steps_ are _Filter Steps_ which continue a traversal if the properties of the nodes they point to pass a certain criterion. 
For example, to query the Code Property Graph for all CALL nodes which have the string `exit` as the value for their CODE property:

```java
joern> cpg.call.name("exit").code.l 
res0: List[String] = List("exit(0)", "exit(42)")
```

The criterion is unique to every _Property Filter Step_. In the case of the `name` _Property Filter Step_ of the `call` _Node-Type Step_, its criterion is a string that represents a regular expression; that is, all following queries will deliver the same result for the `X42` program:

```java
joern> cpg.call.name("exit").code.l 
res0: List[String] = List("exit(0)", "exit(42)")

joern> cpg.call.name("[eE]xit").code.l 
res1: List[String] = List("exit(0)", "exit(42)")

joern> cpg.call.name("ex.*").code.l 
res2: List[String] = List("exit(0)", "exit(42)")
```

Just like all other _Filter Steps_, _Property Filter Steps_ can be chained together:

```java
joern> cpg.call.name("exit").code(".*0.*").code.l 
res0: List[String] = List("exit(0)")
```

Unlike _Property Directives_, _Property Filter Steps_ are usually greater in number than the properties defined on a node type. Most _Property Filter Steps_ have their negated version available:

```java
joern> cpg.call.name("exit").codeNot(".*0.*").code.l 
res0: List[String] = List("exit(42)")
```


### filter

The `filter` _Step_ is a _Filter Step_ which continues a traversal for all nodes which pass its criterion. The criterion of the `filter` step is represented by an expression which has one argument, a variable that points to the node matched in the previous step, and which returns a boolean. For example, suppose you'd like to query the Code Property Graph of the `X42` program for all CALL nodes which have `exit` as the value of their NAME property, and return their CODE property in a list:

```java
joern> cpg.call.filter(node => node.name == "exit").code.l
res0: List[String] = List("exit(0)", "exit(42)")
```

`filter` steps can be chained together:

```java
joern> cpg.call.filter(node => node.name == "exit").filter(node => node.code.contains("42")).code.l 
res0: List[String] = List("exit(42)")
```

And their expression can contain any combination of boolean statements:

```java
joern> cpg.call.filter(node => node.name == "exit" && node.code.contains("42")).code.l 
res0: List[String] = List("exit(42)")

// equivalent in logic to the query above
joern> cpg.call.filter(node => true && 1 == 1 && node.name == "exit" && node.code.contains("42")).code.l 
res0: List[String] = List("exit(42)")
```

One helpful trick is to use the shorthand `_` operator in `filter` expressions, which points to the single argument that is passed into it, that is, the node.

```java
// long form
joern> cpg.call.filter(node => node.name == "exit").code.l
res0: List[String] = List("exit(0)", "exit(42)")

// short form
joern> cpg.call.filter(_.name == "exit").code.l
res1: List[String] = List("exit(0)", "exit(42)")
```

### where

Just like `filter`, the `where` _Step_ is a _Filter Step_ (`ಠ~ಠ`) which continues a traversal for all
nodes which pass its criterion. The expression representing the criterion takes in one argument: a
variable that represents the traversal of the previous step. It returns another traversal that includes
all nodes from the previous traversal for which the criterion returns a non-empty traversal. Say you'd
like to query the Code Property Graph of the `X42` program again for all CALL nodes which have `exit`
as the value of their NAME property, and return the value of their CODE property in a list:

```java
joern> cpg.call.where(node => node.name("exit")).code.l 
res0: List[String] = List("exit(0)", "exit(42)")

// or using the `_` shorthand:
joern> cpg.call.where(_.name("exit")).code.l 
res1: List[string] = List("exit(0)", "exit(42)")
```

`where`'s expression supports traversals of any length:

```java
joern> cpg.call.where(_.name("exit").argument.code("42")).code.l 
res0: List[String] = List("exit(42)")
```

And just like `filter`, it supports chaining:

```java
// equivalent to the previous query
joern> cpg.call.where(_.name("exit")).where(_.argument.code("42")).code.l 
res0: List[String] = List("exit(42)")
```

### whereNot

`whereNot` is the inverse operation of the `where` _Step_.

```java
joern> cpg.call.whereNot(_.name("exit")).code.l 
res0: List[String] = List(
  "printf(\"What is the meaning of life?\\n\")",
  "fprintf(stderr, \"It depends!\\n\")",
  "argv[1]",
  "strcmp(argv[1], \"42\")",
  "strcmp(argv[1], \"42\") == 0",
  "argc > 1",
  "argc > 1 && strcmp(argv[1], \"42\") == 0"
)
```

It supports chaining as well:

```java
joern> cpg.call.whereNot(_.name("exit")).whereNot(_.name("strcmp")).code.l 
res0: List[String] = List(
  "printf(\"What is the meaning of life?\\n\")",
  "fprintf(stderr, \"It depends!\\n\")",
  "argv[1]",
  "strcmp(argv[1], \"42\") == 0",
  "argc > 1",
  "argc > 1 && strcmp(argv[1], \"42\") == 0"
)
```

And traversals of any length:

```java
joern> cpg.call.whereNot(_.name("exit").argument.code("42")).whereNot(_.name("strcmp")).code.l 
res0: List[String] = List(
  "exit(0)",
  "printf(\"What is the meaning of life?\\n\")",
  "fprintf(stderr, \"It depends!\\n\")",
  "argv[1]",
  "strcmp(argv[1], \"42\") == 0",
  "argc > 1",
  "argc > 1 && strcmp(argv[1], \"42\") == 0"
)
```


