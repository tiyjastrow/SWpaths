## Scala & Functional Programming
* Use industry-standard tools
  * JDK
  * Scala, SBT, Lightbend Activator
  * IntelliJ + Scala plugin
* Focus on data structures
  * Emphasize the distinction between code and data === "isomorphic"
  * Think about the data first, and the code will follow
  * Design projects specifically around use of various data structures
* Start with: Overview, install Scala in IDE, then learn Scala language

#### REFERENCES
- [Scala Koans](http://www.scalakoans.org/) available online here: [Scala Exercises - The easy way to learn Scala](http://scala-exercises.47deg.com)
- [A Scala Tutorial for Java Programmers](http://docs.scala-lang.org/tutorials/scala-for-java-programmers.html)  ==> "Scala interoperates seemlessly with Java" so, we can simply import the classes of the corresponding Java packages.
- [Scalatra is web micro-framework written in Scala](http://scalatra.org)
- video intro to Finatra:  [Finatra Presentations](http://twitter.github.io/finatra/presentations/)
- online Scala: try it in your browser with [ScalaFiddle.io](https://scalafiddle.io)
  - minimal editor: [ScalaKata](http://www.scalakata.com/)
- [Java to Scala cheatsheet](http://techblog.realestate.com.au/java-to-scala-cheatsheet/)
- [Scala Tutorial for Java Programmers](http://docs.scala-lang.org/tutorials/scala-for-java-programmers.html)
- [Using Scala Code from Java](http://lampwww.epfl.ch/~michelou/scala/using-scala-from-java.html)
- [Scala support](http://www.scala-lang.org/community/)
- Scala [Class Hierarchy diagram](http://docs.scala-lang.org/tutorials/tour/unified-types.html)
- [Scala tuple examples and syntax](http://alvinalexander.com/scala/scala-tuple-examples-syntax)

## Language Overview
* Background
  * statically-typed
  * 2001 by Martin Odersky (background IS in Java)
  * compiles down to Java ByteCode, runs on JVM.  BUT, can also compile to .NET's CLR.
  * syntax diff
    * C (Java, JavaScript)
        * `int score = 1;`
        * `sayHello("Alice");`
        * `if (isDone) { sayHello("Alice"); }`
    * Scala
        * `val score = 1`
        * `sayHello("Alice")`
        * `if isDone sayHello("Alice")`
             * Scala is so much more concise, there's almost no boilerplate! 
             * One single line of Scala code can replace up to ten lines of Java code! 
  * Major paradigms
      * Procedural: `add(nameList, "Charlie")`
      * Object-Oriented: `nameList.add("Charlie")`
      * Functional: `nameList = add(nameList, "Charlie")`
  * Java is an *object-oriented* language in the *C* family
  * Scala is a pure *object-oriented* language with additional functional language constructs:
      * Scala does not have NULLs. You will not see a null pointer exception in Scala!
      * Functional constructs in Java can be awkward and clumsy where Scala they can be quite elegant:

```scala
    //  Java

    peoples.stream().filter( x -> x.firstName.equals("John")).collect( Collectors.toList() );

    //  Scala

    peoples.filter(_.firstName == "John")

```


  * Scala runs on the JVM, compiles down to Java Byte Code, and therefore seamlessly runs alongside Java

## Install Scala Plugin
Either download [Scala IntelliJ plugin](https://plugins.jetbrains.com/plugin/?id=1347), or from within IntelliJ:
1. Open IntelliJ  
2. Preferences - Plugins / search for Scala ver 3.0.3
3. New Project - Scala - Scala / name it "LearnScala"  
   Project SDK: Java 1.8  
   Scala SDK: scala-sdk-2.11.8
4. Scala / Java interoperability  

Create a new Scala **object** called `LearnScala`

```scala
    object LearnScala {
      def main(args: Array[String]): Unit = {

      }
    }
```

Create a Scala Class called 'Hello'
```scala
    /** Hello.scala  */
    class Hello {
      def hello() { println("Hello (Scala class)")}
    }

    // singleton object
    object Hello {
      def hallo(): Unit = {
        println("Hallo from (Scala object)")
      }

      def main(args: Array[String]): Unit = {

        println("Now running Scala...")
        new Hello().hello()
        Hello.hallo()
      }
    }
```

Run the Scala main()..

Create a Java Class called 'HelloJava'
```java
    /** HelloJava.java */
    public class HelloJava {
      public static void main(String[] args) {

        System.out.println("Now running Java...");
        new Hello().hello();
        Hello.hallo();
      }
    }

```

Run the Java main()..

#### Describe Scala (show in a Scala scratch file)
* Take the [Tour of Scala](http://docs.scala-lang.org/tutorials/tour/tour-of-scala.html)
* Everything is an Object:
  * Review the [Scala Class Hierarchy](http://docs.scala-lang.org/tutorials/tour/unified-types.html)
  * Look at the example that `Demonstrates strings, integers, characters, boolean values, and functions are all objects`

```scala
        val list: List[Any] = List(
          "a string",
          732,  // an integer
          'c',  // a character
          true, // a boolean value
          () => "an anonymous function returning a string"
        )
        list.foreach(element => println(element))
```

* numbers: 1 + 2 * 3 / x   is same as  (1).+(((2).*(3))./(x))     NOTE: 1.0 becomes a Double
* MUTABILITY
  * "Make your variables immutable, unless there's a good reason not to"
  * Java programmers make variables mutable mostly since adding the 'final' keyword is extra thinking/work!
  * So, use immutable to "Reduce the possible number of states!"
  * e.g.; "I/O, or interacting with the outside world is inherently state dependent, and thus must be dealt with in a mutable manner"
* Functions: are values!
  * can be passed around like objects (because they are!)
  * can be stored in a variable (because they are values!)
  * This is what makes Scala useful for Functional Programming!
  * `"Scala just makes turning a method into a function (or storing a function in a variable) trivial."`
  * Functions in Scala are just a special form of an object (FunctionN) and actually, they are implemented as such, but that's an implementation detail and not exposed.
* classes in Scala can have parameters; this is essentially a constructor with parameters simultaneously defining fields!
* type inference
  * "built-in type inference mechanism" ie; option to SKIP type annotations!

```scala
        val x = 1 + 2 * 3         // the type of x is Int
        val y = x.toString()      // the type of y is String
```

* methods do NOT need () but only for zero parameters

```scala
        def f(i: Int) = i * 2
        f(4)

        var x = 1
        def g() = x * 2
        g()
        g

        def h = x * 2
        h
```

* override is now a keyword
  * "It is mandatory to explicitly specify a method overrides another method using the `override` modifier"
* `object` is a Scala Type. Scala application is a Singleton Object vs Java's `Class Main`
* with a "case class":
  * `new` keyword is not mandatory
  * getter functions are automatically defined
  * toString(), equals(), and hashCode() are defined by default
  * instances can be decomposed through pattern matching - quite useful!
* arrays:
  * num(0) = 5
  * val strs = Array("a", "b")
  * JAVA ArrayList`<String`> ..vs..  Scala List[String]
* Type Casting:
  * JAVA (Banana) myFruit
  * SCALA myFruit.asInstanceOf[Banana]
* Autoboxing:
  * BOX   val a: AnyRef = 4
  * UNBOX val b: Int = a.asInstanceOf[Int]

```scala
        val a: AnyVal = 4
        a.getClass
        val b = a.asInstanceOf[Int]
```

* Access Modifiers
  * Everything is public by default. Simply Google Search "Scala Access Modifiers", or [read details](http://www.jesperdj.com/2016/01/08/scala-access-modifiers-and-qualifiers-in-detail/)
* == calls the .equals() method on objects, checks value equality on primitives, and is null-safe.
* eq is used for object equality check if rarely used in FP because it implies mutability!
* Generics: [String] instead of \<String\>
* If/Else: same as Java
* Import: same
* no ++ or -- operators
* Tertiary Conditional:  JAVA condition ? A : B;  SCALA if (condition) A else B
  * A nice improvement over the Java if/then/else in Scala, if/then statements also return a value. As a result, there's **no need for a ternary operator** in Scala. This means that you can write if/then statements in Scala like this:

  ```java
     val x = if (a > b) a else b
  ```

* Scala Date - use the Java 8 library!  Since Scala interoperates seamlessly with Java, there is no need to implement equivalent classes in the Scala class library; we can simply import the classes of the corresponding Java packages:

```scala
        import java.time.LocalDate
    
        object LearnScala {
          def main(args: Array[String]) = {
    
            val day = LocalDate.now()
            println(s"Date is: ${day.getMonth} ${day.getDayOfMonth}, ${day.getYear}")
          }
        }
```

### Class declaration

#### READ: [Scala Classes](http://docs.scala-lang.org/tutorials/tour/classes.html) 
* Classes in Scala have parameters for constructor arguments
  * The `return` keyword is not needed to return a value. The value returned from a method is simply the last value in the method body!
  * still instantiated with the `new` keyword
  * But now defined more simpler with the Case class

```scala
case class Person(val firstName: String, val lastName: String)  
```

  * Compare to Java:

```java
public class Person {
    private String firstName;
    private String lastName;

    String getFirstName() {
        return firstName;
    }

    String getLastName() {
        return lastName;
    }

    public Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    int hashCode() ....

    boolean equals(Object o) { ....}
}
```
* Syntactic Sugar: means shorter, simpler syntax!
  * val x = 5    // this is just the accessor method! READ: [Instance variables with accessors/mutators](http://techblog.realestate.com.au/java-to-scala-cheatsheet/)
* Trait is like a Java Interface except they CAN contain implementation code
  * Scala [solves the Multiple Inheritance Problem](https://www.safaribooksonline.com/blog/2013/05/30/traits-how-scala-tames-multiple-inheritance/) by defining the implementation order. Additionally, Traits avoid the problem of the order running super class constructors since Traits don't have constructors!
* `Unit` in Scala is the same as `void` in Java

#### SCALA CONSOLE
Open a Scala `scratch` file and let's try this together now..

Hands on Exercises:  
1. * IntelliJ's "Run Scala Console" is BROKEN, Known Bug!  (see this: Right-Click on .scala file)
2. Either: Install Scala if you don't already have it:  
   $ brew update
   $ brew install scala
   Open Terminal inside IntelliJ and type: scala
3. Or; open a Scala `scratch` file.

```text
7
println("number " + res0)
7.toDouble * 0.3
"7".toDouble * 0.3
7.\<TAB\>
8 + 4.0
"8" + 4.0
val str = "hello"
str.toUpperCase
str.
s"$str, world"

val r =List.range(10,20)
def m(x: Int) = x * 10     // METHOD
m(r(0))
// FOR LOOP in JAVA vs SCALA: (int x : Collection){} vs ( var x <- Collection ){}
for ( x <- r ) { println(m(x)) }
// the Functional way
(x: Int) => x * 10               // a FUNCTION
val f = (x: Int) => x * 10       // Value assigned a FUNCTION
for ( x <- r ) { println(f(x)) }
r.foreach(x => x * 10)           // this will not print, just executes
r.foreach{x => m(x)}
r.foreach{x => f(x)}
r.foreach(f)
r foreach f                // called "INFIX NOTATION"  ie; 7 + 1 == 7.+(1)
r.map(x => x * 10)         // this will print after it executes
f
m    // errors because f is a value but m requires it's parameter list
val f2 = f
f2(3)
val newList = r foreach f   // returns type Unit (void)
// OTHER Functional Combinators:
val fe = r map f           // returns list of same # elements
r.filter( (i:Int) => i % 2 == 0 )
val isEven = (i:Int) => i % 2 == 0        // Function
r filter isEven
r map isEven
r.find( x => x == 15)    // READ find x where x equals 15
r.find(_.equals(15) )    // Scala Wildcard _
r find (_ == 15)         // SAME
// 'SOME' & 'NONE' are CASE Classes, children of the OPTION Class  (*WILL SHOW LATER!!)
r indexOf 15
r exists (x => x == 15)   // OR  (_ == 15)
r count isEven
r.sum
r.max
r partition isEven
var nameMap = Map("j" -> "John")
nameMap = nameMap + ("k" -> "kyle")
nameMap.keys
nameMap.values
nameMap = nameMap - ("k")
```

* Now, back in the `LearnScala` project in main() create some variables...
  * notice you cannot simply define without value assignment. Can use the wildcard '_' or define class as Abstract.
* Here's how the **OPTION Type** works.  
  * Here we have Some and None case objects that extend the abstract class Option.

```scala
      val r = ( 10 to 90 by 10).toList      // RANGE increment by 10
      println(r)

      def findNum = (i: Int) => (r find (_ == i))

      var num : Int = 0

      num = 15
      findNum(num) match {
        case Some(n) => println(s"Found the number $n")
        case None => println("Found NONE")
      }

      num = 10
      findNum(num) match {
        case Some(n) => println(s"Found the number $n")
        case None => println("Found NONE")
      }
```
