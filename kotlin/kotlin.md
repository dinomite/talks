slidenumbers: true
footer: Drew Stephens <drew@dinomite.net>

# [fit] Kotlin
[.hide-footer]

Drew Stephens
@dinomite
\<drew@dinomite.net\>

^Software engineer; Forum for All, chat to websites

---

# What do I know?

- Perl and Regexes
- PHP (5.3/5.4)
- Ruby & Rails
- JVM (Java, Groovy, Kotlin)

---

# What is it?

- A language for the JVM
- As easy to integrate as Groovy
- Syntax like Java, but cleaner
- Written by JetBrains, who wrote your IDE[^1]

[^1]: If you use the correct IDE

^Last point is important because it's well supported (unlike Scala)

---

# Hello, World!

```kotlin
package demo

fun main(args : Array<String>) {
  println("Hello, world!")
}
```

<a name="kotlin-hello-world"/>
#### also in [Java](#java-hello-world)

^Kotlin is cleaner and more concise than Java

---

# Variables

- `val`s are read-only (like `final` in Java)
- `var`s are mutable

^Using final in Java provides guarantees for threading; immutable by default

---

# Data class

```kotlin
data class User(val id: Int,
                val name: String,
                val admin: Boolean)
```

<a name="kotlin-data-class"/>
#### also in [Java](#pojo)

^Data classes (like Scala case class; more later) are a great example of this simplicity

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"
```

^One of the most important differences is null safety

^Cleaner than option types

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
```

^We love typed languages because they give us errors early—Kotlin won't let you get into a situation where you could have an NPE

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK
```

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
```

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
```

^If you're really ok with returning null, Kotlin will let you, but you have to be explicit

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE
```

^You can also just be stupid

---

# Null safety

```kotlin
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrNegativeOne = maybeNull ?: -1
```

^Finally, the Elvis operator makes returning a default instead of null easy

---

# Null tracking

```kotlin
if (maybeNull != null && maybeNull.length > 5) {
    // maybeNull guaranteed to not be
    print("That's a long string!")
} else {
    print("The string was null 😢")
}
```

^Null values are easy to handle because the compiler will track their nullibility

---

# Type inference

```kotlin
fun returnsAListOfStrings(): List<String> {
    return listOf("foo", "bar")
}

// ...

val listOfStrings = returnsAListOfStrings()
val firstString = listOfStrings[0]
```

^The compiler can infer types

---

# forEach{} and map{} examples

---

# [fit] Checked exceptions?
<br>
# [fit] Ain't nobody got time for that

---

# Class visibility

- Default public
- Default final

---

# Data class (POJO equivalent)

```kotlin
data class Pagechat(val id: Int,
                    val url: String,
                    val fresh: Boolean)
```

Free stuff:
- `equals()`/`hashCode()`
- `toString()`
- `copy()`

^Free copy constructor is great—change a few attributes but get a new object

---

# DB example of copy constructor use

---

# Better enums

```kotlin
enum class Matcher(val cssQuery: String,
                   val attr: String) {
    OG_TITLE("meta[property=og:title]", "content"),
    META_TITLE("meta[name=title]", "content"),
    TITLE("title", null)
}
```

^Make string enum without looking it up on Stack Overflow

---

# Lambdas

```kotlin
enum class Matcher(val cssQuery: String,
                   val accessor: (Element?) -> String?) {
    OG_TITLE("meta[prop=og:t]", { it?.attr("content") }),
        ...
}

// Elsewhere
val image: String? = matcher.accessor.invoke(element)
```
<a name="kotlin-lambda"/>
#### also in [Java](#java-lambda)

---

# String interpolation

```kotlin
val foo = "foobar"
println("The variable is $foo")
```

---

# Webapp example of tiny classes in file

---

```kotlin
// Presidents.kt
package net.dinomite.pretend

abstract class President(val handSize: HandSize) {
    abstract fun showTie(): String

    enum class HandSize { SMALL, NORMAL }
}

class Obama: President(HandSize.NORMAL) {
    override fun showTie(): String { return "It's a normal tie" }
}

class Trump : President(HandSize.SMALL) {
    override fun showTie(): String { return "Just too long" }
}
```

^Single-class-per-file is a good model most of the time, but lifting that restriction can really help organization

---

# [fit] How do I use it?

---

# build.gradle

```groovy
buildscript {
    ext.kotlin_version = '1.1.1'
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

apply plugin: 'java'
apply plugin: 'kotlin'

dependencies {
    compile "org.jetbrains.kotlin:kotlin-reflect:$kotlin_version"
}
```

^If you're still using Maven, rethink your life choices

---

# Tell me more
Kotlin docs—https://kotlinlang.org/docs/reference/

Try Kotlin in your browser—http://try.kotlinlang.org/

# Drew
@dinomite on Twitter
drew@dinomite.net
http://dinomite.net

---




---

# Hello, world! (java)

```java
package demo;

public static void main(Array<String> args) {
    try {
        System.out.println("Hello, world!");
    } catch (HatefulException e) {
        e.printStackTrace();
    }
}
```
<a name="java-hello-world"/>
#### (note: slightly embellished) [Back to Kotlin](#kotlin-hello-world)

---

# POJO

```java
public class Pagechat {
    private int id;
    private String url;
    private boolean fresh;

    public Pagechat(int id, String url, boolean fresh) {
        this.id = id;
        this.url = url;
        this.fresh = fresh;
    }

    public int getId() { return id; }
    public String getUrl() { return url; }
    public boolean getFresh() { return fresh; }
}
```
<a name="pojo"/>
#### (note: condensed for presentation) [Oh god, take me back](#kotlin-data-class)

---

# Lambdas in Java

```java
public class Matcher {
    public final Function<Element, String> accessor;

    public Matcher(Function<Element, String> accessor) {
        this.accessor = accessor;
    }
}

// Elsewhere
Matcher matcher = new Matcher("meta[prop=og:t]", (e) -> e.attr("content")),
String value = matcher.accessor.apply(element)
```

<a name="java-lambda"/>
#### (note: condensed for presentation) [Oh god, take me back](#kotlin-lambda)
