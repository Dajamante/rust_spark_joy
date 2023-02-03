# Plan

## Intro
- Why is this an interesting topic?
- Provide examples where Ada was used in the safety critical industry in the form of a story.
- What is happening in the industry with Ferrocene example.
- Provide a good example with Rust (if possible not the usual 70% less mistakes) 
- Make a reading plan with boxes like in Oli’s thesis
- Anecdotic relevance: remind that incorrect programs can be compiled and communicate together: example is my first program that was returning some random address because the Rust side had a returning `i32`.
- Rust has the borrow checker, SPARK has gnatprove. But gnatprove would be very costly to integrate into compilation.
- Generalization of this work? 

## The background 
## My own background
- Description of memory safety, type safety, ownership
- Will need some description of Rust, Ada, SPARK and what makes SPARK itself and not Ada
- ABI between Rust and Spark: what kind of C ABI they use. ABi are mandated by the processor vendor, they become facto standard
- Remember there are not standards for FFI: they are provided by vendors
Ferrocene specsThe CLA paper, describe CLA attacks
- Describe the specs. Specs of Ada, specs of SPARK, the rust non-existent specs, so we could use - Ferrocene for reference (and what is Ferrocene), then the GNAT compiler specs as we will be using only the GNAT compiler.
- Maybe talk about portability: size was a problem before when we ported between different architectures
## Method
- Library used: `gpr-rust`, an adaptation of gprbuild for Rust that I cloned and just contributed to
List the experiments (1. Integers, 2. Enum, 3. Some Pointers…)
- Why choose just those experiments? Those are simple types as that will be replicated in bigger projects
We will focus on legality rules on both sides
- We will then make sure value, size and alignment are preserved (even though alignment was only for testing)?
- we look at memory safety, type safety and ownership as they are shared by both Rust and SPARK
Experiments
    - 4.1 Numbers Numbers
    - 4.2 Enums Enums
    - 4.3 Pointers Pointers
## Conclusion
## Further work

