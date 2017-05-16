slidenumbers: true
footer: Drew Stephens • \<drew@dinomite.net\> • [github.com/dinomite/talks](https://github.com/dinomite/talks)

#[fit] Kotlin
[.hide-footer]

Drew Stephens
\<drew@dinomite.net\>
[@dinomite](https://twitter.com/dinomite)

[Forum For All](https://forumforall.net)

^Software engineer; Forum for All, chat to websites

---

# What do I know?

- JVM (Java<sup>1.4–8</sup>, Kotlin<sup>M14–1.1</sup>, Groovy<sup>1.5</sup>)
- Ruby<sup>1.9–2.4</sup> & Rails<sup>3.0–5.0</sup>
- Perl<sup>5.5–5.12</sup>
- PHP<sup>5.2–5.3</sup>
- JavaScript<sup>1.5–ECMAScript 6/ES2016/ES6 Harmony/WTF</sup> (when I have to)
- Android & iOS in a pinch

^Version numbers for understanding of where I'm coming from
^I'll make references to Groovy, but that was pre-2.0 (which added static compilation)
^I'm not a language nerd (don't ask me about Agda or even Haskell)

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
#[fit] Statics
#[fit] ———————————
#[fit] ———————————

---

# Hate Java

<br>

# Don't hate types

^I'll argue that many developers who think static typing is bad really just think Java is bad

^Lots of Rails comparisons because it encourages an object oriented style and project structure similar to Java

^Java is the flag bearer for staticly typed languages

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

```kotlin
int foo = 7;
foo = "bar"; // compile error
if (foo == "bar") { // compile error
    ...
}

if (fooo == 7) { //compile error
    ...
}
```

- Prevents misspelled variables
- Catch NoMethodError, NameError, undefined early

---

# Clarity

- Share your intent with computers and humans
- Reduce cognitive load

^When you're writing code, you have types in your head; they are part of the understanding

^Specifying those types helps you reduce the amount you need in your working memory

---

# Productivity

- Unit tests can cover functionality, not borders
- Easy refactoring, thanks to powerful tools
- Hard to break things accidentally
- Much less reading the source APIs you're using
- No tracking down parameters

^In Ruby, much of our unit tests are covering things coming and going from a functions.  Defined types obviate this.

---

# Param checking in Ruby

```ruby
def test_rule
    check_params

    params[:site_rule][:domain]
    ...
end

def check_params
    has_required_params = params.key?(:site_rule) && params.key?(:test_url)
    fail ActionController::ParameterMissing unless has_required_params

    site_rule = params[:site_rule]
    fail ActionController::ParameterMissing
        unless site_rule.key?(:domain) && site_rule.key?(:action) && site_rule.key?(:regex)
    // Doesn't verify types of site_rule bits
end
```

^Manually check params; potential to mis-spell in check or in method

---

# Param checking in Kotlin

```kotlin
@Consumes(MediaType.APPLICATION_JSON)
fun testRule(toTest: SiteRuleTest): Boolean {
    // Guaranteed to be well-formed
    toTest.siteRule.domain
    ...
}

// Specific to controller
data class SiteRuleTest(val siteRule: SiteRule, val testUrl: String)

// Domain types
data class SiteRule(val domain: String, val action: Action, val regex: Regex)
enum class Action { BLACKLISTED, TRANSMOGRIFY_URL, REPLACE, DOWNCASE }
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
#[fit] Features
#[fit] ———————————
#[fit] ———————————

---

# Hello, world! (Java)

```java
package demo;

public static void main(Array<String> args) {
    try {
        System.out.printf("Hello, world!");
    } catch (IllegalFormatException e) {
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
// Java
// final String foo = "Foo";
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

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^Finally, the Elvis operator makes returning a default instead of null easy

---

# Null tracking

```kotlin
if (maybeNull != null && maybeNull.length > 5) {
    // maybeNull guaranteed to not be
    print("It is ${maybeNull.length} characters")
} else {
    print("The variable is null 😢")
}
```

^Null values are easy to handle because the compiler will track their nullibility

---

# Embracing null[^2]

```kotlin
fun findUser(id: Int): User?

...

findUser(7)?.let {
    // Only executes if user exists
    it.transmogrify(42)
}
```

[^2]: http://beust.com/weblog/2016/01/14/a-close-look-at-kotlins-let/

---

# Type inference

```kotlin
fun returnsAListOfStrings(): List<String> {
    return listOf("foo", "bar")
}

// ...

val listOfStrings = returnsAListOfStrings()
val firstString = listOfStrings[0]

// Java
// final List<String> listOfStrings = returnsAListOfStrings();
// final String firstString = listOfStrings[0];
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
...
val grid = qualified.map { cars[it] }
// Java:   qualified.stream().map(e -> cars.get(e))

// [Hamilton, Vettel, Bottas, Räikkönen, …]
```

^Note Kotlin's default `it`, map index access

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

fun createPagechat(url: String, fresh: Boolean): Pagechat {
    val pagechat = Pagechat("http://foobar.com", true)
    // { id: 0, url: "http://foobar.com", fresh: true }
    val newId = dao.insert(pagechat)
    return pagechat.copy(id = newId)
    // { id: $newId, url: "http://foobar.com", fresh: true }
}
```

^Functional programming (immutability)

^Create an object, then insert in to DB to get ID

---

# Better `switch` statement (`when`)

```kotlin
when (view) {
    is TextView -> toast(view.text)
    is RecyclerView -> toast("Item count = ${view.adapter.itemCount}")
    is SearchView -> toast("Current query: ${view.query}")
    else -> toast("View type not supported")
}
```

^Example from Android; note auto-casting

---

# Better try-with-resources

```kotlin
OutputStreamWriter(r.getOutputStream()).use {
    // by `it` value you can get your OutputStreamWriter
    it.write('a')
}

// Java
// try (OutputStreamWriter it = r.getOutputStream()) {
//     it.write('a')
// }
```

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

^Single-class-per-file is a good model most of the time, but lifting that restriction can really help organization

^Kotlin allows multiple classes per file—great for small request classes

---

# Named arguments & defaults

```kotlin
data class User(name: String, email: String, admin: Boolean = false)

...

val drew = User("Drew", "drew@dinomite.net")
// drew.admin is false

val plague = User("The Plague", "babbage@ellingson.com", true)
// plague.admin is true

val named = User(email = "foo@bar.com", name = "Foo Bar")
```

---

# Tail recursion[^4]

```kotlin
tailrec fun findFixPoint(x: Double = 1.0): Double {
    val cosine = Math.cos(x)
    if (x == cosine) {
        return x
    } else {
        return findFixPoint(cosine)
    }
}
```

```kotlin
// Shorter
tailrec fun findFixPoint(x: Double = 1.0): Double
        = if (x == Math.cos(x)) x else findFixPoint(Math.cos(x))
```

[^4]: https://kotlinlang.org/docs/reference/functions.html#tail-recursive-functions

---

#[fit] ———————————
#[fit] ———————————
#[fit] How?
#[fit] ———————————
#[fit] ———————————

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

apply plugin: 'kotlin'
```

^If you're still using Maven, rethink your life choices

---

**Tell me more**
- Kotlin docs—[https://kotlinlang.org/docs/reference/]()
- Try Kotlin in your browser—[http://try.kotlinlang.org/]()
- My Kotlin repos—[https://github.com/dinomite](https://github.com/dinomite)
- Dynamic vs Static—[https://youtu.be/Uxl_X3zXVAM](https://youtu.be/Uxl_X3zXVAM)

**Drew Stephens**
\<drew@dinomite.net\>
[@dinomite](https://twitter.com/dinomite)

[https://forumforall.net](https://forumforall.net)

---

#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————
#[fit] ———————————

---

# JSON[^3]

```json
{
    "id": 7,
    "email": "drew@dinomite.net"
}
```

```kotlin
data class User(id: Int, email: String)
```

[^3]: https://medium.com/square-corner-blog/kotlins-a-great-language-for-json-fcd6ef99256b

^Easy to create data model and get type safety

---

# Simpler lambdas

```java
takesLambdaArg(e -> e.getValue())
```

```kotlin
takesLambdaArg({ it.getValue() })
```

^Debatable

---

# String interpolation

```kotlin
val foo = "foobar"
println("The variable is $foo")
// The variable is foobar
```

^Just a small improvement over Java; there are many of these

