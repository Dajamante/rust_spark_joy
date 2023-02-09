# Rust and SPARK

In this part we will cover Rust and SPARK, and what makes them relevant as safe languages according to literature. This is an attempt to explain why combining the choice of those two languages are relevant for the safety critical industry and for software safety.

Rust and SPARK enforce memory safety, type safety and have their own flavor of ownership.
### Type safety

Type safety is a feature of the programming language that is enforced at compile time. A strong type system implies that when a value is declared of a certain type, the compiler will check whether this is true. 

### Memory safety

Memory safety means that memory should not be accessed in unsafe way. This includes verifying that memory is allocated before usage or that memory is not accessed out of bounds.

### Ownership 

Ownership is inspired by Linear Types (values that must be used exactly once) and Affine types (must be used at most once, i.e it can be ignored). Linear Types enable managing memory of mutable values without garbage collector (FERDOWSI). 

## Rust

Rust was designed as a systems programming language (Merendahl et al) designed from the start to intaract with other low-level languages. It focuses on speed, memory-safety and concurrency. It is multiparadigm, combining features from imperative and functional programming. It is fully open-sourced and has six-weeks release cycle, with backward compatibility guaranteed (Poveda Ruiz).

Rust is characterized by its "strong performance and safety properties". Rust combines a strong type system enforced at compile time, and other compile time checks, to prevent "large classes of memory bugs" (Mergendahl).

Rust ensures spatial safety by performing compile time checks for statically-sized objects. For dynamically sized objects, instructions are passed to the binary, to delay those checks to runtime.

For temporal safety, Rust relies on its concept of ownership - a concept shared by SPARK. As described by Mergendahl et al., a unique owner for a value can exist at a time and the value is destroyed when the unique owner goes out of scope.  This implies that when a value is destroyed, the reference is nullified, without the need for garbage collection. The Rust compiler adds lifetimes to all values, which is a tag guaranteeing the value is still valid. The borrow checker makes sure that no value outlives its lifetime (Poveda Ruiz).

To relax this strictness, Rust can temporary transfer ownership, known as borrowing. More importantly, Rust allows one mutable reference (`&mut T`) or multiple immutable references to co-exist(`&T`) simultaneously - the two scenarios are exclusive. 

This is protecting against type related errors at compile time, as rustc (the rust compiler) will warn of any error.

#### Trivial example

```rust
let a = vec![1, 2, 3]; // a is a vector, it is heap allocated
let b = a;             // ownership of a is moved to b

println!("b: {:?}", a);
```

In addition, the Rust’s type system statically rules out data races. At it's heart, Rust type system has the concept of ownership. In the Rust ownership system, many alias can exist to a place in memory, but only one alias can give mutable access, that is the write capacity (Jung). If ownership is shared between many aliases, then those aliases can only read. The restrictions linked to borrowing ensures that a value cannot exist in memory without an owning variable in the same scope, and it is impossible to modify a variable from different threads, preventing data-races (Poveda Ruiz). 

The system of ownership is not applied to simple types for which the compiler knows the size at compile-type. Those types are known to implement the `Copy trait`. It means that the compiler can copy those types without needing to transfer ownership. Some simple types implementing the copy traits are integers, for example `i32` or `u64`,

Rust allows unsafe operation that are fenced by the keyword `unsafe{}`. Inside unsafe brackets, operations such as raw pointer manipulation, or initialisations of unsafe objects are possible, under the programmer responsibility. Unsafe blocks are used mainly to intaract with low level code.

Rust does not implement exception, but has an error managment system, distinguishing between recoverable and unrecoverable errors. Recoverable errors can be managed by the program, while unrecoverable errors will make the program stop and unwind the stack -called `panic`.

In this somehow circumvoluted example, we see that the compiler will not accept modification of a global variable from different threads:

```rust
use std::thread;

// Global variable
static mut SHARED: i32 = 0;
fn main() {

   // The compiler will warn about aliasing violations or data races that will cause undefined behavior
   let cls = thread::spawn(move || {
        for _ in 0..10000 {
            SHARED += 1;
        }
   });
   for _ in 0..10000 {
      SHARED += 1;
   }

   cls.join();

    print!("Shared {shared}");
}
```

On the contrary, the program above will compile and run, and print all possible results between 100000 and 200000, as expected from a data race.

```rust
use std::thread;
static mut SHARED: i32 = 0;

fn main() {
    let cls = thread::spawn(move || {
        for _ in 0..100000 {
            // Unsafe block allows to increment inside the closure
            unsafe {
                SHARED += 1;
            }
        }
    });
    for _ in 0..100000 {
        unsafe {
            SHARED += 1;
        }
    }

    cls.join();

    unsafe {
        print!("Shared {shared}");
    }
}

```

#### Does Rust delivers on its safety promises?

Some authors studies whether Rust could hold the garantees it promises (Xu et al.) while others studied whether programmers were using Rust *responsabily* (Evans et al.). Xu et al. surveyed 186 Rust CVEs and found that memory-safety bugs required unsafe code, except one compiler bug. They conclude that bugs can only be introduced by using unsound APIs -including APIs using FFI. Evans et al., results are somehow different. From a large scale empirical study, Evans et al. conclude that, while unsafe is used in less than a third of Rust libraries, a half of those cannot be statically checked. Furthermore, unsafe Rust can be hidden in dependencies, and that the "propagation of unsafeness offers
a challenge to the claim of Rust as a memory-safe language". In this case the authors recommend even changes to the rust compiler to raise awareness in programmers, when their code is rendered unsafe through dependencies.


#### Formalisation

Rust is an open source project but there are many efforts ongoing for its formalization. The Rust Belt[^1] project aims to "to equip Rust programmers with the first formal tools for verifying safe encapsulation of "unsafe" code. Rust Belt defines a set of rules
to model Rust programs and use those rules to prove mathematically the security of those programs. It has proved the safety of basic typing and that there is no undefined behaviour in well-typed Rust (Xu et al.).

The Ferrocene language specification[^2] (a collaboration between Ferrous Systems and AdaCore) is aiming to document the behaviour of the Rust compiler in a standardized form. The goal is to qualify a version of the Rust compiler from safety-critical industries.
The FLS specify amongst others, the form of a program and the effect of executing this program.

# Ada

Ada is a high-level programming language, designed for long term applications that should not be dependant of software updates -typically the safety-critical industry. It was developed in the early 1980s and has a syntax inspired by the Pacal language family.
The advantages of Ada are many (Moy and Chapman):
- its syntax protects its from assignment and comparison as well as the dangling else problem
- no explicit use of pointers as in C
- strong typing system preventing confusion or abusive type promotion
- many features for concurrency

## SPARK
SPARK, currently in its version 2014, refers a set of tools for verification as well as the language name.

SPARK was designed to be the largest subset of Ada that was "still amenable to simple specification and sound verification", and as such the use of pointers (which permits aliasing!) was not a part of the language until recently. (Jaloyan et al., Dross on AdaCore blog).

 SPARK supports "basic data and control flow analysis, exhaustive detection of uninitialized variables and ineffective assignment". SPARK supports mathematical proof, that are used to prove the absence of runtime exception, proving security and safety properties, or that the software meets the required behaviour or its specifications (SPARK community site)[^spark].
 
Carré (one of the original creators of SPARK) and Garnsworthy define the rationale of SPARK as below:
- logical soundness: eliminate all ambiguities
- simplicity of formal definition
- maintain expressive power to ensure rigorous program development and faithful representation of abstractions
- security, as failure in the safety-critical programming field is untolerable
- verifiability

SPARK is compiled with the Ada compiler (GNAT will be used in the experiments in this thesis) but it has in addition the SPARK checker. After the release of Ada 2012, the contract API (pre and post conditions coming from modeling language) became a part of the language. SPARK verifies contracts are always upheld, insuring no runtime error, proving for example that no division by zero is possible or that the result of an increment will always be bigger than the number sent to the program. In addition, SPARK ensures thread safety via synchronized objects (Scherer, SPARK resources).


As an example from AdaCore documentation, this program is legal in Ada but illegal in SPARK, as the operation on `X` is not begnin:

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

Ownership in SPARK is different from ownership in Rust. In SPARK, only read aliasing is allowed, when both names are only used to read the data. "Assignment between access objects operates a transfer of ownership, where the source object loses its permission to read or write the underlying allocated memory". (AdaCore resources Learn.Adacore.Com)

SPARK 2014 has pointers inspired from Rust[^sparkpointers]: there is single ownership for poitners as we can see in this example:

```pascal
procedure Test is
  type Int_Ptr is access Integer;
  X : Int_Ptr := new Integer'(10);
  Y : Int_Ptr;                --  Y is null by default
begin
  Y := X;                     --  ownership of X is transferred to Y
  pragma Assert (Y.all = 10); --  Y can be accessed
  Y.all := 11;                --  both for reading and writing
  pragma Assert (X.all = 11); --  but X cannot, or we would have an alias
end Test;
```

SPARK allows begning aliasing (read) that it calls observability.

[^1]: https://plv.mpi-sws.org/rustbelt/#project
[^2]: https://spec.ferrocene.dev/general.html
[^spark]: https://www.adacore.com/about-spark
[^sparkpointers]: https://blog.adacore.com/using-pointers-in-spark§
