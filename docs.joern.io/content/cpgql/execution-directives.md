---
id: execution-directives
title: Execution Directives
---

Execution Directives are CPGQL Directives which execute the traversals they suffix and return the result in a specific format. The most straightforward _Execution Directive_ is `toList` which, as the name suggests, executes the CPGQL Query it suffixes and returns the results in a list:

### toList

```java
joern> cpg.call.name.toList 
res0: List[String] = List(
  "exit",
  "printf",
  "exit",
  "fprintf",
  "<operator>.indirectIndexAccess",
  "strcmp",
  "<operator>.equals",
  "<operator>.greaterThan",
  "<operator>.logicalAnd"
)
```

### l

`toList` has a shorthand named `l`:

```java
joern> cpg.call.name.l 
res0: List[String] = List(
  "exit",
  "printf",
  "exit",
  "fprintf",
  "<operator>.indirectIndexAccess",
  "strcmp",
  "<operator>.equals",
  "<operator>.greaterThan",
  "<operator>.logicalAnd"
)
```

### head

Executes the traversal and returns the first result

```java
joern> cpg.call.name.head
res0: List[String] = List(
  "exit",
)
```

### size

`size` executes the traversal and returns the size of the result set

```java
joern> cpg.call.size
res0: Int = 9
```

### Execution Directives and the Joern Interpreter

_Execution Directives_ sit at the border between CPQGL Queries and the Joern Interpreter, that is at the border between querying Code Property Graphs, and using the results of those queries for further processing using the Scala programming language. For example, say you'd like to take the results of a query execution and write them to a text file:

```java
// first import the namespace for Java I/O
joern> import java.io._ 
import java.io._

// afterwards open a file stream to a new file named `my-query-result.txt`
joern> val pw = new PrintWriter(new File("./my-query-result.txt" )) 
pw: PrintWriter = java.io.PrintWriter@fe4c8fc

// execute your query and store the results in a constant
joern> val myQueryResult = cpg.call.size 
myQueryResult: Int = 9

// cast myQueryResult to a string, and write it to the file stream
joern> pw.write(myQueryResult.toString()) 

// close the file streasm
joern> pw.close() 

```

You'll see your results written to a file, ready for post-analysis:

```bash
$ cat my-query-result.txt 
9
```

Writing the results of a query to a file can also be done in a more concise way using the `|>` operator provided by the Joern Interpreter:

```java
joern> cpg.call.size.toString() |> "my-query-result.txt"
```

### p

`p` executes the traversal and pretty-prints the results:

```java
joern> cpg.call.p 
res26: List[String] = List(
  "(CALL,31): ARGUMENT_INDEX: 3, CODE: exit(0), COLUMN_NUMBER: 2, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 11, METHOD_FULL_NAME: exit, NAME: exit, ORDER: 3, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,29): ARGUMENT_INDEX: 2, CODE: printf(\"What is the meaning of life?\\n\"), COLUMN_NUMBER: 2, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 10, METHOD_FULL_NAME: printf, NAME: printf, ORDER: 2, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,27): ARGUMENT_INDEX: 2, CODE: exit(42), COLUMN_NUMBER: 4, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 8, METHOD_FULL_NAME: exit, NAME: exit, ORDER: 2, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,24): ARGUMENT_INDEX: 1, CODE: fprintf(stderr, \"It depends!\\n\"), COLUMN_NUMBER: 4, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 7, METHOD_FULL_NAME: fprintf, NAME: fprintf, ORDER: 1, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,18): ARGUMENT_INDEX: 1, CODE: argv[1], COLUMN_NUMBER: 25, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 6, METHOD_FULL_NAME: <operator>.indirectIndexAccess, NAME: <operator>.indirectIndexAccess, ORDER: 1, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,17): ARGUMENT_INDEX: 1, CODE: strcmp(argv[1], \"42\"), COLUMN_NUMBER: 18, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 6, METHOD_FULL_NAME: strcmp, NAME: strcmp, ORDER: 1, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,16): ARGUMENT_INDEX: 2, CODE: strcmp(argv[1], \"42\") == 0, COLUMN_NUMBER: 18, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 6, METHOD_FULL_NAME: <operator>.equals, NAME: <operator>.equals, ORDER: 2, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,13): ARGUMENT_INDEX: 1, CODE: argc > 1, COLUMN_NUMBER: 6, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 6, METHOD_FULL_NAME: <operator>.greaterThan, NAME: <operator>.greaterThan, ORDER: 1, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY",
  "(CALL,12): ARGUMENT_INDEX: 1, CODE: argc > 1 && strcmp(argv[1], \"42\") == 0, COLUMN_NUMBER: 6, DISPATCH_TYPE: STATIC_DISPATCH, LINE_NUMBER: 6, METHOD_FULL_NAME: <operator>.logicalAnd, NAME: <operator>.logicalAnd, ORDER: 1, SIGNATURE: TODO assignment signature, TYPE_FULL_NAME: ANY"
)
```

### toJson

`toJson` executes the traversal and returns the results in a JSON string:

```java
joern> cpg.call.name.toJson 
res28: String = "[\"exit\",\"printf\",\"exit\",\"fprintf\",\"<operator>.indirectIndexAccess\",\"strcmp\",\"<operator>.equals\",\"<operator>.greaterThan\",\"<operator>.logicalAnd\"]"
```

### toJsonPretty

`toJsonPretty` executes the traversal and returns the results in a pretty-printed JSON string:

```java
joern> cpg.call.name.toJsonPretty 
res29: String = """[
  "exit",
  "printf",
  "exit",
  "fprintf",
  "<operator>.indirectIndexAccess",
  "strcmp",
  "<operator>.equals",
  "<operator>.greaterThan",
  "<operator>.logicalAnd"
]"""
```

### size

`size` executes the traversal and returns the number of results:

```java
joern> cpg.call.size 
res0: Int = 9
```
