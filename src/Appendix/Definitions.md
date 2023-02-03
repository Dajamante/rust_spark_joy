# Definitions 

This is a glossary for the encountered terms.


**Affine language**
An affine language is a type of programming language that uses affine types, which are a type system that restricts the usage of values based on the number of times they are consumed. In an affine language, values are either "used" (or "consumed") exactly once, or not used at all. This allows affine types to enforce strict resource management policies and to prevent bugs caused by unexpected sharing or reuse of values.

**Affine types** are similar to linear types, but they are a weaker restriction as they only enforce that a value is used at most once. This allows for more flexible resource management policies and can make it easier to write correct code.

**Linear types**
Linear types are a type system used in programming languages to restrict the usage of values based on the number of times they are consumed. In linear type systems, values are either "used" (or "consumed") exactly once, or not used at all. This allows linear types to enforce strict resource management policies and to prevent bugs caused by unexpected sharing or reuse of values.

**Target-level linking** is a process in computer programming where the machine code of different components of a software system is combined and linked together to create a final executable file. The term "target level" refers to the level at which the linking occurs, typically the machine code level.

A **linking type declaration** is a type definition in a programming language that specifies how data or components from one module or program can be linked to or used by another. It provides a way for the programmer to specify the compatibility and integration of different parts of a system, ensuring that the data and functionality can be used together in a type-safe manner. Linking type declarations can be used to enforce inter-module compatibility and help prevent issues like type mismatches or unintended data interactions. The exact syntax and behavior of linking type declarations may vary between programming languages.