# Amal Ahmed - Linking types for ML Software

Software developers compose systems from components written in many different languages. A business-logic component may be written in Java or OCaml, a resource-intensive component in
C or Rust, and a high-assurance component in Coq. In this multi-language world, program
execution sends values from one linguistic context to another. This boundary-crossing exposes values to contexts with unforeseen behavior – that is, behavior that could not arise in the source language of the value. 

**this exemplifies pretty well**

This paper proposes the novel idea of
linking types to address the problem of reasoning about single-language components in a multi-lingual setting.

**a solution I can use for related work**

## Working with FFI parts

Using the “best language” means the language that makes it is easiest for a programmer to reason about the behavior of that part of the system.
Moreover, programmers should be able to reason only in that language when working on that component.


Even if a high-assurance componen\ss written in Coq, the programmer must reason about extraction, compilation, and any linking that happens at the machine-level. 

This is a problem for programmers, because as they evolve complex systems, much time is spent refactoring – that is, making changes to components that should result in equivalent behavior. Programmers reason about that equivalence by thinking about possible program contexts within which the original and refactored components could be run, though usually they only think about contexts written in their own language. But if they have linked with another language, the additional contexts from that language also need to be taken into account. Equivalence in all contexts,or contextual equivalenc  is therefore central to programmer reasoning

**The part above summarises why is that challenging from the programmers perspective**

the situation is made worse by the fact that the common target is likely a low-level unsafe language like assembl hich permits direct access to memory and the call-stack. An object-code linker will verify symbols, but little more. 

C code they write can easily disrupt the equivalences they rely on when reasoning about their OCaml code.

OCaml function that is polymorphic in its arguments could have these arguments inspected by C code it linked with, violating parametricity. 

**more examples on the challenges, getting more specific with parametricity**

Since programmers use a language for its features and linguistic abstractions, we would like programmers to be able to reason using contextual equivalence for that language, even in the presence of target-level linking. A fully abstract compiler enables exactly this reasoning:
it guarantees that if two components are contextually equivalent at the source their compiled versions are contextually equivalent at the target.

An example of additional behavior is a non-concurrent language linking with a thread implementation written in C. An example of additional control is an unrestricted language linking with a concurrent data structure written in Rust, where linear types ensure data-race freedom.
**one solution: an abstract compiler, that provides contextual equivalence at tthe source and the target, but it is too hard to implement as this compiler would throw away everything inexpressible**

A has features unavailable in B that can be used to create contexts that can distinguish components that are contextually equivalent in B 
A has rich type-system features unavailable in B that can be used to rule out contexts that, at less precise types, 

Linking with code from more expressive languages affects not just programmer reasoning,but also the notion of equivalence used by compiler writers
to justify correct optimizations. While there has been a lot of recent work on verified compilers, most assume no linking (e.g., [...]), or linking only with code compiled from the same source language [4, 5, 15, 23, 16].

**affects also the compiler, not only the programmer**


Another approach is the multi-language style of verified compilers by Perconti and
Ahmed, which allows linking with arbitrary target code that may be compiled from another source language R . This approach, which embeds both the source S and target T into a single multi-language
ST, means that compiler optimizations can be justified in terms of ST contextual equivalence. However, as a tool for programmer reasoning, this comes at a significant cost, as the programmer needs to understand the full ST language and the
compiler from R to T. 

**their own approach that is an issue for the programmer**

We contend that compiler writers should not get to decide what linking is allowed, and
indeed, we don’t think they want to. Currently compiler writers are forced to either ignore
linking or make such arbitrary decisions because existing source-language specifications are
incomplete with respect to linking. Instead, this should be a part of the language specification
and exposed to the programmer so that she can make fine-grained decisions about linking,
which leads to fine-grained control over what contexts she must consider when reasoning
about a particular component

Every compiler should then be fully abstract, which means it preserves the equivalences chosen by the programmer
**their opinion is that linking should be done on the language design side**


Some security vulnerabilities rely on the fact that the cost of a computation may be discernable
(e.g., by observing time, or CPU or memory consumption). To prove the absence of such
vulnerabilities, we could remove the mechanism of observation – but this is likely impossible,
since even if we remove timing from our language, if the program communicates over the
network timing can happen on other systems. A more promising strategy is to introduce
the notion of cost (time or space) into the model and then prove that various branches are
indistinguishable in that model (see, e.g. [6], [14], [9]). 

side channels like timing. 

**here we are back to security**


Large software systems are written using combinations of many languages. But while some
languages provide powerful tools for reasoning
in the language, none support reasoning
across multiple languages. Indeed, the abstractions that languages purport to present do
not actually cohere because they do not allow the programmer to reason solely about the
code she writes. Instead, the programmer is forced to think about the details of particular
D. Patterson and A. Ahmed 12:13
compilers and low-level implementations, and to reason about the target code that her
compiler generates.
With linking types , we propose that language designers incorporate linking into their
language designs and provide programmers a means to specify linking with behavior and
types inexpressible in their language. There are many challenges in how to design linking
types, depending on what features exist in the languages, but only through accepting this
challenge can we reach what has long been promised – an ecosystem of languages, each suited
to a particular task yet stitched together seamlessly into a single large software project.

**that is their conclusion**