# csharp-performance-aware-comparison
A C#/.NET project showing how modern programming techniques hurt performance and especially memory a lot.

## Reasons by todays software is very slow

Modern software today is typically very slow, often very unstable and use a ton of memory - without no reason.

There are multiple reasons for this, but the most common problem is that modern software is written in a way that is unhealthy for the CPU, meaning:

- Is uses data structures that can´t make use of the memory prefetch, due to non sequential reads or too large in size
- It uses a ton of memory, because temporary heap allocations are used inside methods/classes to make it more easier for the programmer
- Calling of codes that hides very expensive algorythmns, even though the name of the method or the documentation does not implies this
- Limited access to data structures by forcing the user to use special methods or properties for reading/writing data only
- Too many abstractions that increases the overhead/waste a lot
- Too much use of multi threading for no particular reason
- Let the user wait for a response, but doing nothing between
- Wrong usage of thread waiting / synchronisation primitives
- Use of third party libraries that are not performance aware too
- Code is written without any kind of target architecture in mind
- Writing code for processing one element, even though it is used for multiple elements

And this boils down to three major reasons, why this is especially true for almost any kind of software nowadays:

1. Most programmer does not care if their software is slow or unreliable
2. Companies does not pay for making the software fast and reliable
3. The programmer has no or limited knowledge how a CPU actually works

## We can do better

- Use sequential data structures that fits into CPU caches, so the CPU can use prefetching to improve performance a lot
- Write code to process multiple data at once, instead of processing one element standalone (Loop unrolling, increase IPC, use SIMD)
- Prevent heap memory allocations, use either area based or even stack allocated memory
- Prevent unnecessary abstractions
- Prevent the use of thread synchronisation, by using data that does not depend on another threads
- If possible, cache data and results from third party libraries, so that you dont need to call them very often
- At least do something useful when a user has to wait for a response
- Target your code to run on the slowest possible platform, e.g. quad-core x64 CPU with 3 GHz and 8 GB of memory or something like that

There is typically no need to do any kind of hardcore optimizations, because even doing this can result in 10x or even faster software.
Sometimes you need to use different algorythmn that can use more cache-friendly data structures.

## This project

This github project shows how to translate a real-world problem, that starts very slow, to be much faster and more efficient - in the same environment C# / .NET.

### The problem

- We want to normalize the volume for thousands of audio streams as past as possible, because we are writing the next spotify or something like that
- To normalize the audio samples for each stream, we need the minimum and maximum sample value
- Typical audio streams has a length from ~3 up-to ~10 minutes
- Each audio stream has N samples for 2 audio channels with 48 KHz, each sample is 16-bit signed integer
- Each audio sample has a fixed state that indicates if it is good or bad, so we have to only take valid samples into account
- A bad sample has a value of the highest possible number, for 16-bit signed integer this is +32767

So the worst case is we have 48000 Hz/s * 600 Seconds * 2 Channels = 57.600.000 samples per stream (total of 115.200.000 bytes)
So the best case is we have 48000 Hz/s * 180 Seconds * 2 Channels = 17.280.000 samples per stream (total of 34.560.000 bytes)

### Making the problem harder

- Each audio stream has N samples with at least one audio channel, up-to 8 audio channels, each sample is either 16-bit, 24-bit or 32-bit signed integer or 32-bit floating point
- Increase the length the of audio streams, by supporting audio pod-casts with a length up-to 1 hour
