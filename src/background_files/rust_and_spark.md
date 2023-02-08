# Rust and SPARK

In this part we will cover Rust and SPARK, and what makes them so relevant as safe languages according to literature. This is an attempt to explain why combining those languages might be of the utmost interest for the safety critical industry.

Rust and SPARK enforce type safety and have their own flavor of ownership.
### Type safety

Type safety is a feature of the programming language that is enforced at compile time. A strong type system implies that when a value is declared of a certain type, the compiler will check whether this is true. 

Rust and SPARK are both strongly typed.

### Memory safety

Memory safety means that memory should not be accessed in unsafe way. This includes verifying that memory is allocated before usage, that memory is not accessed out of bounds in arrays to be read or written to.

### Ownership 

Ownership is inspired by Linear Types ("values that must be used exactly once") and Affine types ("must be used at most once, i.e it can be ignored"). "Linear Types enable memory management of mutable values without garbage collector (FERDOWSI). 

## Rust

Rust was designed as a systems programming language (Merendahl et al) designed from the start to interract with other low-level languages.

Rust is characterized by its "strong performance and safety properties". Rust combines a strong type system enforced at compile time, and other compile time checks, to prevent "large classes of memory bugs" (Mergendahl).

Rust ensures spatial safety by performing compile time checks for statically-sized objects. For dynamically sized objects, instructions are passed to the binary, to delay those checks to runtime.

For temporal safety, Rust relies on its concept of ownership - a concept shared by SPARK. As described by Mergendahl et al., "only one owner of a value can exist at a time and when the owner goes out of scope, the value is destroyed". 

Rust can temporary transfer ownership, known as borrowing. More importantly, Rust allows "one ‘mutable reference’ or multiple immutable references to co-exist" simultaneously. This implies that when a value is destroyed, the reference is nullified, without the need for garbage collection. 

This is protecting against type related errors at compile time, as rustc (the rust compiler) will warn of any error.


-- the CVE paper
https://dl-acm-org.focus.lib.kth.se/doi/10.1145/3466642
#### Trivial example

```rust
let a = vec![1, 2, 3]; // a is a vector, it is heap allocated
let b = a;             // ownership of a is moved to b

println!("b: {:?}", a);
```

In addition, the Rust’s type system statically rules out data races (Jung). At it's heart, Rust type system has the concept of ownership. In the Rust ownership system, many alias can exist to a place in memory, but only one alias can give mutable access, that is the write capacity (Jung). If ownership is shared between many aliases, then those aliases can only read.

The system of ownership is not applied to simple types for which the compiler knows the size at compile-type. Those types are known to implement the `Copy trait`. It means that the compiler can copy those types without needing to transfer ownership. Some simple types implementing the copy traits are integers, for example `i32` or `u64`,

Rust allows unsafe operation that are fenced by the keyword `unsafe{}`. Inside unsafe brackets, operations such as raw pointer manipulation, or initialisations of unsafe objects are possible, under the programmer responsibility. Unsafe blocks are used mainly to interract with low level code.

Rust does not implement exception, but has an error managment system, distinguishing between recoverable and unrecoverable errors. Recoverable errors can be managed by the program, while unrecoverable errors will make the program stop and unwind the stack -called `panic`.


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

The last point is particularly important in the frame of this thesis, as its verifiability makes SPARK a very attractive target to be combined with Rust. According to the Ada resource center, "the language is designed to support mathematical proof and thus offers access to a range of verification objectives: proving the absence of run-time exceptions, proving safety or security properties, or proving that the software implementation meets a formal specification of the program's required behaviour". It inherits of "Ada's strength" and properties such as properties
such as "correct uses of data, the absence of runtime errors, and even functional correctness with respect to a formally specified set of requirements" (Moy and Chapman). SPARK completely eliminates runtime errors (Scherer).

SPARK is compiled with the Ada compiler (GNAT will be used in our experiments) but it has in addition the SPARK checker. After the release of Ada 2012, the contract API (pre and post conditions coming from modeling language) became a part of the language. SPARK verifies contracts are always upheld, insuring no runtime error (Scherer), proving for example that no division by zero is possible or that the result of an increment will always be bigger than the number sent to the program.

In addition, SPARK ensures thread safety via synchronized objects (Scherer).

Ownership in SPARK is different from ownership in Rust. In SPARK, only read aliasing is allowed, when both names are only used to read the data. "Assignment between access objects operates a transfer of ownership, where the source object loses its permission to read or write the underlying allocated memory". (AdaCore resources Learn.Adacore.Com)

From AdaCore documentation, this program is legal in Ada but illegal in SPARK, as the operation on `X` is not begnin:

```ada
procedure Ownership_Transfer is
   type Int_Ptr is access Integer;
   X     : Int_Ptr;
   Y     : Int_Ptr;
   Dummy : Integer;
begin
   X     := new Integer'(1);
   X.all := X.all + 1;
   Y     := X;
   Y.all := Y.all + 1;
   X.all := X.all + 1;  --  illegal
   X.all := 1;          --  illegal
   Dummy := X.all;      --  illegal
end Ownership_Transfer;
```

-- rust inspired pointesr!!

https://blog.adacore.com/using-pointers-in-spark§