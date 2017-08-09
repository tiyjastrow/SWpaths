## Review some more Scala combinators:
Combinators are awesome, helping to make short work of complex logic.

#### zip, partition, flatten, flatMap

Zip if a handy list operator. It simply zips together 2 lists, even with diff types! AND `returns a List of Tuples`.
```
  List(1, 2, 3).zip(List("a", "b", "c"))
  res1: List[(Int, String)] = List((1,a), (2,b), (3,c))
```

Partition is similar to splitAt which we used in the Cards draw() to pull out x number of cards. Where splitAt will split based on the number of elements, partition will split based on evaluating a function parameter. So, here the criteria is any element greater than 1 AND `returns a Tuple`.
```
  List(1, 2, 3).partition (_ > 1)
  res2: (List[Int], List[Int]) = (List(2, 3),List(1))
```

Flatten will do just that; remove embedded Lists within Lists AND `returns a single List`.
```
  List(List(1, 2), List(3, 4)).flatten
  res3: List[Int] = List(1, 2, 3, 4)
```

So, now FlatMap combines both map and flatten. Handy with nested Lists. FlatMap parameter is a function applied on all elements AND `returns the flattended results of same type`.
```
  List(List(1, 2), List(3, 4)).flatMap(x => x.map(_-3))
  res4: List[Int] = List(-2, -1, 0, 1)
```

If you have mixed types in the Lists then Scala defaults to the LCD type:
```
  List(List("a","b"), List(3,4)).flatMap( x => x.map(_+"Y"))
  res5: List[String] = List(aY, bY, 3Y, 4Y)
```

#### Map
Best of all, each of these functional combinators also work on Maps (the type). Map returns a Tuple.
```
  val m = Map("steve" -> 100, "bob" -> 101, "joe" -> 201)     // m: immutable.Map[String,Int]

  m.head
  res6: (String, Int) = (steve,100)

  m.filter( (nameExt: (String,Int)) => nameExt._2 > 200 )
  res7: immutable.Map[String,Int] = Map(joe -> 201)

  m.map( x => s"Hi ${x._1} you're ${x._2}" )
  res8: immutable.Iterable[String] = List(Hi steve you're #100, Hi bob you're #101, Hi joe you're #201)
```

## Recursion
One of the biggest benefits of recursion is solving problems based on tree structures.
* It's basically a function that calls itself.
* Use instead of normal `for` loop if you want to control when the loop continues and update its values

This is a simple example of recursion. "For every number in a list, add the first value to the result of this method passing the rest of the list."

```scala
    def sum(list: List[Int]): Int = {
      if (list.isEmpty) 0
      else list.head + sum(list.tail)
    }
    println(sum( List(1,2,8,9) ))
```

In other words:  for list of 1,2,8,9 =  1 + ( 2 + (8 + (9 + (0) ) ) )

Now let's see the real power of recursion! **Just follow this conceptually:**

```scala
    // implementation of Quicksort (similar syntax to how we'd write it in Java or C)
    def sort(xs: Array[Int]) {
      def swap(i: Int, j: Int) {
        val t = xs(i); xs(i) = xs(j); xs(j) = t
      }
      def sort1(l: Int, r: Int) {
        val pivot = xs((l + r) / 2)
        var i = l; var j = r
        while (i <= j) {
          while (xs(i) < pivot) i += 1
          while (xs(j) > pivot) j -= 1
          if (i <= j) {
            swap(i, j)
            i += 1
            j -= 1
          }
        }
        if (l < j) sort1(l, j)
        if (j < r) sort1(i, r)
      }
      sort1(0, xs.length - 1)
    }
```
Here within this SORT recursive method definition we define a method called SWAP. The Swap will take 2 int parameters and store the 1st one in a temp variable t. Then swap the corresponding values from the array. Then we define the sort1 method to: (1) find the middle point of the array to pivot on based on the 2 parameters, (2) loop below and above the pivot point to position new pair of variables i & J, (3) now swap the new vars based on pivot postions, (4) and finally, call sort1 itself based on new values as we walk thru the array.

The array gets sorted in place this way.

**NOW the recursive way:**

```scala
    def sort(xs: Array[Int]): Array[Int] = {
      if (xs.length <= 1) xs
      else {
        val pivot = xs(xs.length / 2)
        Array.concat(
          sort(xs filter (pivot >)),
          xs filter (pivot ==),
          sort(xs filter (pivot <)))
      }
    }
```

* Try writing this imperative solution and compare it to writing it recursively:

```
//imperative

    val myList = List(1,2,3,4)
    var total = 0
    for(f <- myList) { total += f }
    println(total)

//recursion
   def total(xs: List[Int]): Int = xs match {
      case Nil => 0
      case x :: ys => x + total(ys)
   }
   println(total(myList))

//even more simple
  def total(num: Int): Int = {
    if (num <= 1) num else num + total(num -1)
  }
```

The difference is that a recursive solution doesn’t use any mutable temporary variables by letting you break the problem into smaller pieces. Because Functional programming is all about writing side effect free programs it's always encourage to use recursion vs loops (that use mutating variables).

