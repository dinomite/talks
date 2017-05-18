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

^Version numbers

^Groovey pre-2.0

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

^First, an aside

---

# Hate Java

<br>

# Don't hate types

^Java is bad

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

if (fooo == 7) { // compile error
    ...
}
```

- Prevents misspelled variables
- Catch NoMethodError, NameError, undefined early

^Types are in your head; part of understanding

^Reduce cognitive load

---

# Equality

![inline](videos/static-equality.mov)

---

# Productivity

- Unit tests can cover functionality, not borders
- Easy refactoring, thanks to powerful tools
- Hard to break things accidentally
- Much less reading the source APIs you're using
- No tracking down parameters

^In Ruby, much of our unit tests are covering things coming and going from a functions.  Defined types obviate this.

---

# Updating a user's address

`POST /address`

```json
{
    "user_id": 7,
    "address": {
        "number": 2446,
        "street": "Belmont Rd NW",
        "zip": 20008
    }
}
```

^This is my friend's house

---

# Param checking in Ruby

```ruby
def update_address
    check_params
    ...
end

def check_address_params
    required_params = params.key?(:user_id) && params.key?(:address)
    fail ActionController::ParameterMissing unless has_required_params

    address = params[:address]
    fail ActionController::ParameterMissing
        unless address.key?(:number) && address.key?(:street) && address.key?(:zip)
    # Doesn't verify types of anything
end
```

^Manually check params; potential to mis-spell in check or in method

---

# Param checking in Java

```java
boolean updateAddress(UpdateAddress newAddress) {
    // Guaranteed to be well-formed
    ...
}

class UpdateAddress {
    int userId;
    Address address;
}

class Address {
    int number;
    String street;
    int zip;
}
```

---

# Param checking in Kotlin

```kotlin
fun updateAddress(newAddress: UpdateAddress): Boolean {
    // Guaranteed to be well-formed
    ...
}

data class UpdateAddress(val userId: Int, val address: Address )

data class Address(val number: Int, val street: String, val zip: Int)
```

^Params automatically checked, can use the types elsewhere

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

fun main(args: Array<String>) {
  println("Hello, world!")
}
```

^Kotlin is cleaner and more concise than Java

^No semicolons

^default public

^no void return type

^static replaced by top-level functions

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

```kotlin, [.highlight: 1-2]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^One of the most important differences is null safety

^Cleaner than option types

---

# Null safety

```kotlin, [.highlight: 4]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^We love typed languages because they give us errors early—Kotlin won't let you get into a situation where you could have an NPE

---

# Null safety

```kotlin, [.highlight: 5]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

---

# Null safety

```kotlin, [.highlight: 7]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

---

# Null safety

```kotlin, [.highlight: 8]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^If you're really ok with returning null, Kotlin will let you, but you have to be explicit

---

# Null safety

```kotlin, [.highlight: 9]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed OK

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^You can also just be stupid

---

# Null safety

```kotlin, [.highlight: 11]
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

# Type inference

```kotlin, [.highlight: 1-3]
fun returnsAListOfStrings(): List<String> {
    return listOf("foo", "bar")
}

// Java
// final List<String> listOfStrings = returnsAListOfStrings();
// final String firstString = listOfStrings[0];

val listOfStrings = returnsAListOfStrings()
val firstString = listOfStrings[0]
```

^The compiler can infer types

---

# Type inference

```kotlin, [.highlight: 5-7]
fun returnsAListOfStrings(): List<String> {
    return listOf("foo", "bar")
}

// Java
// final List<String> listOfStrings = returnsAListOfStrings();
// final String firstString = listOfStrings[0];

val listOfStrings = returnsAListOfStrings()
val firstString = listOfStrings[0]
```

^The compiler can infer types

---

# Type inference

```kotlin, [.highlight: 8-10]
fun returnsAListOfStrings(): List<String> {
    return listOf("foo", "bar")
}

// Java
// final List<String> listOfStrings = returnsAListOfStrings();
// final String firstString = listOfStrings[0];

val listOfStrings = returnsAListOfStrings()
val firstString = listOfStrings[0]
```

^The compiler can infer types

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
data class Pagechat(val id: Int, val url: String, val fresh: Boolean)

...

fun createPagechat(url: String, fresh: Boolean): Pagechat {
    val pagechat = Pagechat(0, "http://foobar.com", true)
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

^Note auto-casting

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
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.1.2"
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
