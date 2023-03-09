# csharp-performance-aware-comparison
A C#/.NET project showing how modern programming techniques hurt performance and especially memory a lot.

Modern software today is typically very slow, often very unstable and use a ton of memory - without no reason.

There are multiple reasons for this, but the most common problem is that modern software is written in a way that is unhealthy for the CPU, meaning:

- Is uses data structures that can´t make use of the memory prefetch, due to non sequential reads or too large in size
- It uses a ton of memory, because temporary heap allocations are used inside methods/classes to make it more easier for the programmer
- Calling of codes that hides very expensive algorythmns, even though the name of the method or the documentation does not implies this
- Limited access to data structures by forcing the user to use special methods or properties for reading/writing data only
- Too many abstractions increasing the overhead
- Too much use of multi threading for no particular reason
- Waiting for a response, but doing nothing between
- Wrong usage of thread waiting / synchronisation primitives

And this boils down to four major reasons, why this is true for almost any kind of software nowadays:

- Most programmer does not care if their software is slow or unreliable
- Companies does not pay for making the software fast and reliable
- Code is written without any kind of target architecture in mind
- The programmer has no or limited knowledge how a CPU actually works
