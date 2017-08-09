##Introduction
The best way to learn any new language is to jump in and start building. 

But first we do need to cover a few basics.

#### Basic Programming Topics
* Object Oriented Programming 
  * examples: Java, C++, C#, Ruby, Python, JavaScript, etc.
* Objects 
  * metaphor for a real world object
* Classes
  * `Class Animal` is a type definition for an "animal" object
* Inheritance
  * A Dog is a type of Animal
* Methods
  * A Dog can do things such as `makeSound()` like "bark!"
* Variables
  * A Dog has properties like numberOfLegs = 4

## An Intro to Java 
- FREE online [TIY's Java Mini Course](https://online.theironyard.com/join/javamini)
- Practice fundamental skills, play with code, and learn what it takes to be a programmer.
- Runnable Code Samples (try 'em)
- Receive a Free Iron Tee shirt!!

## The Iron Yard program
- Read the full descriptions of the [Back-end Engineering courses](https://www.theironyard.com/courses#back-end-engineering)
- [Here's what you'll learn](https://www.theironyard.com/courses/java-and-clojure)
  - Writing basic logic
  - Object-oriented
  - Debugging and testing
  - Data structures
  - Data storage
  - Libraries, Frameworks, Tools, and more.

## Let's start

- Before we can begin, need to understand the requirements.
- Einstein said it best: "A problem well defined is a problem half solved."
- So, we'll write our requirements in short bursts (Agile method) as "User Stories"

### User Story:
**Title:**  Play the game of Tic Tac Toe

**Narrative:** 
    As a player, I want to take turns with another player by entering X or O, So that one player either wins or it's a draw.

**Acceptance Criteria:**

**Scenario 1:** Take turns on one game move  
    Given a game board has been initialized  
    And another player is available and ready to play  
    When I mark some square with either X or O  
    And the game is not yet over  
    Then the other player takes their turn doing the same
    
**Scenario 2:** Winning the game  
    Given a game is in play  
    And at least 4 moves has been played played  
    When a player places a mark to complete 3 of the same in a row  
    Then that player is declared the winner
    
**Scenario 3:** No winner  
    Given a game is in play  
    And all turns have been played  
    When no player has 3 of the same marks in a row  
    Then the game is declared a draw
    
----

### Consider using your computer OR, use an online tool

- simple online tool is called [repl.it Java Compiler](https://repl.it/Fazy/5)
- Check if you have Java compiler. Run at the Command Prompt: javac -version
- You can download Java to run local from [Oracle here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (you'll have to select `Accept License Agreement` before you can click the [Mac OS X version](http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-macosx-x64.dmg)
- Open up an editor and create HelloWorld source file.  (vi, nano, textedit, eclipse, intellij)
  - To use the Mac's TextEdit run at the command line: open -e HelloWorld.java
      - NOTE: TextEdit is tricky!
      - You must 1st toggle to Plain Text:  Shift + Command + T
      - Then, File-Rename back to .java

- If you don't have Java running local or want to use an online tool, try: [Compile and Execute Java online with CodingGround](http://www.tutorialspoint.com/compile_java8_online.php) ..NOTE: I can share a link of completed code at any point if needed.

### Copy & Paste the following:

``` java
  public class HelloWorld{
     public static void main(String []args){
        System.out.println("Hello World");
     }
  }
```

- compile at the command line with: javac HelloWorld.java
- run this with: java HelloWorld

### Tic Tac Toe game:
- Now, we'll create a file called: TicTacToe.java
- Copy this partly complete Java source code:

```java
import java.util.Scanner;

class TicTacToe {

    private char[][] board; 
    private char currentPlayerMark;
			
    public TicTacToe() {
        board = new char[3][3];
        currentPlayerMark = 'x';
        initializeBoard();
    }
	
	
    // Set/Reset the board back to all empty values.
    public void initializeBoard() {
		
        // Loop through rows
        for (int i = 0; i < 3; i++) {
			
            // Loop through columns
            for (int j = 0; j < 3; j++) {
                board[i][j] = ' ';
            }
        }
    }
	
	
    // Print the current board (may be replaced by GUI implementation later)
    public void printBoard() {
        System.out.println("-------------");
		
        for (int i = 0; i < 3; i++) {
            System.out.print("| ");
            for (int j = 0; j < 3; j++) {
                System.out.print(board[i][j] + " | ");
            }
            System.out.println();
            System.out.println("-------------");
        }
    }
	
	
    // Loop through all cells of the board and if one is found to be empty (contains char '-') then return false.
    // Otherwise the board is full.
    public boolean isBoardFull() {
        boolean isFull = true;
		
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == ' ') {
                    isFull = false;
                }
            }
        }
		
        return isFull;
    }
	
	
    // Returns true if there is a win, false otherwise.
    // This calls our other win check functions to check the entire board.
    public boolean checkForWin() {
        return (checkRowsForWin() || checkColumnsForWin() || checkDiagonalsForWin());
    }
	
	
    // Loop through rows and see if any are winners.
    private boolean checkRowsForWin() {
        for (int i = 0; i < 3; i++) {
            if (checkRowCol(board[i][0], board[i][1], board[i][2]) == true) {
                return true;
            }
        }
        return false;
    }
	
	
    // Loop through columns and see if any are winners.
    private boolean checkColumnsForWin() {
        for (int i = 0; i < 3; i++) {
            if (checkRowCol(board[0][i], board[1][i], board[2][i]) == true) {
                return true;
            }
        }
        return false;
    }
	
	
    // Check the two diagonals to see if either is a win. Return true if either wins.
    private boolean checkDiagonalsForWin() {
        return ((checkRowCol(board[0][0], board[1][1], board[2][2]) == true) || (checkRowCol(board[0][2], board[1][1], board[2][0]) == true));
    }
	
	
    // Check to see if all three values are the same (and not empty) indicating a win.
    private boolean checkRowCol(char c1, char c2, char c3) {
        return ((c1 != ' ') && (c1 == c2) && (c2 == c3));
    }
	
	
    // Change player marks back and forth.
    public void changePlayer() {
        if (currentPlayerMark == 'x') {
            currentPlayerMark = 'o';
        }
        else {
            currentPlayerMark = 'x';
        }
    }
	
	
    // Places a mark at the cell specified by row and col with the mark of the current player.
    public boolean placeMark(int row, int col) {
		
        // Make sure that row and column are in bounds of the board.
        if ((row >= 0) && (row < 3)) {
            if ((col >= 0) && (col < 3)) {
                if (board[row][col] == ' ') {
                    board[row][col] = currentPlayerMark;
                    return true;
                }
            }
        }
		System.out.println("Not allowed!");
        return false;
    }
    
    public void makeMove(int r, int c) {
      placeMark(r-1,c-1);
      if (checkForWin()) {
      	System.out.println("Game over, player (" + currentPlayerMark + ") wins!");
      }
      changePlayer();
      printBoard();
    }
}
```

### Create main()

```java
public static void main(String[] args) {
        System.out.println("Tic Tac Toe");
}
```

- Same as HelloWorld, right? However, we now have all those methods we can call.

#### Let's instantiate the game
TicTacToe game = new TicTacToe();

#### Create an object to accept inputs
Scanner scanner = new Scanner(System.in);

#### Handle exception errors
You'll notice if you try to compile Java says an Exception is not handled. This is because the Scanner class
`throws Exception`

#### Prompt first player
System.out.print("Player 'X', what is your move? Enter row [1-3]: ");

#### Save the user's input
int r = scanner.nextInt();

#### Prompt for the column
System.out.print("Now enter column [1-3]");

#### Save the next variable
int c = scanner.nextInt();

#### Now, request the game to make the move!
game.makeMove(r-1,c-1);

### Main() should now look like this:

```java
public static void main(String[] args) {
        System.out.println("Tic Tac Toe");
        TicTacToe game = new TicTacToe();
        Scanner scanner = new Scanner(System.in);

        System.out.print("Player 'X', what is your move? Enter row [1-3]: ");
        int r = scanner.nextInt();
        System.out.print("Now enter column [1-3]");
        int c = scanner.nextInt();
        game.makeMove(r,c);
    }
```

- How would you use a loop to make this easier to finish?

#### Questions?
- Review the call to makeMove()
- what if we entered a non-integer?
- what if we entered a number outside the range [1-3]?
- could we select which user we want to start, ie; 'X' or 'O'?
- could we play against the computer?
  - what would that logic look like?
  - could we use a random number generator for the range 1-3?
  - how could we store various logic methods for computer's play decisions?

- If time we can review all the methods or other questions!

## Thank-you for coming today!

