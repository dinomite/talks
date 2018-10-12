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

- JVM (Java<sup>1.4–8</sup>, Kotlin<sup>M14–1.2</sup>, Groovy<sup>1.5</sup>)
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

#[fit] Static
#[fit] vs.
#[fit] Dynamic

^First, an aside
^================
^================

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

```kotlin, [.highlight: 1]
int foo = 7;
foo = "bar"; // compile error

if (foo == "bar") { // compile error
    ...
}

if (fooo == 7) { // compile error
    ...
}
```

^Declare an integer variable foo

---

# Statics, generally

```kotlin, [.highlight: 2]
int foo = 7;
foo = "bar"; // compile error

if (foo == "bar") { // compile error
    ...
}

if (fooo == 7) { // compile error
    ...
}
```

^The compiler won't allow you to change the type of data the variable holds

---

# Statics, generally

```kotlin, [.highlight: 4-6]
int foo = 7;
foo = "bar"; // compile error

if (foo == "bar") { // compile error
    ...
}

if (fooo == 7) { // compile error
    ...
}
```

^The compiler also only allows sensical type comparisons

---

# Statics, generally

```kotlin, [.highlight: 8-10]
int foo = 7;
foo = "bar"; // compile error

if (foo == "bar") { // compile error
    ...
}

if (fooo == 7) { // compile error
    ...
}
```

^Because of this, the compiler knows what variables exist

---

# Statics, generally

- Prevents misspelled variables
- Catch NoMethodError, NameError, undefined early
- Add context to the code

^Example: Ruby HashWithIndifferentAccess

^Types are in your head; part of understanding

^Reduce cognitive load

---

# Errors caught early

![inline](videos/static-errors.mov)

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
    "user_id": 44,
    "address": {
        "number": 2446,
        "street": "Belmont Rd NW",
        "zip": 20008
    }
}
```

^My friend wants to update their address, she used to live at 1600 Pennsylvania Ave, NW

---

# Param checking in Ruby

```ruby
def update_address
    check_address_params
    ...
end

def check_address_params
    has_required_params = params.key?(:user_id) && params.key?(:address)
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
boolean updateAddress(AddressRequest newAddress) {
    // Guaranteed to be well-formed
    ...
}

class AddressRequest {
    int userId;
    Address address;
}

class Address {
    int number;
    String street;
    int zip;
}
```

^In Java, we just describe the structure & types of input, the rest is taken care of

---

#[fit] What makes
#[fit] Kotlin better?

^================
^================

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

# Immutable variables

- `val`s are immutable (read-only; like `final` in Java)
- `var`s are mutable
- Type inference

```kotlin
    val foo = "Foo" // is a String
    // Java: final String foo = "Foo";
    foo = "Bar"     // nope, can't change a val

    var num = 7     // mutable int variable
    num = 3         // ok
```

^Using final in Java provides guarantees for threading; immutable by default

^Java 10 also has type inference

---

# Null safety

```kotlin, [.highlight: 1-2]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^One of the most important differences is null safety

^Cleaner than Java's Option types

---

# Null safety

```kotlin, [.highlight: 4,5]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^I love typed languages because they give us errors early—Kotlin won't let you get into a situation where you could have an NPE

---

# Null safety

```kotlin, [.highlight: 7]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^Kotlin won't let you do soemthing dangerous unwittingly

---

# Null safety

```kotlin, [.highlight: 8]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^If you're really ok with getting a null, Kotlin will let you, but you have to be explicit

---

# Null safety

```kotlin, [.highlight: 9]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

maybeNull.length   // Compiler error
maybeNull?.length  // Possible null instead of length
maybeNull!!.length // Possible NPE

var valueOrSadness = maybeNull ?: "it was null 😞"
```

^You can also just be lazy, but it's obvious

---

# Null safety

```kotlin, [.highlight: 11]
var neverNull: String = "foobar"
var maybeNull: String? = "baz"

neverNull = null   // Compiler error
neverNull.length   // Guaranteed ok

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

^What makes vull values easy to handle is the compiler's null tracking

^If you have a conditional that checks whether a variable is null, the compiler understands that

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

^Say you have this function (written in Kotlin)

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

^Calling it from Java requires explicitly stating the new variable's type

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

^But the Kotlin compiler can infer types (Java 10 can do this, too)

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

^In Java, creating a class just to store data is cumbersome

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

^Free copy constructor is great—change a few attributes but get a new object

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

# Coroutines[^4]

```kotlin
fun main(args: Array<String>) {
    GlobalScope.launch { // launches coroutine in background thread
        delay(1000L) // doesn't block main thread
        println("World!")
    }
    println("Hello,")
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive
}

// Hello,
// World!
```

[^4]: https://kotlinlang.org/docs/reference/coroutines/basics.html#your-first-coroutine

---

# Simple asynchronos execution[^5]

```kotlin
suspend fun slowApiOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}
suspend fun slowApiTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}

fun main(args: Array<String>) = runBlocking<Unit> {
    val one = async { slowApiOne() }
    val two = async { slowApiTwo() }
    println("The answer is ${one.await() + two.await()}")
}

// takes ~1000ms
```

[^5]: https://kotlinlang.org/docs/reference/coroutines/composing-suspending-functions.html

---

#[fit] Other bits

^================
^================

---

# Tail recursion[^6]

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

[^6]: https://kotlinlang.org/docs/reference/functions.html#tail-recursive-functions

^The Kotlin compiler can make tail-recursive functions stack safe

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

# Better `switch` statement (`when`)[^7]

```kotlin
when (view) {
    is TextView -> toast(view.text)
    is RecyclerView -> toast("Item count = ${view.adapter.itemCount}")
    is SearchView -> toast("Current query: ${view.query}")
    else -> toast("View type not supported")
}
```

[^7]: https://antonioleiva.com/when-expression-kotlin/

^Note auto-casting

^Compare with Scala

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

#[fit] How do I use it?

^================
^================

---

# build.gradle

```groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.2.70"
    }
}

apply plugin: 'kotlin'
```

^If you're still using Maven, rethink your life choices

---

**Tell me more**
- Kotlin docs—[https://kotlinlang.org/docs/reference/]()
- Try Kotlin in your browser—[http://try.kotlinlang.org/]()
- My Kotlin repos—[https://github.com/dinomite]()
- Dynamic vs Static—[https://youtu.be/Uxl_X3zXVAM]()
- Criticisms of Kotlin—[https://allegro.tech/2018/05/From-Java-to-Kotlin-and-Back-Again.html]()

**Drew Stephens**
\<drew@dinomite.net\>
[@dinomite](https://twitter.com/dinomite)

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

^Compare with Java version in the first section
