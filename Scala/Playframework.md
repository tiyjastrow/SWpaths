## Playframework
[Play](https://www.playframework.com/) is a framework for easily building web applications with Java or Scala. And it's popular for building RESTful APIs as well.

Play ..  let's start by opening the previous PlayScalaSeed project
- is a very lightweight HTTP server for HTTP requests
- makes JSON a first class citizen (ie; Play makes using JSON easy!)
  - "Modern web applications often need to parse and generate data in the JSON (JavaScript Object Notation) format. Play supports this via its JSON library" see the Play Docs: [JSON basics](https://www.playframework.com/documentation/2.5.x/ScalaJson)
  - To convert our classes to JsValues we'll define implicit Writes converters and provide them in scope. *NOTE: The combinator pattern makes this even simpler.
- uses Modules to provide simple or complex functionality such as database access, etc.
- uses `dependency injection` (DI) where components in the system inject other components in as needed.

> Before we look at how Play makes it easy to work with JSON, let's explore some powerful and often used capabilities:
> - Dependency Injection,
> - Implicits,
> - Currying

#### Dependency Injection
- DI is also known as Inversion of Control (IoC) because it inverts the control of object dependencies.
- Play uses Google Guice. "Guice's @Inject is the new `new`"; meaning that Guice removes the need for the 'new' keyword. Bottom line is updating code and unit testing become easier. 
Quick example of DI. Just pass in the object in as an argument to the constructor instead of having to implement some kind of Factory. This lets some other component do all the work:

```scala
  class WithoutDI() {
    val thing1 = Factory.createThing()      // Factory dependent so we have to know how to create the Thing
  }

  class WithDI(thing: Thing) {
    val this.thing1 = thing                 // Independence since the Thing is passed in
  }

  class WithDI @Inject() (thing: Thing) {
    val this.thing1 = thing                 // DI at Runtime
  }

```

So, without dependency injection, your class is tightly coupled to its dependency. Loose coupling is more desirable so DI is definitely preferred.  For further study see Understanding [Dependency Injection](https://www.playframework.com/documentation/2.5.x/ScalaDependencyInjection) or Google's [Guice Doc](https://github.com/google/guice/wiki/GettingStarted).

A good example is this InjectedRoutesGenerator in build.sbt. It will declare all the controllers that it routes to as dependencies, allowing your controllers to be dependency injected.

```
  routesGenerator := InjectedRoutesGenerator
```


#### Scala Implicits
It is worthwhile understanding 
Scala has **implicit parameters** and **implicit conversions**.
* An `implicit conversion` is a 'predefined' type or method that is made available automatically. The `Predef` object provides definitions that are accessible in all Scala compilation units without explicit qualification. (NOTE: This is some of the 'magic' of Scala)
  * to see a list just type: `int2Integer` and drill into it so IntelliJ will bring up the **object Predef** and show some "Autoboxing" and "Autounboxing" methods.

```
  val i: Int = 3
  val x: Integer = i                  // does an implicit call: int2Integer(i)
  println(x)
  val y: Integer = int2Integer(i)     // implicitly: java.lang.Integer.valueOf(i)
  println(y)
```

* For more on `Implicit parameters` see: [Scala Docs tutorials](http://docs.scala-lang.org/tutorials/tour/implicit-parameters.html).

```scala
   // example of implicit parameter
    def sayHello(name: String)(implicit greeting: String) {
      println(s"${greeting}, ${name}")
    }
    // sayHello("John") // does not compile
    sayHello("Paul")("Hello")

    implicit var greeting = "Hi"
    sayHello("George")

    greeting = "Hey"
    sayHello("Bill")

    greeting = "Hola"
    sayHello("José")
```

* For a terrific explanation, read: [Implicit Conversions and Parameters by Martin Odersky](http://www.artima.com/pins1ed/implicit-conversions-and-parameters.html)

## Scala Currying
Besides Implicits there are other powerful Scala concepts: see [Scala Docs Glossary](http://docs.scala-lang.org/glossary/)
* In a nutshell Currying is simply "Partially applied functions". So, basically you define a function that takes multiple parameter lists. Then when you call this function and pass all but the last parameter, then the last parameter can be applied at a later time. Useful if you have not yet defined that last parameter.
* Define a Curried function with multiple parameter lists in the Scala REPL or SBT Console:

```
  def addThemUp(x: Int)(y: Int) = x + y

  // Alternatively:
  def addThemUp(x: Int) = (y: Int) => x + y

  // Called like this:
  addThemUp(5)(8)

  // Or, create a new function:
  def addTen(x: Int) = addThemUp(x)(10)
```

Another way to think of this.. 

An argument list is a Tuple. So, basically, Currying turns a Tuple into separate arguments:  (a, b) -> c) -> a -> b -> c


#### Json is a first class citizen
Building on the `PlayScalaSeed` project:
* create a case class for `Thing`

```scala
  case class Thing(myThing: String)
```

* add a route to return JSON

```scala
  GET     /json                       controllers.Application.json(var: String)
```

* create the controller

```scala
  def json(parm: String) = Action {

    val thing1 = Thing(s"One ${parm} thing")
    val thing2 = Thing(s"Leads to another: ${parm}")
    val allThings = List(thing1, thing2)

    Ok( Json toJson allThings )
  }

```
* add a companion object to the Thing case class. This works by resolving case class fields & required implicits at COMPILE-time.

```scala
  object Thing {
    implicit val thingReads = Json.reads[Thing]
    implicit val thingWrites = Json.writes[Thing]
  }
```
* Here we're also using the `Macros` (new in Scala 2.10) - a feature enabling:
  * compile-time code enhancement
  * compile-time class/implicits inspection
  * compile-time code injection

#### Activator UI
Run `activator ui` at the command line to view all templates interactively!
> We'll look for ways to consume REST API (web services) in play

#### Sinatra
Perhaps the easiest way to find available services is to create our own. But let's just make a small app using a very simple tool for just this occasion, Sinatra.

Sinatra is a simple and lightweight web framework written in Ruby. Here is a quick way to create a REST API, but first install Ruby and the Sinatra gem:
>1. brew update 
2. brew doctor
3. brew install ruby
4. gem install sinatra
5. mkdir ~/websites
6. cd ~/websites
7. mkdir Sinatra
8. cd Sinatra
9. vi myapp.rb

```
  require 'sinatra'

  get '/' do
    '{"Hello world": [ "from" : "my simple little Sinatra webframework", "using" : "Ruby!" ] }'
  end

  get '/text/:title' do
    "You sent me: #{params['title']}"
  end

  get '/jsontest' do
    '[{"myThing":"One nifty-neat thing"},{"myThing":"Leads to another: nifty-neat"}]'
  end
```
* Run: `ruby myapp.rb`
* View: http://localhost:4567
* Read MORE: http://www.sinatrarb.com/intro.html


if time.. explore some HTML and Javascripting:

```
<!DOCTYPE html>
<html>
<body>
<script>
function swap(newtab){ 
  var tab1 = document.getElementById('tab1');
  var swap = document.getElementById(newtab);
  tab1.innerHTML = swap.innerHTML;
}
function flip(){
  var tab1 = document.getElementById('people')
  var tab2 = document.getElementById('baseball')

  if (tab1.style.visibility == "hidden") {
    tab1.style.visibility = "visible";
    tab2.style.visibility = "visible";
  }else{
    tab1.style.visibility = "hidden";
    tab2.style.visibility = "hidden";
  }
}
</script>
<a href="javascript:swap('baseball');">Show Baseball</a><br/>
<a href="javascript:swap('people');">Show people</a>

<input type="button" value="flip tables"
 onclick="flip();"
/>

<table border=0 id="tab1" style="width:100%">
  <tr><td></td><td></td><td></td></tr>
  <tr><td></td><td></td><td></td></tr>
  <tr><td></td><td></td><td></td></tr></table>

<table id="people" style="width:100%; visibility:hidden">
  <tr>
    <td>Jill</td>
    <td>Smith</td>
    <td>50</td>
  </tr>
  <tr>
    <td>Eve</td>
    <td>Jackson</td>
    <td>94</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
    <td>80</td>
  </tr>
</table>

<table id="baseball" style="width:100%; visibility:hidden;">
  <tr>
    <td>New York</td>
    <td>Mets</td>
    <td> Citi Field </td>
  </tr>
  <tr>
    <td> Minneapolis </td>
    <td>Twins</td>
    <td> Target Field </td>
  </tr>
  <tr>
    <td> Kansas City </td>
    <td>Royals</td>
    <td> Kauffman Stadium </td>
  </tr>
</table>
</body>
</html>
```

#### Let's create a local web page to display JSON returned from a Sinatra GET route.
* Edit your myapp.rb file we created the other day to add this route:

```ruby
get '/cards' do
  response.headers['Content-Type'] = "application/json"
  headers['Access-Control-Allow-Origin'] = "*"
  headers['Access-Control-Allow-Methods'] = "GET, POST, PUT, DELETE, OPTIONS"
  headers['Access-Control-Allow-Headers'] ="accept, authorization, origin"
  '{
  "meta": { "code": 200 },
  "data": { "hand": { "card1": "Ace of Spades", "card2": "Five of Hearts", "card3": "King of Diamonds" }}
  }'
end
```

* Now run it with: ruby myapp.rb
* Then, cd to your HOME directory and create a file called `cards.html`:

```html
<html>
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>

<script>
$(document).ready(function(){
    $("button").click(function(){
        $.ajax({url: "http://localhost:4567/cards", success: function(result){
            $("#div1").html(JSON.stringify(result));
        }});
    });
});
</script>
</head>

<body>

<div id="div1">Click below to change this text...</div>

<button>Get External Content</button>

</body>
</html>
```

* In the same directory start a webserver with: python -m SimpleHTTPServer
* In your browser run: http://localhost:8000/cards.html
