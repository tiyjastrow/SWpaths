#### Playing Games in Scala

* Create a `PlayCards` Scala project
* learn about and practice:
  * For Comprehension
  * Range
  * Yield
  * def apply()
  * Seq() object
  * pattern matching
  * Random shuffle a collection
  * map
  * splitAt
  * mkString

Let's begin by defining a Cards object and main() to run the game:
```
  import scala.util.Random

  object Cards {
    def main(args: Array[String]) = {
```
Next, add a case class to define a single card:
```
  case class Card(value: Int, suit: String) {
```
We also need an object to contain the 4 suits as a Sequence of Strings:
```
  object Suits {
    def apply() = Seq("Diamonds", "Clubs", "Hearts", "Spades")
  }
```
The apply() here works like this:  Suits.apply()  


Now we'll create a new deck of cards as a List[Card]. Using a `For Comprehension` allows us to build up the deck 1 card at a time by defining the suit from the Seq and the card value from a Range.
```
    def newDeck: List[Card] = {
      val deck = for {
        suit <- Suits()
        value <- 1 to 13
      } yield Card(value, suit)

      deck.toList
    }
```
* The `yield` keyword is syntactic sugar to return the resulting Set of elements in the For loop.
So, `for { each suit in Suits() AND each value from 1 to 13} yield RESULT` can be read as: "RESULT is a Set of all Cards for every Suit and Value combination"
* Yield provides nice syntax for accessing flatMap, filter and foreach; especially when we have multiple expressions in our loop.

 So,
```
val result = for (n <- aList) yield n + 2
```
is equivalent to:
```
val result = aList.map(n => n + 2)
```

Define a method to shuffle the deck:
```
    def shuffle(d: List[Card]) = Random.shuffle(d)
    var brandNewDeck = newDeck
    var shuffledDeck = shuffle(newDeck)
```
Define a method to draw 1 or more cards:
```
    def draw(amount: Int): List[Card] = {
      val (hand, remainder) = shuffledDeck.splitAt(amount)
      shuffledDeck = remainder
      hand
    }
```
Print out a Poker hand of 5 cards and show the number of cards left in the shuffledDeck:
```
    println(s"Size of deck = ${shuffledDeck.size}")
    val hand = draw(5)
    println(hand)
    println(s"After draw = ${shuffledDeck.size}")
```
Use Pattern Matching to show the proper names of the card values. Add this to the Card class:
```
  def name = {
    value match {
      case 1 => "Ace"
      case 11 => "Jack"
      case 12 => "Queen"
      case 13 => "King"
      case _ => value
    }
  }
  override def toString = s"${name} of ${suit}"
```
Print out another hand:
```
val hand2 = draw(5)
println(hand2)
```

How would we show what a FullHouse looks like? Here we'll override the toString:

```
object FullHouse  {
  val randInt = Random.shuffle(1 to 13)
  val v1 = randInt.head
  val v2 = randInt.drop(1).head
  val randSuit = Random.shuffle(Suits())
  val s1 = randSuit.head
  val s2 = randSuit.drop(1).head
  val s3 = randSuit.drop(2).head
  val s4 = randSuit.drop(3).head
  def show = Seq(Card(v1,s1), Card(v1,s2), Card(v1,s3), Card(v2,s1), Card(v2,s4))
  override def toString = show.toList.map(_ + ", ").mkString
}

println(s"A Full House:\n ${ FullHouse }")
```

- a Pair?
- 3-of-a-kind?
- how about a Straight?
  - a straight is simply a list of 5 unique cards, where; the values if in sequence matches up with the index which is also in sequence. Then if you subtract value - index they are all the same result! (ie; 10-0 = 11-1). So...
 
```scala
  def isStraight: Boolean = {
    cards.map(c => c.value) // .map(_.value)
      .sortBy(v => v)
      .zipWithIndex
      .map(tup => tup._1 - tup._2)
      .distinct
      .size
      .equals(1)
  }
```


