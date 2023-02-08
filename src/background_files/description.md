# Background description

This thesis will cover the interraction of Rust and SPARK and which safety guarantees are retained when both languages are combined. Both languages are safe languages but combining them still require clear guidelines and recommendations. We make the hypothesis that most of the language coherency can be retained if those guidelines are respective. When we say "safe", we mean memory-safe, and this is thesis mainly a work about memory: how it is manage in software by safe (typically C/C++) and unsafe languages (we focus on Rust and Ada/SPARK), and the interraction of both safe and unsafe languages. 
Even combination of safe languages can be expected to cause issues, as previous litterature show.

With this goal in mind, we provide some background to better apprehend those questions:

- [Part 1](./software_safety.md): a general overview about memory safety and why safe languages are considered to be a solution to common memory errors and attacks.
In this part, memory errors and attacks will be discussed to understand what kind of protections safe languages offer.

- [Part 2]: a description of multi-language applications. How they are established through FFI (Foreign Function interfaces).
In this part, we will also discuss briefly new threats and the  vectors of attack introduced by those composite software.

- [Part 3](./rust_and_spark.md): a detailed description of the safe languages Rust and SPARK, and how they achieve this status through memory safety, type safety and the concept of ownership.


