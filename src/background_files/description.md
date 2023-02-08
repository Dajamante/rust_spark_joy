# Background description

This thesis will cover the interraction of Rust and SPARK and which safety guarantees are retained in the process, and give some recommendation on how to proceed. We make the hypothesis that most of the language coherency can be retained.

The background important to this work is some context on:
    - a general overview about memory safety and why safe languages are a solution to common memory errors and attacks
    - Multi-language applications and Foreign Function interfaces, including the new vectors of attack introduced by those
    - a more detailed description of the safe languages Rust and SPARK

Part 1: Memory safety.
In this part, attack techniques will be discussed even though it is not the main point of this thesis, to understand what kind of protections safe languages offer.