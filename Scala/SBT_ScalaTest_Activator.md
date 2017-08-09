## SBT
sbt is a build tool for Scala & Java + interactive environment to work in

- Little or no configuration required for simple projects

- Scala-based build definition that can use the full flexibility of Scala code

**INSTALL**
```
brew install sbt
```
Then type: `sbt` at the unix prompt.
* The \> is the sbt prompt and you are in what's called the “sbt shell”.

Try these commands:
> * about
* tasks
    * Here you'll find a list of useful commands like: console, compile, run, test, testQuick, and publish. SBT will runs tasks concurrently unless they have dependencies.
* help
* exit

**Go to IntelliJ**
* CREATE an SBT project named `HelloSBT`
* Look at project structure
* create a Scala object named `Hello` under src/main/scala/

```
  object Hello {
    def main(args: Array[String]){
      println("SBT says Hello!")
    }
  }
```
* Open Preferences - Plugins. Search for SBT and install ver 1.8
* open the SBT Console (SBT Action) and click the green start arrow
* type: run

**ScalaTest**
* To include [ScalaTest](http://www.scalatest.org/user_guide) in your sbt project, add the following line to build.sbt  (this works for scalaVersion 2.12.1  ..ref [Scalatest.org](http://www.scalatest.org/install))
```
 libraryDependencies += "org.scalatest" %% "scalatest" % "3.0.1" % "test"
```
* Click IntelliJ's `Refresh the project`. In the SBT Console run: `reload` to load the config changes.
* Now create a unit test using the FlatSpec "general-purpose" style (read the doc [testing styles](http://www.scalatest.org/user_guide/selecting_a_style))
  * create a Scala class called `PersonTest` in src/test/scala2.12/ that extends FlatSpec
  * write this:

```
import org.scalatest.FlatSpec

  class PersonTest extends FlatSpec {
    "A Person" should "have a name and age" in {
      val p = new Person("Joe", 17)
      assert(p.name == "Joe")
      assert(p.age == 17)
  }
}
```
  * Person is red so let's create it as a case class in the Hello.scala file.
```
case class Person(val name: String, val age: Int)
```
  * Run the test from the SBT Console window. At the `>` prompt, type: `test`

**FeatureSpec:**
* create a new test file `PeopleSpec`
  * we'll follow TDD
  * start with just the test strings
  * then add test code (ie; you may have to create an Item class with name parameter)

```
import org.scalatest._
import scala.collection.mutable

class PeopleSpec extends FeatureSpec with GivenWhenThen {

  feature("Collect items") {
    info("As a Person")
    info("I want to be able to collect items")
    info("So I can track my things")
    info("And always know what I have")

    scenario("User adds item to collection") {
      Given("a user exists")
      val person = new Person("Joe", 17)

      And("has a collection of items")
      val itemsList = person.items
      assert(itemsList.isEmpty)

      And("an item is available")
      val item = new Item("Tom Tom")
      assert(item != null)

      When("the user adds an item")
      person.addItem(item)
      assert(! person.items.isEmpty)

      Then("it should be in the collection")
      assert(itemsList.contains(item))
    }

    scenario("User searches for an item in collection") {
    }
  }
}

```

* Person.items is red so add it in Person:
  * val items = mutable.MutableList\[Item\]()
* Item is red so create a case class:
  * case class Item(name: String)
* Now new Item("Tom Tom") is red so add the parameter / field to Item:
  * (name: String)
* addItem() is red so define this method:
  * def addItem(item: Item): Unit = { items += item }

**Compare SBT Console** running PeopleSpec from IntelliJ (Menu bar or Right-Click)
- See how the `SBT test output` is something you could confidently provide to the customer!


## Activator
Lightbend Activator is a custom version of sbt which adds extra commands. 
[Lightbend Activator](https://www.lightbend.com/community/core-tools/activator-and-sbt) and SBT will kick start you with the `Lightbend Reactive Platform`. We already have "Lightbend Reactive Platform" plugged into ItelliJ from the Scala Plugin, but let's take a look at the command line options:

With `Activator` you can monitor, compile, run, test and code your application.

**INSTALL** from command line:

```
brew install typesafe-activator
```
* RUN: `activator new` to get a full catalog of template projects
* Try 4) minimal-scala and run: activator ui
  * The UI shows up in http://127.0.0.1:8888/home

**INSTALL** from IntelliJ:
* Start creating an Activator project named `BasicActivator` but before hitting ENTER...
  * Select `A basic Scala project` in the list of Templates. The Description says: "Bootstraps a cross-building project."
  * Click finish (this'll take time to download!)
      * RUN, TEST, check out the example Unit Tests in each of the Templates.

* Mixin the Trait `with Matchers` in your test class to take advantage of these great Equality Assertions:
> ScalaTest matchers provides five different ways to check equality, each designed to address a different need. They are:
> * result should equal (3) // can customize equality
> * result should === (3)   // can customize equality and enforce type constraints
> * result should be (3)    // cannot customize equality, so fastest to compile
> * result shouldEqual 3    // can customize equality, no parentheses required
> * result shouldBe 3       // cannot customize equality, so fastest to compile, no parentheses required
> * result should > 3
> * result should have length 3
> * string should startWith ("Hello")
> * etc.

* Now select from Templates (Just start typing to bring up the Search box) and select `Play 2.4 Scala Seed` Template to create a new project called `PlayScalaSeed`. We will build more on this project.

**HomeWork Exercises:** 
- Create a new Activator project named `IntroSBT` using the `An introduction to sbt` Template.  - Guides you through the basics of sbt given the complete development cycle.
- try adding some routes, unit or feature tests, other code and play with Play!
- play around with the code, make changes one at a time, run/test and see what breaks: this gives you experience with the error messages so you'll recognize the various error messages and what you changed to cause those errors.


## PLAY [currently on 2.6](https://www.playframework.com/documentation/2.6.x/Home)
Play is a very lightweight HTTP server. Functionality is provided by Play modules.

#### AKKA
Akka is a toolkit and runtime for building highly concurrent, distributed, resilient, message-driven applications on the JVM.
- written in Scala
- actor implementation is included in Scala standard library



