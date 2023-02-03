# Engineering of Reliable and Secure Software via Customizable Integrated Compilation Systems

Author: Oliver Scherer

## Motivation: Security should not be an afterthought

Train schedule systems [95], entire energy grids [79], hospital networks [24], vehicular control software [46] and even critical aircraft systems [110] have been successfully attacked.

On the other hand, implementing it will inevitably introduce defects [14].

Model driven development, formal analyses and similar holistic approaches are too expensive upfront for the application in small and medium sized companies, as well as hard to sell to large companies [77]. These methods are mainly used in
safety critical applications like military, aerospace, space and transportation, where a software defect is legally considered to be the author’s negligence.

**those recommendations can be used at less high level or critical level**

**reducing mechanical work is a priority. By CI, is infrastructure as code, one is model driven development and code generation, but code choice is determinant:


In 2018, the 4 most forked C++ projects on github.com (bitcoin, cocos2d-x, opencv, tensorflow) contained 20k equality operations (==) in if conditions, and
further 30k if conditions without an equality operation. In contrast, there are only 150 assignments in if conditions. Since equality operations are much more common than assignments, it is prudent to ensure that such a trivial error (forgetting to type the second = character) will not occur.

**this can be used about the probability of an error occuring**

Another avenue at reducing unnecessary mechanical work are high level language
features. The choice of programming language has a significant impact on project
success [130] and defect rates [75]. For safety critical systems where maintenance is hard or outright impossible Ada1 is a common choice due to the safety guarantees that are built in and don’t need to be validated after the fact [39].

Chapter 2 highlights stagnation in the actual application of the best practices in the engineering of reliable software. Basic approaches like modeling, formal analyses, different stages of testing, continuous integration and traceability of requirements are applied in modern projects [142]. While there still are minute improvements happening and the tooling around the software development cycle has many avenues for advancement, the process itself still requires not insignificant amounts of human interactions.

**chapter 1.3 details more of the problems with modeling (long cycles, toolchains, outdated compilation systems, incompatibilities**

modeling also suffers from the dangers of over- and under-modeling. If the models are too abstract there is no real benefit to the project,
if the models are too concrete, they are most likely the result of model


Unfortunately the turnaround times are commonly between 1-10 years, which means that by the time the problem has been solved, the workarounds in place are well established or the problem is not relevant anymore due to other changes. Together with a continuously increasing rate of bugs with increasing project size amongst other factors in long lived projects, the slow turnaround time causes the issues to stack up [104].

The gap between software development research and practice has been growing over
the last years. There exist many methodologies for formal correctness analyses
and model driven development, but in practice, most projects do not employ such
schemes due to the high upfront cost. This thesis aims to permit developers to
incrementally work towards the more advanced software development schemes by
incrementally introducing concepts as they become beneficial to the project.


Due to the systematic search for defects by the attacker, the chance for a defect to be discovered by the attacker is significantly higher than the randomized detection happening during testing and production. Thus the security of a software system cannot be quantified by judging the trend of found defects over time. An insecure system can thus by its very definition not be classified as reliable in case it is accessible by attackers in any manner. [65]

**this is good to put in parallel with other authors focusing on security**

### Notes about both languages, Ada and Rust

Programming language design rarely considers safety and error reduction. The only
commercially viable programming language before 2009, which was explicitly de-
signed for safety and which is successfully used outside of academia is the language Ada. In quick succession Go (2009 [150]), Rust (2010 [151]) and Swift (2010 [152]) were designed in order to eradicate the memory safety issues and common bug sources of the C, C++ and Objective-C languages.
The authors in [51] analyze various language subsets and their effectiveness. They note that such standards are either informal or formal, where the latter sees little use in practice due to the user being required to understand formal notation. The informal standards on the other hand suffer from ambiguities. It is also noted, that creating language subset standards is an expensive process, ranging usually from $50000 to $100000. These figures do not include the development of automated rule checking software.

** the issue of cost is not mentioned in other papers**

## About SPARK and Ada

In the case of Ada the Ravenscar profile prevents all deadlocks, while the SPARK subset completely eliminates runtime errors.
**describe that accurately in the thesis as it is a strong point**

The Ada Ravenscar profile [10, 18] is a subset of the Ada language that decreases
the chance for accidentally introducing non-real-time constructs and makes the programs easier to analyze. In addition to removing various Ada features, a best effort analysis for detecting blocking actions within protected operations (critical sections) is performed.


## About Rust
A repeated experiment [105, 111] shows that even trivial standard readability features like syntax highlighting have a significant positive impact on time needed to comprehend a piece of code. In summary, low comprehensibility of software negatively impacts the reliability of software in the long term due to the friction introduced in resolving problems properly.
**let’s write that Rust is great**


