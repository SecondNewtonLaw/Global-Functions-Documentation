# 🔨 sUNC Function Documentation

If you're running short on time or do not like reading AT ALL, you can read the condensed version of this README [here](./README-condensed.md).

## What is sUNC? 🤔

**sUNC**, short for **senS' Unified Naming Convention**, is a testing tool designe d to validate an executor's ability to implement functions under the Unfied Naming Convention (UNC). Unlike the original UNC, which is now deprecated and prone to spoofing, sUNC rigorously evaluates function implementations to ensure full functionality and authenticity.

**sUNC is a testing script, *NOT* another underdeveloped naming convention.**

### Key Features

- sUNC has a focus on **validation**: Tests are designed to verify that functions operate correctly, regardless of whether an executor is internal or external.
- sUNC enforces **transparency** and **accountability**: The script exposes fake, spoofed, or partially implemented functions to protect users from misleading claims.
- sUNC is constantly updated with new tests and refinements, ensuring it remains as good as gold for testing executor validity.

-----

## Addressing a few misconceptions 🧙

Again, **sUNC is *NOT* a Naming Convention**.

sUNC is a **testing script**, *not* a naming convention. Its purpose is to validate function implementations under the exsiting UNC standards, not to define these standards or to create new ones.

> [!WARNING]
> TODO: clarify that we are not redefining UNC and that this is not a new convention just because we test a few additional functions
> only some additional functions like decompile(), and restorefunction(), etc are added because they are highly requested and see high usage within the exploiting community / popularity

### Internal vs. External Executors

There is no difference in how functions should behave between internal and external executors. sUNC evaluates functionality based on expected behaviour, not on whether an executor is internal or external. Claims that suggest otherwise often come from executor developers who try to make excuses for their sub-par implementations.

-----

## Common Issues exposed by sUNC 🕵️‍♀️

### Misimplementation of Functions

Many executors fail to implement certain functions correctly or resort to even faking/spoofing their results.

Here are some examples:

- `getcustomasset()`: Often spoofed by returning a string rather than implementing the function properly. Only a few executors (e.g. Synapse Z, Wave, Solara) have been observed to implement this function correctly.
- `getinstances()`: Often misimplemented to just return `game:GetDescendants()`, which fails to include objects outside the game environment, contrary to the function's intended behaviour.
- `getrunningscripts()`: Often incorrectly coded to simply return every LocalScript in the game, rather than actually returning only those that have actively running threads.

### Implementing functions in Lua! 😡

Many functions, such as `newcclosure` and `setreadonly`, are poorly implemented because some people decide to implement them in Lua instead of C. Here are some examples:

- `newcclosure()`: Often created using coroutines or just wrapping a function inside of another function, which is horribly incorrect.
- `setreadonly()`: Commonly hacked together by just rewriting the table library or creating spoofed environments... (the lengths that people will go to implement face functions is crazy)

> [!WARNING]
> TODO: clarify that you should be implementing sUNC in Lua as MINIMALLY as possible - most functions should be implemented in C rather than lua because you will have more granularity.

### Spoofed WebSocket Functions

Original UNC tests for WebSocket functions are often bypass with filler implementations. sUNC tests these WebSocket functions properly and exposes any issues.

### Thread Identity (`setthreadidentity` ans `getthreadidentity`)

Some exploits spoof these functions:

- `setthreadidentity()`: Commonly implemented to simply increment a number variable without actually setting the thread identity.
- `getthreadidentity()`: Often spoofed to just return the executor's identity number (or the variable that was modified by `setthreadidentity`) instead of the thread's actual identity.

-----

## Why do bad executor owners dislike sUNC? 😹

Some executor owners may criticise sUNC with baseless claims because it highlights the shortcomings of their exploit:

- They label sUNC as "faulty" or "non-functional" when *their* functions fails its tests.
- Many rely on spoofing or partial implementations to pass simpler UNC tests and avoud using sUNC due to its stricter validation.

-----

## Testing Methodology

sUNC uses multiple tests for each function to properly ensure functionality. Some functions rely on others to be tested properly:

- **FileSystem Functions** defepd on nearly every other function in the category to work together.
- `loadstring` requires `getscriptbytecode` for basic functionality tests.

-----

## TL;DR - In conclusion, ...

- Avoid implementing functions in Lua unless absolutely necessary
- Do not fake functions. Spoofing is just embarassing and it ultimately damages the reliability and reputation of your executor.
- If you cannot fully implement a funciton, it is better to leave it out than to provide a half-baked function or misleading implementation. Fully implementing sUNC is hard - and we understand! - just leave the function out and be honest, you will be respected. You can always come back later!
- Normalise using sUNC for testing. UNC is outdated, the documentation is very vague, and the test is highly spoofable. sUNC ensures transparency and validates functionality comprehensively.

sUNC is an evolving project and we aim to raise the standards of executor development and call out those who are dishonest. If your functions fail sUNC tests, then the problem lies within your implementation - not in sUNC.

-----

# Credits

- [Original UNC Documentation](https://github.com/unified-naming-convention/NamingStandard/tree/main)
