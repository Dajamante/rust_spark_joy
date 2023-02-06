# Zhuohua - CL memory management issues in Rust

Although it is widely believed that gradually re-implementing security-
critical components in Rust is a way of enhancing software security, how-
ever, using FFI is inherently unsafe. In this paper, we show that memory
management across the FFI boundaries is error-prone. Any incorrect
use of FFI may corrupt Rust’s ownership system

or example, Firefox contains a considerable amount of Rust
code [
4
], and the Linux kernel is in the process of integrating Rust as its second
language for kernel development [
21
,
30
]

 libraries to avoid reinventing the wheels. Rust can be
used in conjunction with other languages because it supports Foreign Function
Interface (FFI), which enables Rust to call external interfaces and exchange
arbitrary data

 However, calling external code is inherently unsafe in Rust
because the Rust compiler cannot perform security checks across the FFI bound-
aries. Programmers may accidentally misuse the unsafe abilities that lead to vul-
nerabilities. In addition, different assumptions made by different languages make
it possible for attackers to maneuver between the FFI boundaries and exploit
these vulnerabilities [
24
]. Recent empirical studies [
12
,
41
] have shown that the
incorrect use of FFI is one of the most significant causes of real-world memory-
safety bugs. Even for Rust packages written in pure safe Rust (i.e., without using
FFI), they may still be affected because they may depend on other packages
that include FFI. According to our statistics (Sect.
2.2
), among around 77
,
000
packages on the official Rust package registry
1
, more than 72% of the packages
depend on at least one package that contains unsafe FFI calls. 

We perform extensive evaluations in the Rust ecosystem. We evaluate 987
packages crawled from the official package registry and detect 34 bugs among
12 packages. All the detected bugs have been manually confirmed and reported
to the authors and 15 of them have been fixed at the time of writing

 Rust can easily collaborate with other
languages through the Foreign Function Interface (FFI). In this paper, we con-
sider the case where the external code is written in C/C++ since this is the most
common usage of FFI. Integrating Rust code with C/C++ code is prevalent and
necessary because (1) Many C/C++ projects integrate Rust into existing code-
bases (e.g., the Linux kernel and Firefox) to enhance their security. (2) It can
avoid duplicated work and benefit from the rich ecosystem of libraries written
in C/C++. (3) C/C++ can be used for performance-critical scenarios

FFI is inherently unsafe. Programmers need to explicitly use the
unsafe
keyword to bypass the security check enforced by the compiler. Therefore,
using FFI is extremely error-prone. Existing studies [
12
,
24
,
41
] have shown that
the incorrect use of FFI has become a severe source of memory safety bugs.

In fact, we find that more than 72% of packages on the official Rust
package registry (
crates.io
) depend on at least one unsafe FFI-bindings pack-
age, as shown in Fig.
1
.


 Among all the 76
,
894 packages, we start from all the
packages that are of category “external-ffi-bindings” (900 packages). These pack-
ages contain direct Rust FFI bindings to libraries written in other languages,
often denoted by a “
-sys
” suffix. Then we collect all the reverse dependencies
of them and repeat this process to get multi-level dependencies. As a result, the
number of packages converges at the 10th level, with a total of 55
,
762 packages
(55
,
762
/
76
,
894
≈
72
.
52%). Note that the “external-ffi-bindings” category by no
means includes all the FFI binding libraries since many packages’ categories are
not tagged properly, hence the actual percentage can only be higher

here are two ways of passing a heap-allocated object across
FFI: (1) by
borrowing
the object as a reference, (2) by
moving
the ownership to
the FFI. For
borrowing
as a reference, the ownership remains on the Rust side, so
the ownership system is responsible for releasing the memory after it goes out of
3
As of February 14, 2022.
684 Z. Li et al.
Fig. 1.
Number of packages that depend on unsafe FFI
its scope. For
moving
the ownership, one can first “forget” it from the ownership
system, then pass it to the FFI via a raw pointer. The Rust standard library
provides several functions to “forget” an object, e.g.,
std::mem::forget
and
Box::into
raw
. In this case, the responsibility of memory management returns
back to the programmers, who have to take extra care because the ownership
system no longer takes charge


 heap memory is passed across the FFI boundaries, the ownership system
cannot guarantee its safety. Therefore the responsibility of memory management
returns back to the programmers, meaning that all kinds of common memory
corruption bugs that happen in C, like use-after-free, double free, and memory
leak, still exist. Listing
1
shows a memory leak found in package
emd
4
. In Rust,
Box
is a smart pointer type used to securely manage heap memory. The developer
uses
Box::into
raw
to expose the raw pointer of the heap memory managed by
the
Box
in order to pass it to the FFI. However, after using
Box::into
raw
, the
ownership system will “forget” the 

However, when passing heap memory across the FFI boundaries and cooperat-
ing with external code, developers usually have to transiently create unsound
states via unsafe code (e.g., creating temporarily uninitialized data). Then after
the external code finishes, developers manually clean up the states. If some error
happens in between, the execution stops and the stack is unwound, so the clean-
up procedure will not be executed. The remaining unsound state may cause
security issues.


 Note that the question mark operator
(
?
) at lines 5 and 7 means that if the operation fails, the function returns early
and propagates the error to the caller function. Therefore, the memory may be
leaked if the function returns early and hence the
free
function at line 10 will
not be called


3 Security and Memory Management Issues via FFI
