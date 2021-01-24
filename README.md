# KotlinNote
Note for Kotlin learning
# Table of Contents
1. [What is Kotlin](#what)
2. [Why you should use Kotlin](#why)
    * [Compatible with Java](#compatible)
    * [Suitable with and easy to use in Spring Boot](#suitable)
    * [Simple to transfor from Java using IntelliJ with ignorable effort](#transfor)
    * [NO semicolon](#semicolon)
    * [Null pointer check](#null)
3. [Useful Features](#features)
    * [Elvis operator](#elvis)
    * [var & val](#var)
    * [String format ${}](#string)
    * [Mutable collection](#mutable)
    * [data class](#data)
    * [enum & sealed class](#enum)
    * [more powerful Lambda expression](#lambda)
    * [and more ...](#more)
4. [References](#references)
# What is Kotlin <a name="what"></a>
> Keywords: Cross-platform with JVM, Static typed programming language, Combines the features of OOPs and functional-oriented programming.

Kotlin (/ˈkɒtlɪn/) is a cross-platform, statically typed, general-purpose programming language with type inference. Kotlin is designed to interoperate fully with Java, and the JVM version of Kotlin's standard library depends on the Java Class Library, but type inference allows its syntax to be more concise. Kotlin mainly targets the JVM, but also compiles to JavaScript (e.g. for frontend web applications using React) or native code (via LLVM), e.g. for native iOS apps sharing business logic with Android apps. Language development costs are borne by JetBrains, while the Kotlin Foundation protects the Kotlin trademark.

On 7 May 2019, Google announced that the Kotlin programming language is now its preferred language for Android app developers. As a result many developers have switched to Kotlin Since the release of Android Studio 3.0 in October 2017, Kotlin has been included as an alternative to the standard Java compiler. The Android Kotlin compiler targets Java 6 by default, but lets the programmer choose to target Java 8 up to 13, for optimization, or more features.

> Though in this note, we more focus on Kotlin applying in Web Service.
# Why you should use Kotlin <a name="why"></a>
## Interoperable with Java <a name="compatible"></a>
We can use all the Java Libraries in Kotlin, and vice versa. Thus, in a Java project, we can partially add or step by step to transform the codes to Kotlin.

We have a Java library like this.
``` java
package sample.java.library;

public class HelloWorld {
   public void print() {
      System.out.println("Hello World!!");
   }
}
```
And we can directly import and use the class in Kotlin.
``` kotlin
import sample.java.library

class testHelloWorld {
   fun testHelloWorld() {
      val helloWorld = HelloWorld()
      helloWorld.print()   // will print "Hello World!!" in console
   }
}
```

## Suitable with and easy to use in Spring Boot <a name="suitable"></a>
We only need to config some dependencies and plugins in Maven POM.  
Assume that we already have some Maven configuration for Java project.

Dependencies
``` xml
<properties>
   <java.version>1.8</java.version>
   <kotlin.version>1.4.72</kotlin.version>
</properties>

<dependencies>
   <dependency>
      <groupId>org.jetbrains.kotlin</groupId>
      <artifactId>kotlin-stdlib-jdk8</artifactId>
      <version>${kotlin.version}</version>
   </dependency>
   <dependency>
      <groupId>org.jetbrains.kotlin</groupId>
      <artifactId>kotlin-reflect</artifactId>
   </dependency>
   <dependency>
      <groupId>com.fasterxml.jackson.module</groupId>
      <artifactId>jackson-module-kotlin</artifactId>
   </dependency>
...
</dependencies>
```
Plugins
``` xml
<plugin>
    <artifactId>kotlin-maven-plugin</artifactId>
    <groupId>org.jetbrains.kotlin</groupId>
    <version>${kotlin.version}</version>
    <executions>
        <execution>
            <id>compile</id>
            <goals>
                <goal>compile</goal>
            </goals>
            <configuration>
                <sourceDirs>
                    <sourceDir>${project.basedir}/src/main/kotlin</sourceDir>
                    <sourceDir>${project.basedir}/src/main/java</sourceDir>
                </sourceDirs>
            </configuration>
        </execution>
        <execution>
            <id>test-compile</id>
            <goals>
                <goal>test-compile</goal>
            </goals>
            <configuration>
                <sourceDirs>
                    <sourceDir>${project.basedir}/src/test/kotlin</sourceDir>
                    <sourceDir>${project.basedir}/src/test/java</sourceDir>
                </sourceDirs>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Simple to transform from Java using IntelliJ with ignorable effort <a name="transfor"></a>
## NO semicolon <a name="semicolon"></a>
In Kotlin, we don't need to use semicolon in the end of statement.  
Java
``` java
String str = "Hello";
String strAppend = str + " World!!";
```
Kotlin
``` kotlin
val str = "Hello"
val strAppend = "$str World!!"
```
We only need to add semicolon in these two cases:
1. An enum class that has a list of enums and also properties or functions
``` kotlin
enum class Things {
    ONE, TWO;
    
    fun isOne(): Boolean = this == ONE
}
```
2. Two statements in one line
``` kotlin
val str = "Hello"; println(str)
```

## Null pointer check <a name="null"></a>
In Kotlin, there are !! and ? operators, which are used for null safety. This feature can pre-check the codes to prevent NullPointerException in runtime caused by thoughtless programmer.

Regular initialization means non-null by default.
``` kotlin
var a: String = "abc" // Regular initialization means non-null by default
a = null // compilation error
```
If we need to declare a nullable variable.
``` kotlin
var b: String? = "abc" // can be set null
b = null // ok
print(b)
```
And if we consume that the variable won't be null.
``` kotlin
var b: String? = "abc" // can be set null
if (StringUtils.isEmpty(b)) {
   return
} else {
   doSomething(b!!) // if no "!!", it will has a compilation error
}

fun doSomething(b: String) { // this function only accept non-null parameter
   ...
}
```

# Useful Features <a name="features"></a>
## Elvis operator <a name="elvis"></a>
## type inference - var & val <a name="var"></a>
## String format ${} <a name="string"></a>
## Mutable collection <a name="mutable"></a>
## data class <a name="data"></a>
## enum & sealed class <a name="enum"></a>
## more powerful Lambda expression <a name="lambda"></a>
## and more ... <a name="more"></a>
# References <a name="references"></a>
1. [Kotlin Proframming Language](https://kotlinlang.org/)
2. [Kotlin Wiki](https://en.wikipedia.org/wiki/Kotlin_(programming_language))
3. [Elvis Operator](https://zh-tw.coderbridge.com/series/794d69188e074c5e81abedfcb649b809/posts/f84fef99ba074b16aa520c7f095fe737)
4. [Spring Boot and Kotlin](https://www.baeldung.com/kotlin/spring-boot-kotlin)
5. [Create a Java and Kotlin Project with Maven](https://www.baeldung.com/kotlin/maven-java-project)
6. [初探 Kotlin Lambda 表達式](https://medium.com/@louis383/%E5%88%9D%E6%8E%A2-kotlin-lambda-%E8%A1%A8%E9%81%94%E5%BC%8F-cfe8796c9fac)
7. [Java vs. Kotlin: Lambdas and Functions](https://dzone.com/articles/java-vs-kotlin)
8. [Kotlin vs Java: What's the Difference?](https://www.guru99.com/kotlin-vs-java-difference.html)
9. [What are the rules of semicolon inference?](https://stackoverflow.com/questions/39318457/what-are-the-rules-of-semicolon-inference)
