You are welcome to collaborate with one or two classmates on this lab, though it is expected that every student in any such group contribute equally to the lab.

# Implementation Details
Complete the implementation of `scrabble.c`, such that it determines the winner of a short scrabble-like game, where two players each enter their word, and the higher scoring player wins.

- Notice that we’ve stored the point values of each letter of the alphabet in an integer array named POINTS.

    - For example, A or a is worth 1 point (represented by `POINTS[0])`, B or b is worth 3 points (represented by `POINTS[1])`, etc.

- Notice that we’ve created a prototype for a helper function called `compute_score()` that takes a string as input and returns an `int`. Whenever we would like to assign point values to a particular word, we can call this function. Note that this prototype is required for C to know that `compute_score()` exists later in the program.
- In `main()`, the program prompts the two players for their words using the get_string() function. These values are stored inside variables named word1 and word2.
- In `compute_score()`, your program should compute, using the `POINTS` array, and return the score for the string argument. Characters that are not letters should be given zero points, and uppercase and lowercase letters should be given the same point values.

    - For example, ! is worth 0 points while A and a are both worth 1 point.
    - Though Scrabble rules normally require that a word be in the dictionary, no need to check for that in this problem!

- In `main()`, your program should print, depending on the players’ scores, Player 1 wins!, Player 2 wins!, or Tie!.
```c
#include <ctype.h>
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Points assigned to each letter of the alphabet
int POINTS[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

int compute_score(string word);

int main(void)
{
    // Get input words from both players
    string word1 = get_string("Player 1: ");
    string word2 = get_string("Player 2: ");

    // Score both words
    int score1 = compute_score(word1);
    int score2 = compute_score(word2);

    // TODO: Print the winner
    if (score1 > score2)
        printf("%s\n", "Player 1 wins!");
    else if (score1 < score2)
        printf("%s\n", "Player 2 wins!");
    else
        printf("%s\n", "Tie!");
}

int compute_score(string word)
{
    // TODO: Compute and return score for string
    int score = 0;

    // Compute score for each character
    int len = strlen(word);
    for (int i = 0; i < len; i++)
    {
        if (isupper(word[i]))
        {
            score += POINTS[word[i] - 'A'];
        }
        else if (islower(word[i]))
        {
            score += POINTS[word[i] - 'a'];
        }
    }
    return score;
}
```

How to complier code: `gcc -o scrabble scrabble.c cs50.c`

Output:
```txt
$ ./scrabble
Player 1: Question?
Player 2: Question!
Tie!
$ ./scrabble
Player 1: Oh,
Player 2: hai!
Player 2 wins!
$ ./scrabble
Player 1: COMPUTER
Player 2: science
Player 1 wins!
$ ./scrabble
Player 1: Scrabble
Player 2: wiNNeR
Player 1 wins!
```
