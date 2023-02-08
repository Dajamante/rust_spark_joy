# Rust and SPARK

In this part we will cover Rust and SPARK, and what makes them so relevant as safe languages according to literature. This is an attempt to explain why combining those languages might be of the utmost interest for the safety critical industry.

Rust and SPARK enforce type safety and have their own flavor of ownership.
### Type safety


## Rust

Rust was designed as a systems programming language according to Merendahl et al.

Rust is characterized by its "strong performance and safety properties". Rust combines a strong type system enforced at compile time, and other compile time checks, to prevent "large classes of memory bugs" (Mergendahl).

Rust ensures spatial safety by performing compile time checks for statically-sized objects. For dynamically sized objects, instructions are passed to the binary, to delay those checks to runtime.
For temporal safety, Rust relies on its concept of ownership - a concept shared by SPARK. As described by Mergendahl et al., "only one owner of a value can exist at a time and when the owner goes out of scope, the value is destroyed". This is an adaptation of linear languages. Rust does temporary transfer of ownership, known as borrowing. More importantly, Rust allows "one ‘mutable reference’ or multiple immutable references to co-exist" simultaneously. This implies that when a value is destroyed, the reference is nullified, without the need for garbage collection. 

# Ada

Ada is a high-level programming language, designed for long term applications that should not be dependant of software updates -typically the safety-critical industry. It was developed in the early 1980s and has a syntax inspired by the Pacal language family.
The advantages of Ada are many (Moy and Chapman):
- its syntax protects its from assignment and comparison as well as the dangling else problem
- no explicit use of pointers as in C
- strong typing system preventing confusion or abusive type promotion
- many features for concurrency

## SPARK
SPARK is a subset of Ada, developed by the DoD.
SPARK, currently in its version 2014, refers a set of tools for verification as well as a subset of Ada (Adacore resources).
 
Carré and Garnsworthy define the rationale of SPARK as below:
- logical soundness: eliminate all ambiguities
- simplicity of formal definition
- maintain expressive power to ensure rigorous program development and faithful representation of abstractions
- security, as failure in the safety-critical programming field is untolerable
- verifiability

The last point is particularly important in the frame of this thesis, as its verifiability makes SPARK a very attractive target to be combined with Rust, another safe language touted to reduce memory errors. According to the Ada resource center, "the language is designed to support mathematical proof and thus offers access to a range of verification objectives: proving the absence of run-time exceptions, proving safety or security properties, or proving that the software implementation meets a formal specification of the program's required behaviour". It inherits of "Ada's strength" and properties such as properties
such as "correct uses of data, the absence of runtime errors, and even functional correctness with respect to a formally specified set of requirements" (Moy and Chapman). SPARK completely eliminates runtime errors (Scherer).

SPARK is compiled with the Ada compiler (GNAT will be used in the experiments in this thesis) but it has in addition the SPARK checker. After the release of Ada 2012, the contract API (pre and post conditions coming from modeling language) became a part of the language. SPARK verifies contracts are always upheld, insuring no runtime error (Scherer), proving for example that no division by zero is possible or that the result of an increment will always be bigger than the number sent to the program.

In addition, SPARK ensures thread safety via synchronized objects (Scherer).