# Pal - Memory corruption and counter measures


### C/C++ is bad

Attacks have been known since 30 years and
have been heavily exploited for the last 15 years. Applications, written in languages like C/C++ where the lack of memory or type safety makes enables attackers to alter the program behaviour and potentially take complete control of the control-flow thus enabling them to exploit the memory. The solution to it would be to write the applications in a type safe language and use annotations to make the code more secure. Unfortunately, this is unrealistic in many cases as low-level features are required for performance -critical programs (e.g.- operating systems). To curb the exploitation, operating system vendors and compiler designers took many measures. Stack cookies, exception handler validation, Data Execution Prevention and Address Space Layout Randomization make the exploitation of memory corruption bugs much harder. These anti-exploitation patches now ship by default on most


### reallife

The win32 platform has been a home ground for most memory
corruption attacks and malware, which forced Microsoft to
deploy aggressive anti-exploitation methods in their recent Operating System

OS for smartphones has a serious bug in the
heart of its code which is required to process videos

Other mitigation techniques like control flow integrity, data flow integrity, code pointer integrity, etc. It may be noted that even though the techniques discussed are popular in their implementation, each of them have their limitations and can be exploited through advanced attacks
like return -to -libc, copied code chunks, brute -forced/blind reverse oriented programming, JIT spraying, pointer inference and deference, heap spraying etc
#### and this gives a link to war memory