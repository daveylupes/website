---
id: glossary
title: Glossary
weight: 130
---

<blockquote><i>Writing is nature's way of telling us how lousy our thinking is.</i><br />Leslie Lamport</blockquote>

### Augmentation

An Augmentation is an operation by which a Code Property Graph is extended with nodes, properties and edges.

### Augmentation Directive

CPGQL Augmentation Directives are Directives which extend a Code Property Graph with nodes, properties and edges.

### Call Site

Location in a Program Structure where a function or subroutine is called.

### Code Property Graph

Data structure designed for vulnerability discovery. A directed, edge-labeled, attributed multigraph, or _property graph_ for short.

### Code Property Graph Overlay

A set of Nodes, Node Properties and Edges grouped together based on certain criteria. For example, the Dataflow Overlay is the set of Nodes, Node Properties and Edges that are grouped together to represent Dataflows in a Code Property Graph.

### Code Property Graph Query Language

A domain-specific language designed for querying Code Property Graphs.

### Complex Step

CPGQL Complex Steps are Step which combine the functionality of one or more Node-Type Steps, Repeat Steps, Filter Steps, Core Steps or Execution Directives. They are represented by one or more Directives.

### Core Step

CPGQL Core Steps are Steps which can be combined with any other Step. They are represented by one or more Directives.

### Dataflow

A dataflow represents paths information can take from an external input of a program to an internal procedure.

### Dataflow Sink

The information consumer in a Dataflow, i.e. an internal procedure of a program.

### Dataflow Source

The information generator in a Dataflow, i.e. the input of a program.

### Dataflow Step

An atomic traversal on Nodes and Edges that are part of the Dataflow Overlay.

### Dataflow Overlay

The set of Nodes, Node Properties and Edges that represent Dataflows in a Code Property Graph.

### Dependency

External program code that is used in another program.

### Directive

CPGQL Directives are keywords of the Code Property Graph Query Language.

### Entry Directive

A CPGQL Entry Directive is a Directive which references the entry node of a Code Property Graph.

### Execution Directive

CPGQL Execution Directives are Directives which execute the traversals they suffix and return the result in a specific format.

### Filter Step

CPGQL Filter Steps are Steps which filter nodes in a traversal according to a criterion. They are represented by one or more Directives.

### Help Directive

The CPGQL Help Directive is a Directive which returns textual descriptions of other directives.

### Language Frontend

Joern component that generates Code Property Graphs from a program's source.

### Node, Edge, Graph

A Code Property Graph is a graph, that is, all objects are represented as nodes and their relationships are represented by edges. Objects represented by nodes are, e.g., files, methods, expressions, and even dataflows.

### Node Property

A key-value pair attached to a Node.

### Node Type

A label that defines the set of mandatory and optional Node Properties and Edges for a specific Node.

### Node-Type Step

CPGQL Node-Type Steps are Steps that traverse nodes based on their type. They are represented by a single Directive.

### Query

A CPGQL Query is a combination of more than two Directives.

### Program Code

Source representation of a program. Can be a directory with multiple source files, a jar file containing Java Bytecode or anything similar.

### Program Structure

The overall form of a computer program which represents its control flow and data structures.

### Repeat Step

CPGQL Repeat Steps are Steps which repeat another traversal multiple times. They are represented by one or more Directives.

### Script

A file containing instructions for Joern to execute.

### Semantic Overlay

The set of Nodes, Node Properties and Edges that represent Program Structure in a Code Property Graph.

### Step

CPGQL Steps are combinations of one or more Directives that describe graph traversals in the Code Property Graph Query Language. They are represented by one or more Directives.

### Tagging Overlay

The set of Nodes, Node Properties and Edges of a Code Property Graph that make up for a higher-level abstract representation of Program Structure.

### Transformation

A Transformation is an operation by which a Code Property Graph is generated from Program Code.

### Traversal

A recipe which, given a set of start nodes, describes a walk in the graph to reach a set of end nodes.
