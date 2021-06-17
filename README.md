# KotlinNote
Note for Kotlin learning
# Table of Contents
1. [What is Kotlin](#what)
2. [Why you should use Kotlin](#why)
    * [Compatible with Java](#compatible)
    * [Suitable with and easy to use in Spring Boot](#suitable)
    * [Simple to transfor from Java using IntelliJ with ignorable effort](#transfor)
    * [Type inference & Smart cast](#typeinference)
    * [NO semicolon](#semicolon)
    * [Null pointer check](#null)
    * [More powerful Lambda expression](#lambda)
3. [Useful Features](#features)
    * [Elvis operator](#elvis)
    * [Variable var & val](#variable)
    * [String format ${}](#string)
    * [Difference in String method implement with Java](#diffstring)
    * [Mutable collection](#mutable)
    * [Data class](#data)
    * [Enum & sealed class](#enum)
    * [And more ...](#more)
4. [References](#references)
# What is Kotlin <a name="what"></a>
![Kotlin](img/kotlin.PNG)
> Keywords: Cross-platform with JVM, Static typed programming language, Combines the features of OOPs and functional-oriented programming.

Kotlin (/ˈkɒtlɪn/) is a **cross-platform, statically typed**, general-purpose programming language with type inference. Kotlin is designed to interoperate fully with Java, and the JVM version of Kotlin's standard library depends on the Java Class Library, but type inference allows its syntax to be more concise. Kotlin mainly targets the **JVM**, but also compiles to JavaScript (e.g. for frontend web applications using React) or native code (via LLVM), e.g. for native iOS apps sharing business logic with Android apps. Language development costs are borne by JetBrains, while the Kotlin Foundation protects the Kotlin trademark.

On 7 May 2019, Google announced that the Kotlin programming language is now its preferred language for Android app developers. As a result many developers have switched to Kotlin Since the release of Android Studio 3.0 in October 2017, Kotlin has been included as an alternative to the standard Java compiler. The Android Kotlin compiler targets Java 6 by default, but lets the programmer choose to target Java 8 up to 13, for optimization, or more features.

> Though in this note, we more focus on Kotlin applying in **Web Service**.

In Stack Overflow Annual Developer Survey 2020, Kotlin got 4th in developers most loved language, 6th in most wanted, and 13th in most popular.

> Most Loved
> ![Most Loved](img/most_loved.PNG)

> Most Wanted
> ![Most Wanted](img/most_wanted.PNG)

> Most Popular
> ![Most Popular](img/most_popular.PNG)

And the size of Kotlin communities in Q3, 2020 ranked 9th.
![Community Size](img/community_size.PNG)

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
   <!-- this library include kotlin-stdlib & kotlin-reflect -->
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
    <dependencies>
        <dependency>
           <groupId>org.jetbrains.kotlin</groupId>
           <artifactId>kotlin-maven-allopen</artifactId>
           <version>${kotlin.version}</version>
        </dependency>
        <dependency>
           <groupId>org.jetbrains.kotlin</groupId>
           <artifactId>kotlin-maven-noarg</artifactId>
           <version>${kotlin.version}</version>
        </dependency>
    </dependencies>
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
With the strong function of IntelliJ, we can leverage it to convert Java file to Kotlin code.

![Convert to Kotlin](img/convert_to_kotlin.PNG)

After conversion, we almost no need to do anything. While sometimes to fix the complilation error, we need to modify the null safety (add `!!`, `?`, or other null value check), getter and setter (in Kotlin, `object.getSomething()` is simplify as `object.something`).

## Type inference & Smart cast <a name="typeinference"></a>
Kotlin is a **statically type language**. Regarding to that specification, that means that the compiler will know what is the type of a variable or the return type of a function. That's why you can write `val str = "hello"` instead of `val str: String = "hello"`, or even `fun sum(ints: List<Int>) = ints.sum()` instead of `fun sum(ints: List<Int>): Int = ints.sum()`. In a lot of cases you will be allowed to omit the declaration type, so your code should be more concise.

``` kotlin
val str: String = "Hello"
val str = "Hello" // same as above, but more concise
```

There are two kinds of *type inference* supported by Kotlin.
1.	*Local type inference*: for inferring types of expressions locally, in statement/expression scope.
2.	*Function signature type inference*: for inferring types of function return values and/or parameters.

### Smart cast

But before type inference, we need to talk *smart cast* first.

``` kotlin
// Note: the most important exception when smart casts are used in type inference is direct property declaration.

fun noSmartCastInInference() {
   var a: Any? = null
   if (a == null) return
   var c = a // Direct property declaration

   c // Declared type of `c` is Any?
     // However, here it’s smart casted to Any
}

fun <T> id(a: T): T = a

fun smartCastInInference() {
   var a: Any? = null
   if (a == null) return
   var c = id(a)
   
   c // Declared type of `c` is Any
}
```
### Local Type Inference

Local type inference in Kotlin is the process of deducing the compile-time types of expressions, lambda expression parameters and properties.

Type inference in Kotlin is **bidirectional**; meaning the types of expressions may be derived not only from their arguments, but from their usage as well. Note that, albeit bidirectional, this process is still local, meaning it processes one statement at a time, strictly in the order of their appearance in a scope; e.g., the type of property in statement S1 that goes before statement S2 cannot be inferred based on how S1 is used in S2.

### Function Signature Type Inference

Function signature type inference is a variant of local type inference, which is performed for *[function declarations], lambda literals and anonymous function declarations*.

Named and anonymous function declarations

``` kotlin
fun <T> foo(): T { … }
fun bar(): Int = foo() // an expected constraint T’ <: Int
// allows the result of `foo` to be infereed automatically.
```

Statements with lambda literals

``` kotlin
fun <T> foo(): T { … }
fun <R> run(body: () -> R): R { … }

fun bar() {
   val x = run {
      run {
         run {
            foo<Int>() // last expression inferred to be of type Int
         } // this lambda is inferred to be of type () -> Int
      } // this lambda is inferred to be of type () -> Int
   } // this lambda is inferred to be of type () -> Int
   // x is inferred to be type Int

   val y: Double = run { // this lambda has an external constraint R’ <: Double
      run { // this lambda has an external constraint R’ <: Double
         foo() // this call has an external constraint T’ <: Double
               // allowing to infer T to be Double in foo
      }
   }
}
```


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
In Kotlin, there are `!!` and `?` operators, which are used for null safety. This feature can pre-check the codes to prevent NullPointerException in runtime caused by thoughtless programmer.

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
## More powerful Lambda expression <a name="lambda"></a>

There are many APIs in Kotlin Standard Library using Lambda function, even `String`, `Collection`, and so on.
![Lambda](img/lambda.PNG)

### Higher-Order Functions

Kotlin functions are **first-class**, which means that they can be stored in variables and data structures, passed as arguments to and returned from other higher-order functions. You can operate with functions in any way that is possible for other non-function variables.

Instantiating a function type
- Lambda Expression
- Anonymous Function

Lambda expressions and anonymous function are ‘function literals’, i.e. functions that are not declared, but passed immediately as an expression.

### Lambda Expression
A lambda expression is always surrounded by curly braces `{}`, parameter declarations in the full syntactic form go inside curly braces and have optional type annotations, the body goes after an `->` sign. If the inferred return type of the lambda is not `Unit`, the last (or possibly single) expression inside the lambda body is treated as the return value.

``` kotlin
// val action: (A, B) -> ReturnType
val plus: (Int, Int) -> Int = { n1, n2 -> n1 + n2 }
println("${plus(2, 3)}") // 5
```

Also, due to type inference, we can rewrite above to this:

``` kotlin
val plus = { n1: Int, n2: Int -> n1 + n2 }
println("${plus(2, 3)}") // 5
```

### Anonymous Function
One thing missing from the lambda expression syntax presented above is the ability to specify the return type of the function. In most cases, this is unnecessary because the return type can be inferred automatically. However, if you do need to specify it explicitly, you can use an alternative syntax: an anonymous function.

An anonymous function looks very much like a regular function declaration, except that its name is omitted. Its body can be either an expression (as shown above) or a block:

``` kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)

val result1 = numbers.filter(fun(number: Int): Boolean = number > 2) // body is an expression
println("$result1") // [3, 4, 5]

val result2 = numbers.filter(fun(number: Int): Boolean { return number > 2 }) // body can also be a block
println("$result2") // [3, 4, 5]
```

The parameters and the return type are specified in the same way as for regular functions, except that the parameter types can be omitted if they can be inferred from context:

``` kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)

val result1 = numbers.filter(fun(number): Boolean = number > 2) // body is an expression
println("$result1") // [3, 4, 5]

val result2 = numbers.filter(fun(number): Boolean { return number > 2 }) // body can also be a block
println("$result2") // [3, 4, 5]
```


# Useful Features <a name="features"></a>
## Elvis operator <a name="elvis"></a>

The name *Elvis operator* refers to the fact that when its common notation. `?:`, is viewed sideways, it resembles an emoticon of Elvis Presley with his quaff.

> But it also looks like Giorno Giovanna in JOJOm haha.

![Elvis Operator](img/elvis_operator.PNG)

This operator handle two states.
1.	If the first operand is NOT null, it will return the first operand
2.	If the first operand is null, then return the second operand

``` kotlin
val firstOperand1: String? = "Non-null String"
val firstOperand2: String? = null
val secondOperand: String = "Non-null String 2"

val result1 = firstOperand1 ?: secondOperand // result1 = "Non-null String"
val result2 = firstOperand2 ?: secondOperand // result2 = "Non-null String 2"
```

## Variable var & val <a name="variable"></a>

When the variable shouldn't be modified, it should be `val`, otherwise will be `var`.

``` kotlin
val constVal = "CONST_VALUE" // similar with `final` in Java
var canModifyVal = "This value can be modified."
```

## String format ${} <a name="string"></a>

If we put aside `StringBuilder`, we may concate String in Java like this:

``` java
String hello = "Hello";
String world = "World";
String helloWorld = hello + " " + world + "!!"; // "Hello World!!"
```

In Kotlin, we have `$` operator to do this concisely.

``` kotlin
val hello = "Hello"
val world = "World"
val helloWorld = "$hello $world!!" // "Hello World!!"
```

Also, it implicit `toString()` method. And if we want to use properties or functions in `$` it can be wrapped in `{}`.

``` kotlin
val strs = mutableListOf("Hello", "World", "!!")
println("str list: $strs, list size: ${strs.size}") // "str list: [Hello, World, !!], list size: 3"
```

## Difference in String method implement with Java <a name="diffstring"></a>

There are some implement of String methods in Kotlin may be different from Java.

String split – Java vs. Kotlin vs. apache StringUtils
``` kotlin
val str = "A / B C /"
(str as java.lang.String).split(" /")           // ["A", " B C"] : String[]
(str as kotlin.String).split(" /")              // ["A", " B C", ""] : List<String>
(str as kotlin.String).split(" /".toRegex())    // ["A", " B C", ""] : List<String>
StringUtils.split(str, " /")                    // ["A", "B", "C"] : String[]
StringUtils.splitByWholeSeparator(str, " /")    // ["A", " B C", ""] : String[]
```

In Java Official API document, you can find out `public String[] split(String regex, int limit)`. The `limit` is 0 as default, and the pattern will be applied as many times as possible, the array can have any length, and trailing empty strings will be discarded. In other words, only if `limit = 0`, Java String split will trim the empty strings. Otherwise, Kotlin String split is smoother on `limit` parameter.
``` java
String str = "a:b:c:d:";
str.split(":", limit);
// limit
// limit < 0 -> ["a", "b", "c", "d", ""]
// limit = 0 -> ["a", "b", "c", "d"] => special case
// limit = 1 -> ["a:b:c:d:"]
// limit = 2 -> ["a", "b:c:d:"]
// limit = 3 -> ["a", "b", "c:d:"]
// limit = 4 -> ["a", "b", "c", "d:"]
// limit = 5 -> ["a", "b", "c", "d", ""] (goes on the same as with limit < 0)
// limit = 6 -> ["a", "b", "c", "d", ""]
```

## Mutable collection <a name="mutable"></a>

Unlike many languages, Kotlin distinguishes between **mutable** and **immutable** collections (lists, sets, maps, etc). Precise control over exactly when collections can be edited is useful for eliminating bugs, and for designing good APIs.

``` kotlin
val numbers = listOf(1, 2, 3, 4, 5)
numbers.add(6) // Compile error - Unresolved reference: add
```

If the collection need to be able to edit, we should use `Mutable`

``` kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)
numbers.add(6) // [1, 2, 3, 4, 5, 6]
```

Same as `MutableMap`, `MutableSet`.


## Data class <a name="data"></a>

In Java, we usually use IDE to generate POJO, e.g.,
- generate Getters and Setters
- generate hashCode() and equals()
- generate toString()

Although there are some libraries like Lombok, can use annotation to simplify the process.

``` java
@Getter
@Setter
@EqualsAndHashCode
@ToString
public class User {
   private int id;
   private String name;
}
```

And finally can become

``` java
@Data
public class User {
   private int id;
   private String name;
}
```

In Kotlin, we can easly get this by `data class`.

``` kotlin
data class User (
   var id: int
   var name: String
) {}

// setter
user.id = 123
user.name = "Tom"

// getter
println("${user.id} / ${user.name}") // "123 / Tom"
```

## Enum & sealed class <a name="enum"></a>

Sealed classes are used for representing restricted class hierarchies, when a value can have one of the types from a limited set, but cannot have any other type. They are, in a sense, **an extension of enum classes**: the set of values for an enum type is also restricted, but each enum constant exists only as a single instance, whereas a subclass of a sealed class can have multiple instances which can contain state.

``` kotlin
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```

- A sealed class is abstract by itself, it cannot be instantiated directly and can have abstract members.
- Sealed classes are not allowed to have non-private constructors (their constructors are private by default).
- Note that classes which extend subclasses of a sealed class (indirect inheritors) can be placed anywhere, not necessarily in the same file.
- The key benefit of using sealed classes comes into play when you use them in a `when` expression. If it's possible to verify that the statement covers all cases, you don't need to add an `else` clause to the statement.

``` kotlin
fun eval(expr: Expr): Double = when(expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
    // the `else` clause is not required because we've covered all the cases
}
```

## And more ... <a name="more"></a>

There are more amazing features in Kotlin waiting for you to discover!

# References <a name="references"></a>
1. [Kotlin Proframming Language](https://kotlinlang.org/)
2. [Kotlin Language Spec](https://kotlinlang.org/spec/introduction.html)
3. [Kotlin Starter Pack - Type inference, checks, and smart casts](https://www.codingame.com/playgrounds/5529/kotlin-starter-pack/type-inference-checks-and-smart-casts)
4. [Kotlin Wiki](https://en.wikipedia.org/wiki/Kotlin_(programming_language))
5. [Elvis Operator](https://zh-tw.coderbridge.com/series/794d69188e074c5e81abedfcb649b809/posts/f84fef99ba074b16aa520c7f095fe737)
6. [長的帥，連Code都是香的 - Elvis Operator ?:](https://ithelp.ithome.com.tw/articles/10231089)
7. [Spring Boot and Kotlin](https://www.baeldung.com/kotlin/spring-boot-kotlin)
8. [Create a Java and Kotlin Project with Maven](https://www.baeldung.com/kotlin/maven-java-project)
9. [初探 Kotlin Lambda 表達式](https://medium.com/@louis383/%E5%88%9D%E6%8E%A2-kotlin-lambda-%E8%A1%A8%E9%81%94%E5%BC%8F-cfe8796c9fac)
10. [Java vs. Kotlin: Lambdas and Functions](https://dzone.com/articles/java-vs-kotlin)
11. [Kotlin vs Java: What's the Difference?](https://www.guru99.com/kotlin-vs-java-difference.html)
12. [What are the rules of semicolon inference?](https://stackoverflow.com/questions/39318457/what-are-the-rules-of-semicolon-inference)
13. [Project Lombok](https://projectlombok.org/)
14. [Stack Overflow Annual Developer Survey 2020](https://insights.stackoverflow.com/survey/2020#overview)
15. [SlashData State of the Developer Nation Q3 2020 Report](https://slashdata-website-cms.s3.amazonaws.com/sample_reports/y7fzAZ8e5XuKCL1Q.pdf)
