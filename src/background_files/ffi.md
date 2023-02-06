# FFI

## Why is FFI useful

FFI (Foreign function interface) is a gluing layer that allow to connect software written in different language. The use of combining software written in different languages seems relatively obvious: different languages have different strengths due to their design choices. 

In Improving Quality of Software with Foreign Function Interfaces using Static Analysis, Li. cover the reason for FFI prevalence in modern software.

Firstly, as software development requires faster processes, higher quality and faster time to market, the use of FFI seem like a good solution. FFI allows to leverage existing libraries, extensively proved, or which offer standard libraries and functionality. In the case of SPARK and Ada, using FFI allows access to software that has been proved mathematically correct and is ISO certified.


### Software written with FFI

Well used software composed with FFI layers can be listed easily (Li. Zhuohua. Mergendhal): 
- the Java Native Interface (JNI) glues together the Java development Kit (JDK) and its core systems libraries written in C and C++.
- the Android mobile OS includes a Java VM and a C/C++ kernel
- Firefox - studied extensively by Mergendhal et al.- is also a well known example
- the Linux kernel is also currently integrating Rust as a core language at time of writing 

### Software quality goals with FFI

Li. distinguishes four software quality issues caused by FFI: security, reliability, safety and performance.

Security implies that the system "the system cannot be exploited in its FFI layer for security attacks". Reliability implies that [FFI should not]"cause the system to behave unexpectedly". Safety implies that multithreaded execution is safe. This property has been studied extensively but only for uni-language system, and is therefore not relevant for FFI. Commonly, safety issues involve"data integrity, thread synchronization, race condition, and deadlock". While the present thesis will not investigate multithreading, it is important to stress that it is a core issue in what makes FFI less safe. Performance issues are mostly related to memory management, that Li. links to "known problems such as memory leaks and dangling pointers".

## Why is FFI dangerous

Calling external code is inherently dangerous in Rust (Mergendahl, Zhuohua).

### New paradigm / conceptual approach for programmers

Li et al., stress why using FFI is an error prone process for the programmer. From the programmer, it is not only expected to reason in their own language, but accross different languages. Some differences are straightforward, such as "program semantics, language syntax, and type systems" while some are more subtle, such as "exception handling and memory management". All of the sudden, the programmer needs to understand "intricacies and complex interractions" of programming languages, as well as the context that allows the apparition of certain bugs.

In a safe language such as Rust, programmers may misuse the unsafe abilities that are provided to them (Zhozhua et al.)

In other words, FFI "present unique challenges in both identification of the bug patterns and the design of suitable solutions to find these types of bugs" (Li.). For example, Java programmers consider "native methods as black boxes", even "avoiding the reasoning about interleaving between Java code and native code". This challenge is made harder by the lack of experience in FFI and the lack of empirical or experimental research. From the programmer or researcher side, it requires keen knowledge and comfort with both the foreign and host languages, in particular within "type
systems, exception handlings, memory management, and thread models", as well as "in-depth understanding of the interactions of
the two programming languages through the use of an FFI and the impact from the FFI
itself during the interactions". 

### Hard from the compiler perspective

### Bugs

#### How to find bugs


##### Static and dynamic analysis
Related work are unanimous with regards to how software bugs in FFI can be found. Static and dynamic analysis (Li). In addition, Li recommends to combine static and dynamic analysis (hybrid), such as taint targets and taint sinks to find taint paths - taint analysis relies on a pointer graph that analysis how a problematic pointer can contaminate the control flow.


In dynamic analysis, bugs are found by writing tests and will be revealed during runtime. Li warns that triggering FFI tests is difficult, due to their particular position as inter-layer between two software systems, and requires extensive manual tests. 


Static analysis bugs are discovered quicker, while scanning the code and mapping control flow. The downside of static analysis, as stressed by Li and Zhuohua, is false positive that need proper filtering. On the positive side, since static analysis does not require to run the software, it has no runtime overhead. It can identify quality issues as it checks carefully every control flow path. Finally, Li approves greatly of static analysis as bug finding tool as it requires "in-depth knowledge of static analysis, rigorous studies and research in the area of programming languages and software engineering, and proposals of novel improve
ments and applications of static analysis to address the unique and challenging issues in
software composed of FFIs", which the author sees as an positive points which create "great intellectual curiosity, interest and creativity". 



Work to improve FFI safety prior to 2014 also focuses on improving static analysis and dynamic checks. 
Even worse, the tools designed for finding bugs in FII can be "off-limits in practice for the most part for programmers who develop software using FFIs". The reasons listed by Li are that the tools are difficult and impractical to use and the bugs are rarely of practical importance. 
 

#### FFI introduces an new kind of bugs

Bugs introduced by FFI are not straightforward (Li.). They are subtle and the lack of previous experience and tools introduces additional complexity. Furthermore, in a constatation from the same author dated 2014, there was very little research "in the systematic identification of bugs caused by FFIs, whether empirical or experimental".

At the time of writing, Li insists that the search for bugs patters particular to FFI has just begun. The reports available to the author focus on a small set of problems and "touch only a few software quality issues" - while some bugs are "unique to theses systems". This is also a point confirmed by Merghendal et al., as well as Zhuohua.

##### Bugs found in the Java Native Interface (JNI)

Li. investigates bugs found in the JNI and finds them of two types.
Li. refers to studies which found hundred of interface bugs in JNI programs. Said studies used methods ranging from "type systems, experimental studies, and empirical studies". According to Li, error were cause by the fact that FFI provide close to no support for safety checking, but also because interface code requires resolving differences between language paradigms - aka "memory models and features".

Without entering into too much details not too closely related to this thesis, one pattern causing vulnerabilities is the mishandled Java exceptions. At time of writing (2014), the pattern was unique (Li.). Li analyses two types of bugs. Bugs when the implementation can throw some exceptions, but the native method does not declare those. Or bugs when the implementation throws checked exception that is not a subclass of the declared exceptions by the native method. To quantify their results, Li found 147 true bugs (129 of them are because of "implicit throws" or unchecked exceptions), in popular packages such as posix, java-gnome, or several java.net, java.security or java.util packages. 

Li. adds that "the Java compiler does not perform compile-time exception checking on native methods, in contrast to how exception checking is performed on Java methods". This is similar to how the SPARK compiler GNAT works: it will not use it's prover on methods imported from Rust. Additionally, the "JVM does not provide runtime exception handling for JNI exceptions" this means that on the native side, a pending exception  will not disrupt the code execution immediately.

Li also notes that "the error pattern of mishandling exceptions is not unique to the JNI. Any programming language with managed environments that allows native components to throw exceptions faces the same issue". Rust is known for it's rich error handling management (distinguishign recoverable vs unrecoverable errors, error propagation) and Li experiments need to be verified by us as well. In addition, he notes that C is not providing exception handling, and uses a code to propagate errors to callers. He describes this process as "tedious" and leading to many programming mistakes, especially in embedded software.


##### Bugs found in the Python/C interface

Python extension modules in C/C++ can be written with the Python/C interface. But this native code is outside Python's garbage collector which does its memory management. The responsibility to count references relies on the programmer.

Python allocates objects on its heap. When no longer in use, Python's memory manager launches the garbage collector. Python is using a reference counting algorithm (Li) and every Python object has a reference-count field. Adjusting the reference count is done during program execution. On the other hand, native modules coming from C/C++ are outside the control of the garbage collector. Reference counting is very complex due to the need to control every path, as well as *borrowed or stolen references* (when the native code is saving itself some work by not incrementing reference count).
This becomes the native code responsibility and is very error prone. This problem was also stressed by Zhuohua et al. in interractions between Rust and C++.

Some of those bugs were found in Python libraries in the Fedora LINUX (Li.)

To quantify Li's results, their program found over 150 errors in 13 benchmark programs (pyOpenSSL, pyaudio, pycrypto...), with
low false positive (22%).

## Attack and example