## Assignment

#### Description
Fork and clone my [ScalaTraits](https://github.com/tiyjastrow/ScalaTraits.git)

Create a project for employees with various "traits" such as a manager and a worker.

#### Requirements

Create a new Scala project named `ScalaTraits` with an Object class and the usual main method.
1. Create a Trait called `Employee` with 2 String fields, `name` and `title`.
2. Override its toString to a formatted string like this example: "My name is: Joe (Mgr)" where the name "Joe" and "Mgr" are substituted for the 2 fields.
3. Now define 2 classes `Manager` and `Worker` that extend the Trait.
4. Add an override val for the name in each class's constructor.
5. For each class define and implement the title field using "Mgr" for the Manager and "Wkr" for Worker.
6. Define a List that contains 3 or more employee objects with 1 of each type.
7. Define a Mutable HashMap[String, String].
8. Use the List's .foreach() method and pass in a function that assigns to a new HashMap the `name` as the Key and the toString method as the Value.
9. Now print the map using its .foreach() method to print just value (Hint: the 2nd part would be ._2)
10. Prompt the user for one of the titles and use the .filter method to extract and print just those employees with that title.

Optional: 
1. Add an Id and Password fields in Employee and include a method to encrypt the password.

Challenge: Write a few unit tests.

