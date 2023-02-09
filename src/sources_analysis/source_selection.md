# Source selection

## Academic sources
### Yes 

[Eternal war in memory](https://people.eecs.berkeley.edu/~dawnsong/papers/Oakland13-SoK-CR.pdf)

[Cross language attacks](https://www.ndss-symposium.org/wp-content/uploads/2022-78-paper.pdf)
Even though this paper is centered on C and Rust, this has motivated this thesis.

[Improving Quality Of Software With Foreign Function Interfaces Using Static Analysis](https://core.ac.uk/download/pdf/228638431.pdf)
Pretty good, exposes many principles and specific examples, unfortunately with Java and JNI but the unhandled exceptions might be reusable. Also talks in general terms of the problems for FFI.

[Detecting Cross-language Memory Management Issues in Rust](https://link-springer-com.focus.lib.kth.se/chapter/10.1007/978-3-031-17143-7_33). Forward searching from the first CLA paper.

[Memory corruption basic attacks](https://ijesc.org/upload/4a057117f90feff62ac8812928f09d10.Memory%20Corruption%20Basic%20Attacks%20and%20Counter%20Measures.pdf)

[Engineering of Reliable and Secure Software via Customizable Integrated Compilation Systems](https://publikationen.bibliothek.kit.edu/1000134165)
Focused on software quality even if I will not be using the MIRI implementation.

[Linking Types for Multi-Language Software: Have Your Cake and Eat It Too](https://drops.dagstuhl.de/opus/volltexte/2017/7125)
Pretty good about the challenges for the programmer and the compiler, to reason with FFI.
I find the part about linking types quite complicated and I am not sure how relevant it is to our main problem. How far fetched is it to talk about linking types in FFI?

[Borrowing Safe Pointers from Rust in SPARK](https://arxiv.org/pdf/1805.05576.pdf)
Obviously relevant.

[Ada bindings for C interfaces: Lessons learned from the florist implementation](https://link-springer-com.focus.lib.kth.se/chapter/10.1007/3-540-63114-3_2)
Relevant for Ada side.


[Foreign Function Typing: Semantic Type Soundness for FFIs](https://wgt20.irif.fr/wgt20-final23-acmpaginated.pdf)

Pretty accessible.

[Weird machines](https://ieeexplore-ieee-org.focus.lib.kth.se/stamp/stamp.jsp?arnumber=8226852)
Yes as we are talking about exploitation

[SPARK - An Annotated Ada Subset for Safety-Critical Programming](https://dl.acm.org/doi/pdf/10.1145/255471.255563)

[Memory safety challenges solved?](https://dl-acm-org.focus.lib.kth.se/doi/10.1145/3466642)

[Is Rust Used Safely by Sofware Developers?](https://dl-acm-org.focus.lib.kth.se/doi/pdf/10.1145/3377811.3380413)
### Maybe

[ML class level interfacing](https://www.adacore.com/uploads/techPapers/Class_level_interfacing.pdf)

[Strengthening memory safety in Rust: exploring CHERI capabilities for a safe language](https://nw0.github.io/cheri-rust.pdf)

[Checking Type Safety of Foreign Function Calls](https://dl-acm-org.focus.lib.kth.se/doi/pdf/10.1145/1377492.1377493)

[A modular foreign function interface](https://www.sciencedirect.com/science/article/pii/S0167642317300709)


[Formalizing a Secure Foreign Function Interface](https://link.springer.com/chapter/10.1007/978-3-319-22969-0_16)
Very abstract but presents the perspective of an attacker.

Building High Integrity Applications with SPARK
(available)

[Verification of Programs with Pointers in SPARK](https://link-springer-com.focus.lib.kth.se/chapter/10.1007/978-3-030-63406-3_4#Sec5)


[Operational Semantics for Multi-Language Programs](https://www.ics.uci.edu/~lopes/teaching/inf212W12/readings/mathews.pdf)


[Types and programming languages](https://ieeexplore-ieee-org.focus.lib.kth.se/book/6267321)
Book, might be relevant for definitions.

[Let Me Unwind That For You: Exceptions to Backward-Edge Protection](https://download.vusec.net/papers/chop_ndss23.pdf)
Forward searching from the CLA paper gives me this resource. They have great figures.

### No

[Efficient Type and Memory Safety for Tiny Embedded Systems](https://dl-acm-org.focus.lib.kth.se/doi/pdf/10.1145/1215995.1216001)

Not relevant as unilingual.

[Physical type checking for C](https://dl.acm.org/doi/pdf/10.1145/316158.316183)

Too focused on C!

## Good blog sources

[What science can tell us about C and C++'s security](https://alexgaynor.net/2020/may/27/science-on-memory-unsafety-and-security/)

[RustFest Rome 2018 - Tshepang Lekhonkhobe: Behind The Scenes Of Producing An Executable](https://www.youtube.com/watch?v=EZHnzTk8YaU)

[Future of memory safety](https://advocacy.consumerreports.org/wp-content/uploads/2023/01/Memory-Safety-Convening-Report.pdf)

[Rust vs Ada on Reddit](https://www.reddit.com/r/rust/comments/pm4k1f/rust_vs_ada_how_do_they_compare/)