# State of software safety

In an AdaCore publication by Moy and Aiello, state of software security is discussed and exemplified, especially in embedded systems. The authors advocate for formal verification, as testing do not cover potential failures in the safety-critical industry, which are (not exhaustively): "aircraft (...) automobiles, medical devices, home appliances, (...)homes
themselves. As products move from hardware-only to cyber-physical, relying on the underlying software for functionality, while software is vulnerable.
Verification is a tough problem as the software complexity is increasing constantly.

![](../Appendix/complexity.png)


The authors proceed with examples that brought down ATT (with a cost of $60 and long lasting damage to its reputation), or the US FDA arning against almost 500 thousand cardiac devices that put the patients in pain or at risk of death, or the WannaCry ransomware attack that ecrypted 200 thousand computers in 150 countries and costed up to $8 billion dollars.

In his dissertation thesis Scherer reports successful attacks of "train schedule systems, entire energy grids, hospital networks, vehicular control software and even critical aircraft systems". Scherer means that security needs to be built in software development, instead of an add-on or an afterthought. Scherer also notes the "continuously increasing rate of bugs with increasing project size amongst other
factors in long lived projects" and a slow turnaround time" for software improvement (1 to 10 years), which cause problems to pile up.


Moy and Chapman explain why designing secure software is inherently hard. In addition to complexity, we evoluate in a malicious environment. Writing secure software is equivalent to “Programming Satan’s Computer” (as coined by Anderson and Needham): "Satan’s computer doesn’t fail
randomly either — it fails intelligently, in the worst -
possible way, at the worst -
possible time, and it can fail in ways that you don’t even know
about". The authors lift a series of asymetries. An asymetry of capability, meaning that the malicious actors capabilities does not grow in a predictable and linear way. An asymetry of effort: while the programmer has to secure all possible security indoors, the malicious actor needs to find just one breach in the system. Asymetry of knowledge, which is self explanatory in the context of rapid progress of knowledge and technical support. And lastly, an asymetry of impact, as the impact and destructivity of a default is in principle unpredictable. Scherer notes that a defect is guaranteed to be exploited when discovered as malicious actors systematically screen for deffect. As a result, "the chance for a defect to be discovered by the attacker is significantly higher than the randomized detection happening during testing".

While basic approaches like "like modelling, formal analyses different stages of testing,continuous integration and traceability of requirements are applied in modern projects" (Scherer), some are reserved to critical safety software. In addition, modeling can suffer from under-modeling and over-modeling (Scherer) become non suitable to improve the software.

## Memory safety by safe languages

To date, there is some body of work on securing multi-language applications. This body of work is based on formal methods, securing FFI, isolation or sanitizers. 

Merendahl et all state that "differing assumptions leads to adverse effects holds generally—multi-language applications must be explicit about the assumptions each component is making, and
care must be taken to ensure that the application as a whole is not weaker than its constituent parts". They proved it was true for both safe/unsafe and safe/safe pairings. In those conditions, more research and recommendation on how those pairings must be made is necessary.

Scherer writes that the choice of programming language has a significant impact on project
success and defect rates".

This thesis will discuss the vulneraibilty for attacks, but even outside of malicious actors, human error is an open door to vulnerability.
For example, the variable assignment inside an if-statement is a classical C-error plaging code bases and given to CS graduates during interviews. 

Scherer notes that in 2018, the "4 most forked C++ projects on github.com", amongst which bitcoin, contained 30k equality operations, with "only 150 assignments in
if conditions". Most of assignment in if condition are correct, but the author mean that there is room for trivial errors.


## Memory corruption attacks

Mergendahl et al. state that the underlying root to all evil, namely memory corruption attack, is the unsafety of programming languages such as C/C++ "which delegate security checks to the programmer". As a consequence, developper errors introduce spatial and temporal errors.

To mitigate those problems, various tools have made their apparition, as well as "exploit mitigation techniques". The lattest can be devided into enforcement (control flow integrity) or randomization based (memory randomization)". Unfortunately those tools are providing very limited protection, while facing a professionalisation and increase of complexity from the attacking side. According to the authors, "sanitizers suffer from coverage limitations, often resulting in many missed bugs" while "enforcement-based exploit mitigation techniques are shown to provide relaxed-
enough policies that allow an attacker to mount a successful attack without violating their policies", while randomization techniques are vulneratble to information leakage.

## Dependencies

Merendahl et al. remind that applications written in C++11 "are still backwards compatible with older C++ standards and C applications, and often feature code with older code standards" especially imported libraries. Such components fragilize safety garantees. Zhuohua found that over 54000 out of 77000 were affected by chain of dependencies that contain unsafe FFI calls.

## Memory corruption bugs

In his very much cited blog article, Alex Gaynor reports that in a million LOC code base written in an unsafe language, 65% of security vulnerabilities cna be expected from memory unsafety.
Going further, Gaynor illustrates with code bases such as Android ("Our data shows that issues like use-after-free, double-free, and heap buffer overflows generally constitute more than 65% of High & Critical security bugs in Chrome and Android."), iOS and macOS ("Across the entirety of iOS 12 Apple has fixed 261 CVEs, 173 of which were memory unsafety. That’s 66.3% of all vulnerabilities.” and “Across the entirety of Mojave Apple has fixed 298 CVEs, 213 of which were memory unsafety. That’s 71.5% of all vulnerabilities.”) Microsoft ("70% of the vulnerabilities Microsoft assigns a CVE each year continue to be memory safety issues") and Ubuntu Linux kernel ("65% of CVEs behind the last six months of Ubuntu security updates to the Linux kernel have been memory unsafety.”). Gaynor provides other examples but the figure 65 to 70% is consistent and stressed in academic (Mergendahl et al) litterature or vulgarisation.
