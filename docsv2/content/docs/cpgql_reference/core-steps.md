---
id: core-steps
title: Core Steps
---

Core Steps are CPGQL Steps which can be combined with any other Step.
Joern offers four _Core Steps_, `map`, `sideEffect`, `dedup` and `clone`.

We will look at each one while analyzing a simple program named `X42`:

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

### map

The `map` _Core Step_ is a Step which transforms objects in a traversal with an expression. Its expression takes one argument, a variable representing the item the `map` _Core Step_ is suffixing, and can return any other type.
For example, say that you'd like to return the value of the CODE property, together with the value of the TYPE_FULL_NAME property of all LITERAL nodes in `X42`'s Code Property Graph:

```java
joern> cpg.literal.map(node => List(node.typeFullName, node.code)).toList
res223: List[List[String]] = List(
  List("char *", "\"What is the meaning of life?\\n\""),
  List("int", "42"),
  List("int", "0"),
  List("char *", "\"It depends!\\n\""),
  List("int", "0"),
  List("char *", "\"42\""),
  List("int", "1"),
  List("int", "1")
)
```

### sideEffect

`sideEffect` is a step that executes a function on each node of the traversal it suffixes, without modifying
the original traversal.

```java
joern> cpg.literal.sideEffect(node => println("Called once for ID " + node.id.toString())).code.l 
Called once for ID 32
Called once for ID 30
Called once for ID 34
Called once for ID 28
Called once for ID 24
Called once for ID 23
Called once for ID 22
Called once for ID 17
res0: List[String] = List(
  "\"What is the meaning of life?\\n\"",
  "42",
  "0",
  "\"It depends!\\n\"",
  "0",
  "\"42\"",
  "1",
  "1"
)
```

### dedup

`dedup` is a step that removes duplicates from the traversal it suffixes.

For example, say you'd like to query `X42`'s Code Property Graph for the AST parent nodes of all CALL nodes, and print out their CODE property:
```java
joern> cpg.call.astParent.isCall.code.l 
res0: List[String] = List(
  "strcmp(argv[1], \"42\")",
  "strcmp(argv[1], \"42\") == 0",
  "argc > 1 && strcmp(argv[1], \"42\") == 0",
  "argc > 1 && strcmp(argv[1], \"42\") == 0"
)
```
Because of the structure of the resulting AST, the query returns a duplicate result. To remove it, add `dedup` to the query:

```java
joern> cpg.call.astParent.isCall.dedup.code.l 
res0: List[String] = List(
  "strcmp(argv[1], \"42\")",
  "strcmp(argv[1], \"42\") == 0",
  "argc > 1 && strcmp(argv[1], \"42\") == 0"
)
```
