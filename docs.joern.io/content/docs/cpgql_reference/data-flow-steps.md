---
id: data-flow-steps
title: Data-Flow Steps
---

Data-Flow Steps are Complex Steps that represent flows of data.

We will look at each one using our sample program `X42`:

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

### reachableBy

`reachableBy` is a _Data-Flow Step_ that returns sources for flows of data from sinks to sources.

```java
joern> cpg.call.name("strcmp").reachableBy(cpg.method.parameter).l
res0: List[MethodParameterIn] = List(
  MethodParameterIn(
    id -> 1000104L,
    code -> "char *argv[]",
    order -> 2,
    name -> "argv",
    evaluationStrategy -> "BY_VALUE",
    typeFullName -> "char * [ ]",
    dynamicTypeHintFullName -> List(),
    lineNumber -> Some(5),
    columnNumber -> Some(19)
  )
)
```

### reachableByFlows

`reachableByFlows` is a _Data-Flow Step_ that returns paths for flows of data from sinks to sources.

```java
joern> sink.reachableByFlows(source).l
res0: List[Path] = List(
  Path(
    List(
      MethodParameterIn(
        id -> 1000104L,
        code -> "char *argv[]",
        order -> 2,
        name -> "argv",
        evaluationStrategy -> "BY_VALUE",
        typeFullName -> "char * [ ]",
        dynamicTypeHintFullName -> List(),
        lineNumber -> Some(5),
        columnNumber -> Some(19)
      ),
      Call(
        id -> 1000112L,
        code -> "strcmp(argv[1], \"42\")",
        name -> "strcmp",
        order -> 1,
        methodInstFullName -> None,
        methodFullName -> "strcmp",
        argumentIndex -> 1,
        dispatchType -> "STATIC_DISPATCH",
        signature -> "TODO assignment signature",
        typeFullName -> "ANY",
        dynamicTypeHintFullName -> List(),
        lineNumber -> Some(6),
        columnNumber -> Some(18),
        resolved -> None,
        depthFirstOrder -> None,
        internalFlags -> None
      )
    )
  )
)
```

