---
title: Overview of New Features in Java 18
category: Java
tag:
  - New Features in Java
---

Java 18 was officially released on March 22, 2022, as a non-long-term support version.

Java 18 introduced 9 new features:

- [JEP 400: UTF-8 by Default](https://openjdk.java.net/jeps/400)
- [JEP 408: Simple Web Server](https://openjdk.java.net/jeps/408)
- [JEP 413: Code Snippets in Java API Documentation](https://openjdk.java.net/jeps/413)
- [JEP 416: Reimplement Core Reflection with Method Handles](https://openjdk.java.net/jeps/416)
- [JEP 417: Vector API](https://openjdk.java.net/jeps/417) (third incubation)
- [JEP 418: Internet-Address Resolution SPI](https://openjdk.java.net/jeps/418)
- [JEP 419: Foreign Function & Memory API](https://openjdk.java.net/jeps/419) (second incubation)
- [JEP 420: Pattern Matching for switch](https://openjdk.java.net/jeps/420) (second preview)
- [JEP 421: Deprecate Finalization for Removal](https://openjdk.java.net/jeps/421)

Java 17 contained 14 features, Java 16 had 17 features, Java 15 included 14 features, and Java 14 had 16 features. Compared to previously released versions, the number of new features in Java 18 is significantly less.

Here, I will provide a detailed introduction to a few important new features: 400, 408, 413, 416, 417, 418, and 419.

Related articles:

- [OpenJDK Java 18 Documentation](https://openjdk.java.net/projects/jdk/18/)
- [IntelliJ IDEA | Java 18 Feature Support](https://mp.weixin.qq.com/s/PocFKR9z9u7-YCZHsrA5kQ)

## JEP 400: UTF-8 by Default

The JDK has finally set UTF-8 as the default character set.

In Java 17 and earlier versions, the default character set was determined at runtime by the Java Virtual Machine, depending on various factors such as the operating system and locale settings, which posed potential risks. For example, a section of Java code that prints text to the console may work fine on a Mac but display garbled text on Windows unless the character set is manually changed.

## JEP 408: Simple Web Server

Starting with Java 18, you can use the `jwebserver` command to launch a simple static web server.

```bash
$ jwebserver
Binding to loopback by default. For all interfaces use "-b 0.0.0.0" or "-b ::".
Serving /cwd and subdirectories on 127.0.0.1 port 8000
URL: http://127.0.0.1:8000/
```

This server does not support CGI and Servlets and is limited to static files.

## JEP 413: Code Snippets in Java API Documentation

Prior to Java 18, if we wanted to introduce code snippets in Javadoc, we could use `<pre>{@code ...}</pre>`.

```java
<pre>{@code
    lines of source code
}</pre>
```

The effect generated by this method is relatively ordinary.

After Java 18, you can use the `@snippet` tag to accomplish this.

```java
/**
 * The following code shows how to use {@code Optional.isPresent}:
 * {@snippet :
 * if (v.isPresent()) {
 *     System.out.println("v: " + v.get());
 * }
 * }
 */
```

The `@snippet` method generates better effects and is easier to use.

## JEP 416: Reimplement Core Reflection with Method Handles

Java 18 improved the implementation logic of `java.lang.reflect.Method` and `Constructor` to enhance performance and speed. This change does not alter the related APIs, meaning developers do not need to modify reflection-related code to experience better performance.

OpenJDK provided performance benchmark results comparing the new and old implementations of reflection.

![Performance benchmark results for the new and old implementations of reflection](https://oss.javaguide.cn/github/javaguide/java/new-features/JEP416Benchmark.png)

## JEP 417: Vector API (Third Incubation)

The Vector API was initially proposed by [JEP 338](https://openjdk.java.net/jeps/338) and integrated into Java 16 as [Incubating API](http://openjdk.java.net/jeps/11). The second incubation, proposed by [JEP 414](https://openjdk.java.net/jeps/414), was integrated into Java 17, while the third round, proposed by [JEP 417](https://openjdk.java.net/jeps/417), was integrated into Java 18. The fourth round, proposed by [JEP 426](https://openjdk.java.net/jeps/426), will be integrated into Java 19.

Vector computation consists of a series of operations on vectors. The Vector API is used to express vector calculations, which can be reliably compiled at runtime to the best vector instructions supported by the CPU architecture, achieving performance superior to equivalent scalar computations.

The goal of the Vector API is to provide users with a concise, easy-to-use, and platform-independent way to express a wide range of vector computations.

This is a simple scalar computation on array elements:

```java
void scalarComputation(float[] a, float[] b, float[] c) {
   for (int i = 0; i < a.length; i++) {
        c[i] = (a[i] * a[i] + b[i] * b[i]) * -1.0f;
   }
}
```

This is the equivalent vector computation using the Vector API:

```java
static final VectorSpecies<Float> SPECIES = FloatVector.SPECIES_PREFERRED;

void vectorComputation(float[] a, float[] b, float[] c) {
    int i = 0;
    int upperBound = SPECIES.loopBound(a.length);
    for (; i < upperBound; i += SPECIES.length()) {
        // FloatVector va, vb, vc;
        var va = FloatVector.fromArray(SPECIES, a, i);
        var vb = FloatVector.fromArray(SPECIES, b, i);
        var vc = va.mul(va)
                   .add(vb.mul(vb))
                   .neg();
        vc.intoArray(c, i);
    }
    for (; i < a.length; i++) {
        c[i] = (a[i] * a[i] + b[i] * b[i]) * -1.0f;
    }
}
```

In JDK 18, the performance of the Vector API has been further optimized.

## JEP 418: Internet-Address Resolution SPI

Java 18 defines a new SPI (service-provider interface) for the resolution of main names and addresses, allowing `java.net.InetAddress` to utilize third-party resolvers outside the platform.

## JEP 419: Foreign Function & Memory API (Second Incubation)

Java programs can interoperate with code and data outside the Java runtime through this API. By efficiently invoking external functions (i.e., code outside the JVM) and safely accessing external memory (i.e., memory not managed by the JVM), this API enables Java programs to call native libraries and handle native data without the dangers and fragility associated with JNI.

The Foreign Function and Memory API underwent its first incubation in Java 17, proposed by [JEP 412](https://openjdk.java.net/jeps/412). The second incubation was proposed by [JEP 419](https://openjdk.java.net/jeps/419) and integrated into Java 18, while the preview was proposed by [JEP 424](https://openjdk.java.net/jeps/424) and integrated into Java 19.

In the [Overview of New Features in Java 19](./java19.md), I provided a detailed introduction to the Foreign Function and Memory API, so I will not go into further detail here.

<!-- @include: @article-footer.snippet.md -->
