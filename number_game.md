#Number Game
We've all played a number-guessing game, where one player thinks of a number in a range and the other player must guess the number in as few guesses as possible.  Let's look at how we can create a similar game using Unity.

###Console Game
For the first iteration of this game, we'll design the game to run in the console, where we have text prompts to accept user input. The computer makes guesses and the user selects either the up-arrow, down-arrow to indicate the number is higher or lower than the current guess.  If the computer selects the correct answer, then the user hits the return key, and the game starts over.  

##Binary Search
We will write the program so that the computer makes the minimum number of guesses necessary.  At each step, the computer makes a guess that splits the number range in half.  This corresponds to (max + min) / 2.

