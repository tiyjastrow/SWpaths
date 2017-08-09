# Scala Map and Filter

[purchases.csv](purchases.csv) (5 KB) 

## Description
Fork and clone my project [ScalaPurchases](https://github.com/tiyjastrow/ScalaPurchases.git)

Read and parse the CSV file of purchase data. Allow the user to filter the list by typing in a category name.  

## Requirements

* Create a Scala project named `ScalaPurchases` and download the above `purchases.csv` file into the project root directory
* Create a Scala Object file named `FileIO` and include a main() method
* Read the file and split each field into a Tuple5 using the .split(",")
* Drop the header from the rest of the lines using .drop(1)
* Create a mutable list of type Tuple5, ie;  `MutableList[ (String, String, String, String, String) ]()`
  * for each line use: line.split(",").map(_.trim) to create and populate each field in the Tuple
  * use the += method to add each purchase to this list
* Then, prompt the user to enter a category name
  * The possible categories are: Furniture, Alcohol, Toiletries, Shoes, Food, Jewelry
* Read a line of input from the user
* Filter or loop for each category matching the user's choice and print out in this format: "Customer: 23, Date: 2016-07-11"
* Save the results into a file called `filtered_purchases.prn`
