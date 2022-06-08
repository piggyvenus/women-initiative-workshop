# women-initiative-workshop
## Prerequisites
 - https://www.linkedin.com/learning/programming-foundations-fundamentals-3/the-fundamentals-of-programming?autoplay=true&u=2164418
 - https://www.pluralsight.com/courses/practical-python-beginners

## Set up
- Python Install [https://www.python.org/downloads/]
- IDE [https://code.visualstudio.com/download] or text editor
- Git for MacOS [https://git-scm.com/download/mac] or for Windows [https://git-scm.com/download/windows]


## Lab
This workshop will walk through you how to build hangman game using python program.
It is a simply command line game that allow player to ender the input via command-line.

### Get the files

```
git clone https://github.com/piggyvenus/women-initiative-workshop.git
```

### Library of words
A list of words are created for this program to randomly select a word for guessing.

### Let's start
We will need a function get_word() to pick a word from the words.py

```
def get_word():
```
Then, need to import random Library

```
import random
from words import word_list
```

Add code to pick a word and need to cast the word to upper case for simplify the comparison logic.
```
word = random.choice(word_list)
return word.upper()
```

Next, we need a function to play the game and call it play. The random picked word is for player to guess, so the word is the input of the function.
```
def play(word):
```

We need a variable that can hold the guessed word for the game, and its length should be the same as the chosen word. Initially, it should be “blank.”

```
word_completion = "_" * len(word)
```

Next, we need a variable call guessed and initialized as false
```
guessed = False
```

Now, let’s create 2 lists
- guessed_letters list that player guesses
- guessed_words list holds the words that player guessed

```
guessed_letters = []
guessed_words = []
```

Last variable is a variable to keep track of the numbers of tries that the player tried, which should be the same as the number of body parts to be draw for the hangman.

```
tries = 6
```

Instruction will provide the hangman drawing for each stage in a function called display_hangman().
Add this function after the function play(word).

```
def display_hangman(tries):
    stages = [  # final state: head, torso, both arms, and both legs
                """
                   --------
                   |      |
                   |      O
                   |     \\|/
                   |      |
                   |     / \\
                   -
                """,
                # head, torso, both arms, and one leg
                """
                   --------
                   |      |
                   |      O
                   |     \\|/
                   |      |
                   |     /
                   -
                """,
                # head, torso, and both arms
                """
                   --------
                   |      |
                   |      O
                   |     \\|/
                   |      |
                   |
                   -
                """,
                # head, torso, and one arm
                """
                   --------
                   |      |
                   |      O
                   |     \\|
                   |      |
                   |
                   -
                """,
                # head and torso
                """
                   --------
                   |      |
                   |      O
                   |      |
                   |      |
                   |
                   -
                """,
                # head
                """
                   --------
                   |      |
                   |      O
                   |
                   |
                   |
                   -
                """,
                # initial empty state
                """
                   --------
                   |      |
                   |
                   |
                   |
                   |
                   -
                """
    ]
    return stages[tries]
```

Now, let's back to the play() function. Let's print some help for the player.

```
tries = 6
print("Let's play Hangman!")
print(display_hangman(tries))
print(word_completion)
print("\n")
```

We need to check the guesses in a loop until the hangman is completed.

```
while not guessed and tries > 0:
```
Prompt player for input (word or letter) and store the guess in a variable. Please don't forget to cast it to upper().

```
guess = input("Please guess a letter or word: ").upper()
```
Next step is to create a logic to process 3 possible inputs from the user (guessing a word, a letter, or anything else)
Let's create an if-else block in the while loop.

```
if len(guess) == 1 and guess.isalpha():
#logic1 for this condition
elif len(guess) == len(word) and guess.isalpha():
#logic2 for this condition
else:
        print("Not a valid guess.")
```
Let's think about the logic1 condition. When the guess input is a letter, it can be:
1. if guessed letter is already guessed, print message.
2. if guessed letter is not in the picked word, tries -1, add to the guessed letter to the list.
3. if player guessed a correct letter, print "good job" message, tries -1, add guessed letter, update the correct guess in the word_list. Also need to compare the list to the word_list to the corresponding letter and update the guessed word and check the word is complete by check "_".
Replace this following block into the #logic1
```
  if guess in guessed_letters:
     print("You already guessed the letter", guess)
  elif guess not in word:
     print(guess, "is not in the word.")
     tries -= 1
     guessed_letters.append(guess)
  else:
     print("Good job,", guess, "is in the word!")
     guessed_letters.append(guess)
     word_as_list = list(word_completion)
     indices = [i for i, letter in enumerate(word) if letter == guess]
     for index in indices:
         word_as_list[index] = guess
     word_completion = "".join(word_as_list)
     if "_" not in word_completion:
         guessed = True
```

When the guess is a word, we check:
1. If the guess in the guessed word list
2. If the guess is not equal to the picked word, -1 tries
3. The guess is correct

```
  if guess in guessed_words:
     print("You already guessed the word", guess)
  elif guess != word:
     print(guess, "is not the word.")
     tries -= 1
     guessed_words.append(guess)
  else:
     guessed = True
     word_completion = word
```

For each try, you can print out the status for the hangman drawing and the guessed word.
```
print(display_hangman(tries))
print(word_completion)
print("\n")
```

When all tries are completed, print the games status.
```
if guessed:
   print("Congrats, you guessed the word! You win!")
else:
   print("Sorry, you ran out of tries. The word was " + word + ". Maybe next time!")
```

Create a function to select a word, play the game. Also, allows player to play again without restart the program

```
def main():
  word = get_word()
  play(word)
  while input("Play Again? (Y/N) ").upper() == "Y":
      word = get_word()
      play(word)
```

Add the code to call main when executing the script from command-line
```
if __name__ == "__main__":
    main()
```
