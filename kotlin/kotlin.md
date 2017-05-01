slidenumbers: true
footer: Drew Stephens • <drew@dinomite.net> • [github.com/dinomite/talks](https://github.com/dinomite/talks)

#[fit] Kotlin
[.hide-footer]

Drew Stephens
@dinomite
\<drew@dinomite.net\>

^Software engineer; Forum for All, chat to websites

---

# What do I know?

- JVM (Java, Groovy, Kotlin)
- Ruby & Rails
- Perl and Regexes
- PHP (5.3/5.4)
- JavaScript when I have to
- Android & iOS in a pinch

---

# What is it?

- A language for the JVM
- As easy to integrate as Groovy
- Syntax like Java, but cleaner
- Written by JetBrains, who wrote your IDE[^1]

[^1]: If you use the [correct IDE](https://www.jetbrains.com/idea/)

^Last point is important because it's well supported

---

#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————

---

# Hate Java

<br>

# Don't hate types

^I'll argue that many developers who think static typing is bad really just think Java is bad

^Lots of Rails comparisons because it encourages an object oriented style and project structure similar to Java

---

Static      | Dynamic
---         | ---
Java        | Ruby
C#          | Python
ObjectiveC  | JavaScript
Swift       | Perl
Kotlin      | PHP
Groovy      | Groovy 🤷

^All of those dynamic langauges have JVM implementations

^Conflating strong vs. weak typing here

---

# Statics, generally

- No misspelled variables
- Unit tests cover functionality, not borders
- Easy refactoring, thanks to powerful tools
- Hard to break things accidentally
- No tracking down parameters
- Much less reading the source APIs you're using

^In Ruby, much of our unit tests are covering things coming and going from a functions.  Defined types obviate this.

---

# Param checking in Ruby

```ruby
def check_params
    has_required_params = params.key?(:site_rule) && params.key?(:test_url)
    fail ActionController::ParameterMissing unless has_base_params

    site_rule = params[:site_rule]

    fail ActionController::ParameterMissing unless
        site_rule.key?(:domain) && site_rule.key?(:action) && site_rule.key?(:regex)
end

def test_rule
    params[:site_rule][:domain]
    ...
end
```

^Manually check params; potential to mis-spell in check or in method

---

# Param checking in Kotlin

```kotlin
// Domain types
data class SiteRule(val domain: String, val action: Action, val regex: Regex)
enum class Action { BLACKLISTED, TRANSMOGRIFY_URL, REPLACE, DOWNCASE }

// Specific to controller
data class SiteRuleTest(val siteRule: SiteRule, val testUrl: String)

fun testRule(toTest: SiteRuleTest): Boolean {
    // Guaranteed to be well-formed
    toTest.siteRule.domain
    ...
}
```

^Params automatically checked, can use the types elsewhere

---

# Kotlin, specifically

- No nulls (if you embrace it)

# vs. Java

- No primitive types
- Type inference lessens the burden

---

#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————

---

# Hello, world! (Java)

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

^A simple Hello World in Java, with slight embellishment

---

# Hello, World!

```kotlin
package demo

fun main(args : Array<String>) {
  println("Hello, world!")
}
```

^Kotlin is cleaner and more concise than Java
No semicolons, default visibility is public, no void return type, static replaced by top-level functions

---

#[fit] No
#[fit] more
#[fit] checked
#[fit] exceptions

^Kotlin doesn't have checked exceptions

---

# Variables

- `val`s are read-only (like `final` in Java)
- `var`s are mutable
- Type inference

```kotlin
val foo = "Foo" // is a String
```

^Using final in Java provides guarantees for threading; immutable by default

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

# Better streams syntax

```kotlin
val cars = mapOf(
            2 to "Vandoorne", 3 to "Ricciardo",
            5 to "Vettel", 7 to "Räikkönen",
            …
        )
val qualified = listOf(44, 5, 77, 7, 33, …)
val grid = qualified.map { cars[it] }
// Java:   qualified.stream().map(e -> cars.get(e))

// [Hamilton, Vettel, Bottas, Räikkönen, …]
```

^Note Kotlin's default `it`, map index access

---

# Class visibility

- Default public
- Default final


---

# POJO

```java
public class Pagechat {
    private final int id;
    private final String url;
    private final boolean fresh;

    public Pagechat(int id, String url, boolean fresh) {
        this.id = id;
        this.url = url;
        this.fresh = fresh;
    }

    public int getId() { return id; }
    public String getUrl() { return url; }
    public boolean getFresh() { return fresh; }

    public boolean equals(Object obj) { … }
    public int hashCode() { … }
    public String toString() { … }
}
```

---

# Data class (POJO equivalent)

```kotlin
data class Pagechat(val id: Int,
                    val url: String,
                    val fresh: Boolean)
```

Free stuff:
- Accessors for fields; setters for `var`s
- `equals()`/`hashCode()`
- `toString()`
- `copy()`

^Like Scala case classes
Free copy constructor is great—change a few attributes but get a new object

---

# Using the copy constructor

```kotlin
data class Pagechat(val id: Int, val url: String, val fresh: Boolean) {
    constructor(url: String, fresh: Boolean) : this(0, url, fresh)
}

...


fun createPagechat(url: String, fresh: Boolean) {
    val pagechat = Pagechat("http://foobar.com", true)
    val newId = dao.insert(pagechat)
    return pagechat.copy(id = newId)
}
```

^Create an object, then insert in to DB to get ID

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

# Multiple classes per file

```kotlin
@Path("/v{version:[1]}/users")
@Produces(MediaType.APPLICATION_JSON)
class UserResource {
    …
}

data class UpdateLocationRequest(val ip: String)
data class RegisterDeviceRequest(val token: String, val type: DeviceId.Type)
data class PushNotificationRequest(val title: String, val body: String, …)
```

^Kotlin allows multiple classes per file—great for small request classes

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

class Trump: President(HandSize.SMALL) {
    override fun showTie(): String { return "Just too long" }
}
```

^Single-class-per-file is a good model most of the time, but lifting that restriction can really help organization

---

# Simpler lambdas

```java
takesLambdaArg(e -> e.getValue())
```

```kotlin
takesLambdaArg({ it.getValue() })
```

---

# String interpolation

```kotlin
val foo = "foobar"
println("The variable is $foo")
// The variable is foobar
```

---

#[fit] How do I use it?

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

**Drew**
@dinomite on Twitter
drew@dinomite.net
http://dinomite.net
https://github.com/dinomite
