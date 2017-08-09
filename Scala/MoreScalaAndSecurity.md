## Security / Constructors / Traits

#### Major Security Topics

**NOTE:** Web Application development MUST include **secure transactions!!**

* SQL injection prevention => Prepared Statements
* Cross-site scripting prevention (XSS) => allow for CORS if needed using `@CrossOrigin` by setting the response.header Access-Control.
  * Read the section on `Enabling CORS` in [Spring Guides](https://spring.io/guides/gs/rest-service-cors/): "The @CrossOrigin annotation enables it only for a specific method!"
  * Some examples:

```ruby
headers['Access-Control-Allow-Origin'] = "*"
headers['Access-Control-Allow-Methods'] = "GET, POST, PUT, DELETE, OPTIONS"
headers['Access-Control-Allow-Headers'] = "accept, authorization, origin"

headers['Access-Control-Allow-Origin'] = "http://foo.example"
headers['Access-Control-Allow-Methods'] = "POST, GET, OPTIONS"
headers['Access-Control-Allow-Headers'] = "X-PINGOTHER, Content-Type"
```

* SSL encryption => HTTPS, TLS, & SSL 
  * search on `"tls vs ssl"`  or read: [What's the difference between SSL, TLS, and HTTPS?](http://security.stackexchange.com/questions/5126/whats-the-difference-between-ssl-tls-and-https))
  * "SSL" means "Secure Sockets Layer"
  * "TLS" means "Transport Layer Security"
  * "HTTPS" essentially means HTTP with SSL"
* Secure password storage => create and store as a Hash

## Secure Password Storage

Thus far, we've been saving passwords in plaintext, which is a big mistake. If your server ever gets compromised, the intruder now has everyone's password. The solution to this is to hash the passwords before inserting them into the database, and when the user attempts to login you can simply hash the password they send and compare it to the hash in the database.

There are a few things to keep in mind about hashing passwords. If an attacker gets a database full of hashes, they can attempt to find the plaintext passwords through a brute force attack. There are two primary ways we can thwart this:

1. Use a hashing algorithm that takes a relatively long time. This will make it take longer for an attacker to brute force the hashes.
2. Use a salt -- i.e., a random number that you add to the password before hashing and store alongside the hash. This prevents an attacker from pre-generating hashes based on common passwords (known as rainbow tables).

A common hashing algorithm is SHA1. One problem, though, is that it is designed to be fast. To securely store it, we should run SHA1 at least a thousand times by taking the output of the previous hashing and running it through SHA1 again. A standardized way of doing this is called [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2).

#### Adding Java PasswordStorage to Scala code
Let's add this available Java source code to use in our `LearnScala project`: [PasswordStorage.java](PasswordStorage.java)

Just try adding this in main():

```scala
    val password = "xyz123"
    val hashed = PasswordStorage.createHash(password);

    println(s"password = ${password}")
    println(s"hashed = ${hashed}")

    println(s"verified with 'junk'? = ${PasswordStorage.verifyPassword("invalid", hashed)}")
    println(s"verified with Original '${password}' = ${PasswordStorage.verifyPassword(password, hashed)}")
```

#### Other Constructors
* Class’ body is the primary constructor

```scala
  class Greeter(msg: String) {
    println("Hello from Greeter class")
    def SayHi() = println(msg)
  }
  object Hello {
    def main(args: Array[String]) = {
      val greeter = new Greeter("saying hi")
      greeter.SayHi()
    }
  }
```

* Now let's change the constructor and add another parm

```scala
  class Greeter(msg: String, msg2: String)
```

This won't compile. So, we need to create `another constructor` for backward compatibility, ie; keep accepting a single parm.

```scala
  class Greeter(msg: String, msg2: String) {
    def this(msg: String) = this(msg, "")
```

#### Traits & Abstract classes
* [Traits](http://docs.scala-lang.org/tutorials/tour/traits.html) are like Java interfaces.
  * Traits are allowed to provide implementations on its method definitions.
  * We can also think of them as classes that can NOT have constructor parameters.
  * Also think multiple inheritance without some of it’s dangers but with some limitations.

Create a new file `Greet` in LearnScala project.

```scala
  trait GreeterTrait {
    def message: String
    def SayHi() = println(message)
  }

  class EnglishGreeter extends GreeterTrait{
    val message = "Hello World"
  }

  class SwedishGreeter extends GreeterTrait{
    val message = "Hej världen!"
  }

  object Greet {
    def main(args: Array[String]) = {

      val eng = new EnglishGreeter()
      eng.SayHi()

      val sweed = new SwedishGreeter()
      sweed.SayHi()
    }
}
```

NOTE: on a Mac type:  Either (1) `Option + u, then a = ä` or, (2) just press and hold the letter a

The trait defines the SayHi() method **with an implementation** so each implementing class inherits it by default.

Now, we can also use a "Mixin" by extending a Class and a Trait using the keyword `with`. You could also 'mixin' many Traits.

```scala
trait GreeterTrait {
  def message: String = ""
  def sayHi() = println(message)
}

trait CommonGreeterTrait {
  val language: String
  def openDoor() = {
    language match {
      case "EN" => println("Come on in")
      case "SW" => println("Kom in")
    }
  }
}

class SweedishGreeter extends GreeterTrait with CommonGreeterTrait {
  override val language = "SW"
  override val message = "Hej världen!"
}

object Greet {
  def main(args: Array[String]) = {

    val sweed = new SweedishGreeter
    sweed.sayHi()
    sweed.openDoor()
  }
}
```

