## more on Functional Scala

#### TUPLES
* Aggregate information without having to define a complete case class
* Introducing Tuples (ref: [Scala tuple examples and syntax](http://alvinalexander.com/scala/scala-tuple-examples-syntax))
* Try this in the console: `val stuff = (42, "Fish")`  THEN, `stuff.getClass`  THEN, `stuff._1` AND `stuff._2`
* A Tuple is not a collection. Tuples are classes named Tuple2, Tuple3, etc., all the way to Tuple22. 
* Defining a Tuple3. Try this in the Scala console:

```scala
  val userInfo = ("Bob", 30, 150.7)
  val(name,age,weight) = userInfo
```

This is a shortcut, also known as [Syntactic Sugar](https://en.wikipedia.org/wiki/Syntactic_sugar), to assign 3 different values from a Tuple3.

#### ZIP and ZipWithIndex
* checkout `zip` in the Scala Console:
  * List(1, 2, 3).zip(List("a", "b", "c"))
* checkout `zipWithIndex`:
  * List("a", "b", "c").zipWithIndex

#### FILEIO
From within the `LearnScala` IntelliJ project:
* Create a new Scala Object called `FileIO` with a main() method
* Create a text file in the Project root directory called `filename.txt`. Enter a few lines: City State Zip 
* Read from the file using `scala.io.Source` into a cities List

```scala
  val cities = Source
      .fromFile("filename.txt")
      .getLines.toList
```  

* Print each line in cities using the List.foreach method

```scala
  cities.foreach(line => println(line))
```

* Then print again with prepended line number for each line using List.zipWithIndex and the Tuple elements.

```scala
  println
  cities.zipWithIndex.foreach( line => println(s"Line ${line._2}: ${line._1}") )
```
* Then add a header to the file:  "CITY STATE ZIP" and run it showing the header.
* Now, print it skipping the header using a conditional:

```scala
  cities.zipWithIndex.foreach( line => if (line._2 != 0) println(s"Line ${line._2}: ${line._1}") )
```

* Create a CSV file named `filename.csv` in the project root directory with format:  "City,State,Zip"
* and read the Buffered Source, this time WITHOUT the `.toList` method, and print nicely formatted.

```scala
    Source
      .fromFile("filename.csv")
      .getLines()
      .foreach( line => {
        val Array(city, state, zip) = line.split(",").map(_.trim)
        println(s"${city}, ${state} ${zip}")
      })
```

* Now, add a Header:  "CITY, STATE, ZIP" and run it.

* Then, skip the header dropping the first line by adding the .drop(1) AFTER .getLines()

### More on Functional Programming

* Scala can be quite CRYPTIC, but does not have to be!  Any language can be obfuscated. Eventually, it becomes cleaner code.  

Example:

```scala
    (0/:l)(_+_)
```

is isomorphic to

```scala
    list.foldLeft(0)(x, y => x+y)
```

And if anyone's still not following, what this does is to sum all the elements of list. If the list is List(2,3,4) it looks like 0+2+3+4. Some languages/tools call it reduce (notably MapReduce/Hadoop), some call it inject (Ruby. Any others?). Some languages make you use a for loop, which I assume must be to stop you from getting too cocky about how good your code looks. [tech blog](http://http://rickyclarkson.blogspot.com/2008/01/in-defence-of-0l-in-scala.html)

* **NOTE:** foldLeft takes 2 parameters lists - `Methods may define multiple parameter lists`

* [Currying](http://http://docs.scala-lang.org/tutorials/tour/currying.html): "When a method is called with a fewer number of parameter lists, then this will yield a function taking the missing parameter lists as its arguments."  
  * or simply: a way of `turning a function of multiple arguments into a function which takes one argument and returns a function`
  * Walk thru this example in a Scratch file..

```scala
    def add(a: Int) = (b: Int) => a + b        //  clear intent
    
    def alternateAdd(a: Int)(b: Int) = a + b   // same thing as a TWO parameter list
    
    add(5)      // Function1
    add(5)(2)   // 7
    
    val addFive = add(5)  // (Int => Int)
    
    addFive(2) // 7
    
    
    assert(add(1)(6) == addFive(2))  // true if Unit
                                     // Try to fail this
```

### ToDoScala

* Create `ToDoScala` project - a console app to track to-do items
  * start with DATA: case class ToDoItem(text: String, isDone: Boolean)
  * use a while loop to collect ToDo items with `io.Source.stdin`

```scala
import scala.collection.mutable

case class ToDoItem (text: String, var isDone: Boolean) {
  override def toString = s"${if (isDone) "[X]" else "[ ]"} ${text}"
}

object Hello {
  val items = mutable.MutableList[ToDoItem]()
  def prompt(s: String) = {println(s); io.StdIn.readLine()}

  def menu = {
    val seq = Seq(choiceOne, choiceTwo, choiceThree, choiceQuit).mkString("\n")
    prompt(s"\nPlease select your choice:\n${seq}\n").toUpperCase
  }

  case object choiceOne {
    def opt() = {items += new ToDoItem(prompt("Enter new item"), false); ""}
    override def toString = "1. Create to-do item"
  }

  case object choiceTwo {
    def opt() = {
      val idx = prompt("Which item do you want to toggle").toInt
      items(idx).isDone = !items(idx).isDone
      ""
    }
    override def toString = "2. Toggle to-do item"
  }

  case object choiceThree {
    def opt() = {items.zipWithIndex.foreach(x => println(s"${x._2}. ${x._1}")); ""}
    override def toString = "3. List to-do items"
  }

  case object choiceQuit {
    def opt() = "Q"
    override def toString = "Q. Quit"
  }

  def main(args: Array[String]): Unit = {

    var resp: String = ""
    while (resp != "Q") {
      resp = menu match {
        case "1" => choiceOne.opt()
        case "2" => choiceTwo.opt()
        case "3" => choiceThree.opt()
        case "Q" => choiceQuit.opt()
      }
    }
  }
}
```

See also a great tutorial on Basic Data Structures and Functional Combinators at [Scala_School](https://twitter.github.io/scala_school/collections.html#zip)

* Basic Data Structures
  * Arrays
  * Lists
  * Sets
  * Tuple
  * Maps
  * Option
* Functional Combinators
  * map
  * foreach
  * filter
  * zip
  * partition
  * find
  * drop and dropWhile
  * foldRight and foldLeft
  * flatten
  * flatMap
  * Generalized functional combinators
  * Map?
