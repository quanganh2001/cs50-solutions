# Readability
For this problem, you’ll implement a program that calculates the approximate grade level needed to comprehend some text, per the below.

## Specification
Design and implement a program, readability, that computes the Coleman-Liau index of text.

- Implement your program in a file called `readability.c` in a directory called `readability`.

- Your program must prompt the user for a `string` of text using `get_string`.

- Your program should count the number of letters, words, and sentences in the text. You may assume that a letter is any lowercase character from a to z or any uppercase character from A to Z, any sequence of characters separated by spaces should count as a word, and that any occurrence of a period, exclamation point, or question mark indicates the end of a sentence.

- Your program should print as output "Grade X" where X is the grade level computed by the Coleman-Liau formula, rounded to the nearest integer.

- If the resulting index number is 16 or higher (equivalent to or greater than a senior undergraduate reading level), your program should output "Grade 16+" instead of giving the exact index number. If the index number is less than 1, your program should output "Before Grade 1".

## How it works?

User inputs some text

Our program tells the user the reading level of the text is through the formula:

$$ index = 0.0588 * L - 0.296 * S - 15.8 $$

L = Number of letters per 100 words

S = Number of sentences per 100 words

So we first need to count the number of: Words, Letters and Sentences.

Recall that a string just an array of characters

To find out how many letters are in the text, we simply need to loop through each character checking whether or not it's a letter.

Is there a way to count the number of words in C? Let's use logic instead...

Words are separated by spaces, so we can simply count the number of spaces in the text!
```c
#include <cs50.h>
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <math.h>

int main(void)
{
    string text = get_string("Text: ");

    int letters = 0;
    int words = 1;
    int sentences = 0;

    for (int i = 0; i < strlen(text); i++)
    {
        if (isalpha(text[i]))
        {
            letters++;
        }

        else if (text[i] == ' ')
        {
            words++;
        }

        else if (text[i] == '.' || text[i] == '?' || text[i] == '!')
        {
            sentences++;
        }
    }

    float L = (float) letters / (float) words * 100;
    float S = (float) sentences / (float) words * 100;

    int index = round(0.0588 * L - 0.296 * S - 15.8);

    if (index < 1)
    {
        printf("Before Grade 1\n");
    }

    else if (index > 16)
    {
        printf("Grade 16+\n");
    }

    else
    {
        printf("Grade %i\n", index);
    }
}
```

How to complier code: `gcc -o readability readability.c cs50.c`

Output:
```txt
Text: Congratulations! Today is your day. You're off to Great Places! You're off and away!
Grade 3
```
# Bulbs
Each sequence of bulbs, though, encodes a message in *binary*, the language computers “speak.” Let’s write a program to make secret messages of your own, perhaps that we could even put on stage!

## Specification
Design and implement a program, bulbs, that converts text into instructions for the strip of bulbs on CS50’s stage as follows:

- Implement your program in a file called `bulbs.c`.

- Your program must first ask the user for a message using `get_string`.

- Your program must then convert the given string into a series of 8-bit binary numbers, one for each character of the string.

- You can use the provided `print_bulb` function to print a series of 0s and 1s as a series of yellow and black emoji, which represent on and off light bulbs.
- Each “byte” of 8 symbols should be printed on its own line when outputted; there should be a \n after the last “byte” of 8 symbols as well.
```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

const int BITS_IN_BYTE = 8;

void print_bulb(int bit);

int main(void)
{
    // TODO
    string message = get_string("Message: ");

    for (int i = 0; i < strlen(message); i++) {
        unsigned char byte = message[i];
        int binary[BITS_IN_BYTE];
        int quotient = byte;
        int remainder;

        for (int j = 0; j < BITS_IN_BYTE; j++) {
            remainder = quotient % 2;
            binary[j] = remainder;
            quotient /= 2;
        }

        for (int k = BITS_IN_BYTE - 1; k >= 0; k--) {
            print_bulb(binary[k]);
        }
        printf("\n");
    }
}

void print_bulb(int bit)
{
    if (bit == 0)
    {
        // Dark emoji
        printf("\U000026AB");
    }
    else if (bit == 1)
    {
        // Light emoji
        printf("\U0001F7E1");
    }
}
```

How to complier code in local computer: `gcc -o bulbs bulbs.c cs50.c`

Output:
```txt
Message: HI!
⚫🟡⚫⚫🟡⚫⚫⚫
⚫🟡⚫⚫🟡⚫⚫🟡
⚫⚫🟡⚫⚫⚫⚫🟡

Message: HI MOM
⚫🟡⚫⚫🟡⚫⚫⚫
⚫🟡⚫⚫🟡⚫⚫🟡
⚫⚫🟡⚫⚫⚫⚫⚫
⚫🟡⚫⚫🟡🟡⚫🟡
⚫🟡⚫⚫🟡🟡🟡🟡
⚫🟡⚫⚫🟡🟡⚫🟡
```
# Caesar
For this problem, you’ll implement a program that encrypts messages using Caesar’s cipher, per the below.

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <stdlib.h>

bool only_digits(string text);
char rotate(char p, int k);

int main(int argc, string argv[])
{
    int k, length;
    string plaintext;
    if (argc != 2 || !only_digits(argv[1])) {
        printf("Usage: ./caesar key\n");
        return 1;
    }
    k = atoi(argv[1]);
    plaintext = get_string("plaintext: ");
    length = strlen(plaintext);
    char ciphertext[length + 1];
    for (int i = 0; i< length; i++)
    {
        ciphertext[i] = rotate(plaintext[i], k);
    }
    ciphertext[length] = '\0';
    printf("ciphertext: %s\n", ciphertext);
    return 0;
}

bool only_digits(string text)
{
    int length;

    length = strlen(text);

    for (int i = 0; i < length; i++){
        if (!isdigit(text[i])) {
            return false;
        }
    }
    return true;
}
char rotate(char p, int k)
{
    char pi, c, ci;
    if (isupper(p)) {
        pi = p - 65;
        ci = (pi + k)%26;
        c = ci + 65;
    }
    else if (islower(p)) {
        pi = p - 97;
        ci = (pi + k)%26;
        c = ci + 97;
    }
    else {
        return p;
    }
    return c;

}
```

How to complier code in local computer: `gcc -o caesar caesar.c cs50.c`

Output:

Here are a few examples of how the program might work. For example, if the user inputs a key of 1 and a plaintext of hello:
```txt
./caesar 13    
plaintext: hello
ciphertext: uryyb
```

## Why?
Notice that the case of the original message has been preserved. Lowercase letters remain lowercase, and uppercase letters remain uppercase.
# Substitution
## What Will Be the Structure of Our Code?
1. Check that the user provides exactly **one** command-line argument (i.e. the key)

- If not, print `"Usage: ./Substitution key"`
```c
// Check that there is one command-line argument
if (argc != 2)
{
    printf("Usage: ./substitution key\n");
    return 1;
}
```
2. Validate that the key consists only of alphabets
- If not, print `"Usage: ./Substitution"`
```c
// Validate that the key consists only of alphabets
string key = argv[1];
for (int i = 0; i < strlen(key); i++)
{
    if (!isalpha(key[i]))
    {
        printf("Usage: ./substitution key\n");
        return 1;
    }
}
```
3. Validate that the key consists of 26 characters
- If not, print "Key must contain 26 characters."
```c
// Validate that the key consists of 26 characters
if (strlen(key) != 26)
{
    printf("Key must contain 26 characters.");
    return 1;
}
```
4. Validate that the key consists only of unique alphabets
- If not, print `"Usage: ./Substitution"`
```c
// Validate that each alphabet in the key is unique
for (int i = 0; i < strlen(key); i++)
{
    for (int j = i + 1; j < strlen(key); j++)
    {
        if (toupper(key[i]) == (toupper(key[j]))
        {
            printf("Usage: ./substitution key\n");
            return 1;
        }
    }
}
```
5. Prompt the user for the plaintext
```c
// Prompt the user for the plaintext
string plaintext = get_string("plaintext: ");
```
6. Print the ciphertext

Three scenarios to consider:
- If the plaintext is uppercase
- If the plaintext is lowercase
- If the plaintext is not an alphabet (e.g. a punctuation mark)

```c
// Convert all alphabets in the key to uppercase
for (int i = 0; i < strlen(key); i++)
{
    if (islower(key[i]))
    {
        key[i] = key[i] - 32;
    }
}

// Print the ciphertext
printf("ciphertext: ");

for (int i = 0; i < strlen(plaintext); i++)
{
    if (isupper(plaintext[i]))
    {
        int letter = plaintext[i] - 65;
        printf("%c", key[letter]);
    }
    else if (islower(plaintext[i]))
    {
        int letter = plaintext[i] - 97;
        printf("%c", key[letter] + 32);
    }
    else printf("%c", plaintext[i]);
}
printf("\n");
```

How to complier code in local computer: `gcc -o substitution substitution.c cs50.c`

Output:
```txt
./substitution YTNSHKVEFXRBAUQZCLWDMIPGJO
plaintext: HELLO
ciphertext: EHBBQ
```

# Wordle50
For this problem, you’ll implement a program that behaves similarly to the popular [Wordle](https://www.nytimes.com/games/wordle/index.html) daily word game.

## How do I solve?
1. In the first TODO, you should ensure the program accepts a single command-line argument. Let’s call it for the sake of discussion. If the program was not run with a single command-line argument, you should print the error message as we demonstrate above and return 1, ending the program.
```c
// ensure proper usage
// TODO #1
if(argc != 2) {
    printf("Usage: ./wordle wordsize\n");
    return 1;
}
```
 2. In the second TODO, you should make sure that
 is one of the acceptable values (5, 6, 7, or 8), and store that value in wordsize; we’ll need to make use of that later. If the value of is not one of those four values exactly, you should print the error message as we demonstrate above and return 1, ending the program.
 ```c
// ensure argv[1] is either 5, 6, 7, or 8 and store that value in wordsize instead
// TODO #2
for(int i =5; i <= 8; i++){
    if(atoi(argv[1]) == i){
        wordsize = i;
    }
}

if(wordsize == 0){
    printf("Error: wordsize must be either 5, 6, 7, or 8\n");
    return 1;
}
```
If the user instead does provide a command-line argument, but it’s not in the correct range:
```txt
./wordle 4
Error: wordsize must be either 5, 6, 7, or 8
```
3. For the third TODO, you should help defend against stubborn users by making sure their guess is the correct length. For that, we’ll turn our attention to the function `get_guess`, which you’ll need to implement in full. A user should be prompted (as via `get_string`) to type in a-letter word (remember, that value is passed in as a parameter to `get_guess`) and if they supply a guess of the wrong length, they should be re-prompted (much like in [Mario](https://cs50.harvard.edu/x/2023/psets/1/mario/less/)) until they provide exactly the value you expect from them. Right now, the distribution code doesn’t do that, so you’ll have to make that fix! Note that unlike the real Wordle, we actually don’t check that the user’s guess is a real word, so in that sense the game is perhaps a little bit easier. All guesses in this game should be in **lowercase** characters, and it is acceptable for you to assume that the user will not be so stubborn as to provide anything other than lowercase characters when making a guess. Once a legitimate guess has been obtained, it can be returned.
```c
string get_guess(int wordsize)
{
    string guess = "";

    // ensure users actually provide a guess that is the correct length
    // TODO #3
    do{
        guess = get_string("Input a 5-letter word: ");
    } while(strlen(guess) != 5);

    return guess;
}
```
4. Next, for the fourth TODO, we need to keep track of a user’s “score” in the game. We do this both on a per-letter basis—by assigning a score of 2 (which we `#define` d as `EXACT`) to a letter in the correct place, 1 (which we `#define` d as `CLOSE`) to a letter that’s in the word but in the wrong place, or 0 (which we `#define` d as `WRONG`)—and a per-word basis, to help us detect when we’ve potentially triggered the end of the game by winning. We’ll use the individual letter scores when we color-code the printing. In order to store those scores, we need an array, which we’ve called `status`. At the start of the game, with no guesses having taken place, it should contain all 0s.
```c
// set all elements of status array initially to 0, aka WRONG
// TODO #4
for(int k = 0; k < wordsize; k++){
    status[k] = WRONG;
}
```
This is another good place to stop and test your code, particularly as it pertains to item 3, above! You’ll notice that at this point, when you finally enter a legitimate guess (that is to say, one that’s the correct length), your program will likely look something like the below:
```txt
Input a 5-letter word: computer
Input a 5-letter word: games
Guess 1:
Input a 5-letter word:
```
That’s normal, though! Implementing print_word is TODO number 6, so we should not expect the program to do any processing of that guess at this time. Of course, you can always add additional printf calls (just make sure to remove them before you submit) as part of your debugging strategy!

5. The fifth TODO is definitely the largest and probably most challenging. Inside of the `check_word` function, it’s up to you to compare each of the letters of the `guess` with each of the letters of the `choice` (which, recall, is the “secret word” for this game), and assign scores. If the letters match, award `EXACT` (2) points and `break` out of the loop—there’s no need to continue looping if you already determined the letter is in the right spot. Technically, if that letter appears in the word twice, this could result in a bit of a bug, but fixing that bug overcomplicates this problem a bit more than we want to now, so we’re going to accept that as a feature of our version! If you find that the letter is in the word but not in the right spot, award `CLOSE` (1) points but don’t `break`! After all, that letter might later show up in the right spot in the `choice` word, and if we `break` too soon, the user would never know it! You don’t actually need to explicitly set `WRONG` (0) points here, since you handled that early in Step 4. Ultimately though, you should also be summing up the total score of the word when you know it, because that’s what this function is supposed to ultimately return. Again, don’t be afraid to use `debug50` and/or `printf`s as necessary in order to help you figure out what the values of different variables are at this point – until you implement `print_word`, below, the program won’t be offering you much in the way of a visual checkpoint!
```c
int check_word(string guess, int wordsize, int status[], string choice)
{
    int score = 0;

    // compare guess to choice and score points as appropriate, storing points in status
    // TODO #5

    // HINTS
    // iterate over each letter of the guess
    for(int i = 0; i < wordsize; i++){
        // iterate over each letter of the choice
        for(int j = 0; j < wordsize; j++)
        {
            // compare the current guess letter to the current choice letter
            if(guess[i] == choice[j]){
                // if they're the same position in the word, score EXACT points (green) and break so you don't compare that letter further
                if(i == j){
                    score += EXACT;
                    status[i] = EXACT;
                    break;
                }
                // if it's in the word, but not the right spot, score CLOSE point (yellow)
                else{
                    score += CLOSE;
                    status[i] = CLOSE;
                }
            }
        // keep track of the total score by adding each individual letter's score from above
        }
    }
    return score;
}
```
6. For the sixth TODO you will complete the implementation of `print_word`. That function should look through the values you populated the `status` array with and print out, character by character, each letter of the `guess` with the correct color code. You may have noticed some (scary-looking!) `#define`s at the top of the file wherein we provide a simpler way of representing what’s called an ANSI color code, which is basically a command to change the font color of the terminal. You don’t need to worry about how to implement those four values `(GREEN, YELLOW, RED`, and `RESET`, the latter of which simply returns to the terminal’s default font) or exactly what they mean; instead, you can just use them (the power of abstraction!). Note as well that we provide an example in the distribution code up where we print some green text and then reset the color, as part of the game’s introduction. Accordingly, you should feel free to use the below line of code for inspiration as to how you might try to toggle colors:
```c
void print_word(string guess, int wordsize, int status[])
{
    // print word character-for-character with correct color coding, then reset terminal font to normal
    // TODO #6
    for(int i = 0; i < wordsize; i++){
        if(status[i] == 2){
            printf(GREEN " %c " RESET, guess[i]);
        }
        else if(status[i] == 1){
            printf(YELLOW " %c " RESET, guess[i]);
        }
        else{
            printf(RED " %c " RESET, guess[i]);
        }
    }

    printf("\n");
    return;
}
```
7. Finally, the seventh TODO is just a bit of tidying up before the program terminates. Whether the main for loop has ended normally, by the user running out of guesses, or because we broke out of it by getting the word exactly right, it’s time to report to the user on the game’s outcome. If the user did win the game, a simple You won! suffices to print here. Otherwise, you should print a message telling the user what the target word was, so they know the game was being honest with them (and so that you have a means to debug if you look back and realize your code was providing improper clues along the way!)
```c
// Print the game's result
// TODO #7
if(won == true){
    printf("You won!\n");
} else{
    printf("You lose!\n The correct word was %s\n", choice);
}
```

How to complier code in local computer: `gcc -o wordle wordle.c cs50.c`

Output:
```txt
This is WORDLE50
You have 6 tries to guess the 5-letter word I'm thinking of
Input a 5-letter word: flowers
Input a 5-letter word: hello
Guess 1:  h  e  l  l  o 
Input a 5-letter word: hello
Guess 2:  h  e  l  l  o 
Input a 5-letter word: hello
Guess 3:  h  e  l  l  o 
Input a 5-letter word: board
Guess 4:  b  o  a  r  d 
Input a 5-letter word: board
Guess 5:  b  o  a  r  d 
Input a 5-letter word: guess
Guess 6:  g  u  e  s  s 
You lose!
 The correct word was asked
```

# Full source code
See all source code at:

1. [Readability](readability/readability.c)
2. [Bulbs](bulbs/bulbs.c)
3. [caesar](caesar/caesar.c)
4. [Substitution](substitution/substitution.c)
5. [Wordle50](wordle/wordle.c)