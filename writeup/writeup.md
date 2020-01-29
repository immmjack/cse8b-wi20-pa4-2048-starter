# PA 4: 2048

## Assignment Overview
**Due date: Thursday, February 06 @ 11:59PM**
Note that this due date is not Tuesday as the calendar initially suggested. We are extending it to give some slack since there is some overlap with PA3, but don't wait to get started, start early and start often! 


## Getting Started
There are a number of ways to get started on development. The following is the recommended way to ensure that your code will compile during grading.

1. If you are using your own machine or are on a lab computer to complete the assignment, go to step 2 directly. Otherwise, ssh into your cs8bwi20 account. 
    - `ssh cs8bwi20__@ieng6.ucsd.edu`
2. Acquire the starter files.
    - From ieng6: 
        - Log in to your cs8bwi20 account. 
        - From the command line, use the command `cp -r ~/../public/pa4 ~/` (this will copy the entire starter files directory to your home directory)
        - Type `ls ~` to verify that you have copied the `pa4` directory over. 
    - From GitHub: 
        - `git clone https://github.com/CaoAssignments/cse8b-wi20-pa4-2048-starter.git`
        - Alternatively, you can download the repo as a zipped folder.
3. If you downloaded the repo as a zipped folder, navigate to it through your terminal or text editor (Atom, Eclipse, etc.). If you git cloned the repo, you can switch into that directory immediately.
    - `cd cse8b-wi20-pa4-2048-starter`
    - Optional: You may choose to rename this repo. You can do this by using this command:
        - `mv cse8b-wi20-pa4-2048-starter pa4`
4. You can now start working on it through vim using the following command or open the directory in your preferred editor.
    - `vim GameState.java` or `gvim GameState.java`
5. To compile your code, use the `javac` command.
    - Syntax: `javac file1.java file2.java etc...`
    - Example: `javac GameState.java`
6. To run your code, use the `java` command passing in the name of the class with the main method that you want to run.
    - Syntax: `java nameOfClass`
    - Example: `java GameState`

## 2048 Background
2048 is a single-player puzzle game created in March 2014 by 19-year-old Italian web developer Gabriele Cirulli, in which the objective is to slide numbered tiles on a grid to combine them and create a tile with the number 2048.  Cirulli created the game in a single weekend as a test to see if he could program a game from scratch.  2048 was an instant hit when the game received over 4 million visitors in less than a week.

The original version of the game was programmed in JavaScript.  You will be creating a Java version of the 2048 game that can be played in the terminal (text based).  Here is a link if you don’t know of this game http://2048game.com/.  Your version of the game will have the exact same functionality as the original version of the game.

In this PA you will be implementing the infrastructure of the game. 

## Basic functionalities
2048 is played on a simple grid of size NxN (4x4 by default), with numbered tiles (all powers of 2) that will slide in one of 4 directions (UP, DOWN, LEFT, RIGHT) based on input from the user.  Every turn, a new tile will randomly appear in an empty spot on the board with a value of either 2 or 4 (determined randomly with a 90% probability of being a tile with value 2).  Tiles will slide as far as possible in the chosen direction until they are stopped by either another tile or the edge of the grid.  If two tiles of the same number collide while moving, they will merge into a tile with a total value of the two tiles that collide.  For example, if two tiles of value 8 slide into one another the resulting tile will have a value of 16.  The resulting tile cannot merge with another tile again in the same move.

A score will also be kept based on the user’s performance.  The user’s score starts at zero, and is increased whenever two tiles combine, by the value of the newly combined tile.  For example, if two tiles of value 8 are merged together, the resulting tile will have a value of 16 and the user’s score will be increased by 16.

## Provided Files
_**DO NOT MODIFY THESE FILES.**_ We will use our own copy when grading, so any modifications you make will not be carried over. The only file you should change is `Config.java` but do not add anything that does not already exist in the file; you should only change the value of these constants to test your code. 

### Config.java
This file contains constants and settings for playing the game. These are magic numbers that define the configuration of the game, so you must place your own magic numbers in their respective files. 

### Direction.java
This file contains an enum. You can read the file for more information. 

### Game2048.java 
This file implements functionality for playing the game. You don't need to know how this file works to write your code in `GameState.java`. 

### PlayManager.java
This file starts and plays the game. 

## **Your Task: GameState.java**
You will be writing a file called `GameState.java`. You will need to create this file from scratch. Make sure that it contains a public class called `GameState` with the following functionality. Since we are not giving you starter code, you should make sure your file follows the strict method signatures we give to you, otherwise the autograder will not be able to run. We have defined what each method is required to do, but you are free to implement it however. In particular, we will only be grading based on what a function call does to the returned value of `getBoard()` and `getScore()` and what the called function returns (see below), so you are free to store the backend representation of your game however you wish. 

**NOTE**: Some of these methods are **REQUIRED**. If you do not implement them, you will fail their respective autograder tests. Make sure that your method signatures for these are exactly as noted on the writeup - this is the most likely reason your code "compiles on your own computer but not on Gradescope." 

### **OPTIONAL**: Instance Variables
There are three suggested instance variables (you are free to use whatever variables you think might help you, these are what we used): 
```java
private Random rng; 
private int[][] board; 
private int score; 
```
- `rng`: we used this everytime we wanted to generate a number so that we didn't need to create a new `Random` object every time. 
- `board`: we used this to keep track of the board. For tiles that had no value (empty), we had a 0 in `board`. 
- `score`: we used this to keep track of the score

### **REQUIRED**: `public String toString ()` 
Add the following code to your `GameState.java` so your game board will be displayed in the correct format. You may have to fix the style of this code to consistent with your own styling. Do not implement your own `toString()` method otherwise your game may have undefined behavior. 
```java
// REQUIRED - put this in your GameState.java
public String toString () {
    StringBuilder outputString = new StringBuilder();
    outputString.append(String.format("Score: %d\n", getScore()));
    for (int row = 0; row < getBoard().length; row++) {
        for (int column = 0; column < getBoard()[0].length; column++) {
            outputString.append(getBoard()[row][column] == 0 ? "    -" :
                String.format("%5d", getBoard()[row][column]));
        }
        outputString.append("\n");
    }
    return outputString.toString();
}
```
You **must** add this method to your `GameState.java` and **do not** implement your own `toString()`. 

### Constructor
```java 
public GameState (int numRows, int numCols); // REQUIRED
```

This is the sole constructor for the GameState class. To implement it, you will use the parameters passed in to initialize your instance variables.
In this constructor, you should:

1. Create a board with `numRows` rows and `numCols` columns. 
2. Set your starting score to 0. 

If you choose to create your `Random` object here, you may want to use the variable `RANDOM_SEED` in `Config.java` as the argument (seed). The sequence of random numbers generated by the `Random` object is solely determined by the value of the seed, meaning that each time you run the program you will get the same sequence of numbers. If you do not specify a seed, then a seed is used depending on the literal time of the initialization of the object. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

### Getters and Setters
```java
public int[][] getBoard (); // REQUIRED
public void setBoard (int[][] newBoard); // REQUIRED
public int getScore (); // REQUIRED
public void setScore (int newScore); // REQUIRED
```

#### REQUIRED: `public int[][] getBoard ()`
In this method, you should return a **deep copy** of the board of GameState class. Although you are free to represent your board however you want, we require the return board to be a 2d-array with the appropriate number of rows and columns. Each element of the 2d-array should either be the value of the tile or 0 if the tile does not exist. This method should not make any changes to the game state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `public void setBoard (int[][] newBoard)`
In the method, you should make a deep copy of the input board and set the board of GameState class to be the deep-copied board. For `null` inputs, make no change and return immediately. This method should not make any changes to the game state other than setting the new board state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `public int getScore ()`
Return the current score. This method should not make any changes to the game state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `public void setScore (int newScore)`
Sets the current score to be the input score. Note that this should also allow us to set a negative score. This method should not make any changes to the game state other than setting the score. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

### Generating Tiles
```java
protected int rollRNG (int bound); // optional
protected int randomTile (); // REQUIRED
protected int countEmptyTiles (); // REQUIRED
protected int addTile (); // REQUIRED
```

#### OPTIONAL: `protected int rollRNG (int bound)`
Return a random integer between 0(inclusive) and bound(exclusive). You should use the `nextInt()` method of the `Random` class. 

#### REQUIRED: `protected int randomTile ()`
In the game of 2048, a tile numbered either 2 or 4 is added to the board after each successful move/slide. You should be using this method to decide which  kind of tile should be added to the board whenever a new tile is needed. Return the int value `2` with probability `TWO_PROB` (from the `Config.java` file, where `TWO_PROB=70` means 70% chance) and `4` with the remaining probabilty. This method should not make any changes to the game state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `protected int countEmptyTiles ()`
Return the number of empty tiles on the board. This method should not make any changes to the game state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `protected int addTile ()`
In this method, add a new tile to the board. You should return the type of the tile added, either 2 or 4 (this must be whatever `randomTile()` returns to us). Note that we might theoretically call this method on a full board, in which case we wouldn't be able to add any tiles. Return 0 if you are unable to add any tile. This method should not make any changes to the game state other than adding the new tile. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

1. First, you should check whether there are empty tiles or not on the board. If there is not empty tile, don't add any tiles to the board and return 0. If there are empty tiles, continue to step 2. 
2. Randomly choose a tile out of all the empty tiles (you can use `rollRNG()` to help you choose the spot to add the tile). Then, add the tile decided by `randomTile()`, and add it to the board. Return the type of the tile just added to the board (i.e. return `2` if you added a 2 and return `4` if you added a 4). 

### Movement
```java
protected void rotateCounterClockwise (); // optional
protected boolean canSlideDown (); // REQUIRED
public boolean isGameOver (); // REQUIRED
protected boolean slideDown (); // REQUIRED
public boolean move (Direction dir); // REQUIRED
```

#### OPTIONAL: `protected void rotateCounterClockwise ()`
Rotate the board counter-clockwise once. You may want to read the specification for the `move()` method before deciding whether or not to implement this method. This should be very similar to how you did the transposition for PA3. You are not required to implement this method. 

#### REQUIRED: `protected boolean canSlideDown ()`
Use this method to determine if you can slide down. You will return `true` if sliding down is possible and `false` otherwise. This method should not make any changes to the game state. 

In particular, there are two cases where sliding down is possible:

1. If two tiles can be combined. Two tiles can be combined if they share the same value and there are no other tiles between them (empty tiles can be between them). 
2. If there is an empty tile below a non-empty tile. 

More precisely, this method should return `true` if `slideDown()` (defined below) should change the state of the board and return `false` if `slideDown()` should not be able to change the state of the board. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `public boolean isGameOver ()`
A game is over when you cannot move in any direction. To implement `isGameOver()`, use `rotateCounterClockwise()` and `canSlideDown()` to see if movement in any direction is possible. If any movement is possible, return `false` as the game is not over. Otherwise, return `true` to indicate that there are no more possible moves and that the game is over. This method should not make any changes to the game state. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

#### REQUIRED: `protected boolean slideDown ()`
Slide the board down and return whether the sliding was successful (if it changed the board). This method should also increase the score for this game, if applicable. 

This method will return `true` if sliding down is successful (i.e. the move was possible and the board changed after the sliding is completed) and return `false` otherwise (the board didn't change). 

To slide down, there are few things you need to implement in order for the game to work. 

One thing you might notice is that all the empty spaces on the board can be represented by `0`s. The functionalities of movement in any direction is the same. Here is what you should implement for each column of the board to slide down.

1. Tiles slide downward as far as they can. Remember that zeros (if you chose to represent it like this) mean there is no tile in that spot, therefore, tiles can slide past them. Tiles should stop sliding if they encounter another tile or if they encounter the edge of the board. However, if two tiles touch while sliding, then: 
1. Merge two tiles with the same number. The tiles merge into a combined tile. The combined tile's number is the sum of the two tiles. Increase the score by the combined tile's number. The combined tile should be where the bottom tile was and the top tile will become `0`. This combined tile cannot merge with any other tile in the same move. Suppose for the following examples that the listed tiles are from top to bottom. Suppose we had 4 4 4. This would combine to be 0 4 8 (note that it is not 0 8 4, in other words, the farthest down tiles combine first). If we had 4 4 4 4, this would combine to be 0 0 8 8. 

You **must** implement this method with the given method signature and it **must** have the specified behavior. 

Here are some examples and a gif for you to visualize the basic movements of our 2048.

Example 1:
Should return `true`
```
0 0 2         0 0 0
0 0 0    ->   0 0 0
0 0 0         0 0 2
```
Example 2:
Should return `true`
```
0 0 0         0 0 0
0 0 2    ->   0 0 0 
0 0 2         0 0 4
```

Example 3: 
Should return `true`
```
0 0 2         0 0 0
0 0 2    ->   0 0 2
0 0 2         0 0 4
```

Example 4:
Should return `false`
```
0 0 0         0 0 0
0 0 4    ->   0 0 4
0 0 8         0 0 8
```

An example game may be: 
```
[cs8bwi20XX@ieng6-203]:pa4: java PlayManager
Starting a default-sized random game...
Score: 0
    -    -    -    -
    -    -    -    -
    2    -    -    2
    -    -    -    -
> d
Score: 4
    -    -    -    -
    -    -    -    -
    -    -    -    4
    -    -    2    -
> s
Score: 4
    -    -    -    -
    -    -    -    -
    -    -    -    -
    2    -    2    4
> a
Score: 8
    -    -    -    -
    -    -    -    -
    -    -    4    -
    4    4    -    -
> w
Score: 8
    4    4    4    -
    -    -    -    -
    -    -    -    -
    -    -    -    2
> a
Score: 16
    8    4    -    -
    -    -    2    -
    -    -    -    -
    2    -    -    -
> a
Score: 16
    8    4    -    -
    2    -    -    2
    -    -    -    -
    2    -    -    -
> s
Score: 20
    -    -    -    -
    -    -    -    -
    8    -    -    4
    4    4    -    2
> q
```

#### REQUIRED: `public boolean move (Direction dir)`
This method should slide the board in the specified direction. If the sliding is done successfully, then a new random tile must be added to the board using your `addTile()` method. This method should return whether the move was successful or not. If `dir` is null, return `false` without doing anything. 

You **must** implement this method with the given method signature and it **must** have the specified behavior.

**Note**: If you implemented the `rotateCounterClockwise()` method then here is where its power shines. You can combine `rotateCounterClockwise()`, the `getRotationCount()` method from the `Direction` enum, and the `slideDown()` method to implement this method for all directions. 

## How to Play Your Game
Compile your code with `javac *.java`. Then, use `java PlayManager` to load a default game. Type one of `wasd` to determine the direction to slide (w is up, a is left, s is down, and d is right), `o` to save the game to a file called `save.2048`, or `q` to exit. You can also type `java PlayManager [SAVEFILE]` to load a game from `[SAVEFILE]`. 

## README
The following questions about provided files and general policies are graded for fair effort and completeness. It is recommended to look at gradescope rubric to see our suggested answers afterward even if you get full credit. 

1. Describe the game of 2048 to someone that doesn't know the game. **Do not use any Java or CSE terms**. Write concisely as if your reader needs to read 10 summaries in a row. You might want to describe how to play the game with specific keys and what the ultimate goal is.
1. What are the benefits of having all our magic numbers in the `Config.java` file? In general, what is the purpose of setting these magic numbers in a scope accessible by all your code (e.g. at the top of your file inside the class)? 
1. In `GameState.java` we had to return a **deep** copy of the board in `getBoard()`. Why might we want to create a deep copy rather than a shallow copy of the board if we only need *read* access to it? 

### Student Satisfaction Survey
Please fill out our [student satisfaction survey](https://docs.google.com/forms/d/e/1FAIpQLSev1PfOzif75xqN7LWbOkU4PHJ_pJwPC0v_SJwiMoxOlvQ6Qw/viewform). We are changing how we approach giving assignments and would like to hear about your experiences. 


## Style
We will grade your code style thoroughly. Namely, there are a few things you must have in each file / class / method:

1. File header
2. Class header
3. Method header(s)
4. Inline comments
5. Proper indentation
6. Descriptive variable names
7. No magic numbers
8. Reasonably short methods (if you have implemented each method according to specification in this write-up, you’re fine). This is not enforced as strictly.
9. Lines shorter than 80 characters (keep in mind each tab is equivalent to 8 spaces).
10. Javadoc conventions (@param, @return tags, /** comments */, etc.)

A full [style guideline](https://sites.google.com/eng.ucsd.edu/cse8b-winter2020/style-guide) can be found here. If you need any clarifications, feel free to ask on Piazza.


## Submission
Submit the following files to Gradescope. Make sure to wait until you see the autograder output. 
Required Submission Files: 
- `GameState.java`
- `README.md`

*Start early and start often!*