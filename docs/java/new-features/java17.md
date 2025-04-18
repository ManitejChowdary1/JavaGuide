---
title: Overview of New Features in Java 17 (Important)
category: Java
tag:
  - New Features in Java
---

Java 17 was officially released on September 14, 2021, and is a Long-Term Support (LTS) version.

The following image shows the timeline for Oracle JDK support provided by Oracle. As can be seen, Java 17 will be supported until September 2029 at the latest.

![](https://oss.javaguide.cn/github/javaguide/java/new-features/4c1611fad59449edbbd6e233690e9fa7.png)

Java 17 will be the most important Long-Term Support (LTS) version since Java 8, representing eight years of effort from the Java community. Spring 6.x and Spring Boot 3.x require at least Java 17.

This update brings a total of 14 new features:

- [JEP 306: Restore Always-Strict Floating-Point Semantics](https://openjdk.java.net/jeps/306)
- [JEP 356: Enhanced Pseudo-Random Number Generators](https://openjdk.java.net/jeps/356)
- [JEP 382: New macOS Rendering Pipeline](https://openjdk.java.net/jeps/382)
- [JEP 391: macOS/AArch64 Port](https://openjdk.java.net/jeps/391)
- [JEP 398: Deprecate the Applet API for Removal](https://openjdk.java.net/jeps/398)
- [JEP 403: Strongly Encapsulate JDK Internals](https://openjdk.java.net/jeps/403)
- [JEP 406: Pattern Matching for switch (Preview)](https://openjdk.java.net/jeps/406)
- [JEP 407: Remove RMI Activation](https://openjdk.java.net/jeps/407)
- [JEP 409: Sealed Classes (Standardized)](https://openjdk.java.net/jeps/409)
- [JEP 410: Remove the Experimental AOT and JIT Compiler](https://openjdk.java.net/jeps/410)
- [JEP 411: Deprecate the Security Manager for Removal](https://openjdk.java.net/jeps/411)
- [JEP 412: Foreign Function & Memory API (Incubating)](https://openjdk.java.net/jeps/412)
- [JEP 414: Vector API (Second Incubation)](https://openjdk.java.net/jeps/417)
- [JEP 415: Context-Specific Deserialization Filters](https://openjdk.java.net/jeps/415)

Here, I will provide a detailed introduction to the new features I consider important: 356, 398, 406, 407, 409, 410, 411, 412, and 414.

Related reading: [OpenJDK Java 17 Documentation](https://openjdk.java.net/projects/jdk/17/) .

## JEP 356: Enhanced Pseudo-Random Number Generators

Before JDK 17, we could generate random numbers using `Random`, `ThreadLocalRandom`, and `SplittableRandom`. However, these three classes each have their drawbacks and lack support for common pseudo-random algorithms.

Java 17 introduces new interface types and implementations for pseudo-random number generators (PRNGs), making it easier for developers to interchange various PRNG algorithms in their applications.

> [PRNG](https://ctf-wiki.org/crypto/streamcipher/prng/intro/) is used to generate sequences of numbers that are close to absolute randomness. Generally, a PRNG relies on an initial value, also known as a seed, to generate the corresponding pseudo-random number sequence. As long as the seed is determined, the random numbers generated by the PRNG are completely predictable, so the generated sequence is not truly random.

Example of usage:

```java
RandomGeneratorFactory<RandomGenerator> l128X256MixRandom = RandomGeneratorFactory.of("L128X256MixRandom");
// Use the current timestamp as the random seed
RandomGenerator randomGenerator = l128X256MixRandom.create(System.currentTimeMillis());
// Generate a random number
randomGenerator.nextInt(10);
```

## JEP 398: Deprecate the Applet API for Removal

The Applet API was used to write Java applets that run on the web browser side, and it has been obsolete for many years, with no reason to use it anymore.

The Applet API was marked as deprecated in Java 9 ([JEP 289](https://openjdk.java.net/jeps/289)), but not for removal.

## JEP 406: Pattern Matching for switch (Preview)

Just like `instanceof`, `switch` has also added automatic type matching functionality.

Example of \`instance
