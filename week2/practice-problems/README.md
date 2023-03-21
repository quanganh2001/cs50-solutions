# Hours
Suppose you’re taking CS50 (if you’re reading this you probably are!) and spending time every week on each problem set. You may be wondering how many hours you’ve spent learning computer science, on average or in total! In this program, you’ll complete a function that calculates, based on a user’s input, a total number of hours or an average number of hours across a given number of days.

## Implementation Details
The main function prompts the user for the number of weeks a user has been taking CS50, then creates an array with as many elements. Notice that, after retrieving some data, the program prompts the user to type in either “T” or “A”—”T” should (but doesn’t yet!) print the total number of hours the user entered, while “A” should (but doesn’t yet!) print the average hours the user entered. Notice that the `do while` loop uses `toupper` to capitalize the letter that’s input before it is saved in the variable output. Then, the `printf` function calls `calc_hours`. Note the syntax involved when passing an array to a function.

To complete calc_hours, first total up the hours saved in the array into a new variable. Then, depending on the value of output, return either this sum, or the average number of hours.

## How to do?
To complete the `calc_hours` function, you need to do the following:

1. Total up the hours saved in the array into a new variable.
2. Depending on the value of output, either return this sum or the average number of hours.

Here's one way to implement the `calc_hours` function:
```c
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>

float calc_hours(int hours[], int weeks, char output);

int main(void)
{
    int weeks = get_int("Number of weeks taking CS50: ");
    int hours[weeks];

    for (int i = 0; i < weeks; i++)
    {
        hours[i] = get_int("Week %i HW Hours: ", i);
    }

    char output;
    do
    {
        output = toupper(get_char("Enter T for total hours, A for average hours per week: "));
    }
    while (output != 'T' && output != 'A');

    printf("%.1f hours\n", calc_hours(hours, weeks, output));
}

// TODO: complete the calc_hours function
float calc_hours(int hours[], int weeks, char output)
{
    int total_hours = 0;
    for (int i = 0; i < weeks; i++)
    {
        total_hours += hours[i];
    }

    float result = 0.0;
    if (output == 'T')
    {
        result = total_hours;
    }
    else if (output == 'A')
    {
        result = (float)total_hours / weeks;
    }

    return result;
}
```

How to complier code: `gcc -o hours hours.c cs50.c`

Output:
```txt
hours/ $ ./hours
Number of weeks taking CS50: 3
Week 0 HW Hours: 3
Week 1 HW Hours: 7
Week 2 HW Hours: 10
Enter T for total hours, A for average hours per week: A
6.7 hours

hours/ $ ./hours
Number of weeks taking CS50: 2
Week 0 HW Hours: 2
Week 1 HW Hours: 8
Enter T for total hours, A for average hours per week: T
10.0 hours
```
The function starts by initializing a new variable `total_hours` to 0. It then loops through the array hours and adds up all the hours into `total_hours`.

Next, the function checks the value of output. If it is equal to 'T', the function returns `total_hours`. If it is equal to 'A', the function calculates the average number of hours by dividing `total_hours` by `weeks`. Note that we cast total_hours to a float to ensure that the division is a floating-point division. Finally, the function returns the result.

This implementation assumes that the user enters either 'T' or 'A' as input. If the user enters any other character, the function will still return a value, but the value may not be meaningful. To handle this case, you could add additional error checking code in the main function to ensure that output is either 'T' or 'A'.
# N0 V0w3ls
If you’ve been on the internet, you might have seen “[leetspeak](https://en.wikipedia.org/wiki/Leet)” (or “l33tsp36k” for our purposes!), which involves the substitution of symbols for alphabetical characters, where those symbols somewhat resemble their alphabetical counterparts. In this lab, you’ll write a program to replace certain vowels with digits!

Up until now, you’ve frequently written programs for which you’ve been provided distribution code. You’ll notice when downloading the “distro” for this problem, you start with nothing more than a couple of commonly used libraries and an empty main function. In this problem, you will convert a word, which you will input at the command line, to a corresponding word with numbers replacing vowels.

## Implementation Details
- Implement your program in a file called `no-vowels.c` in a directory called `no-vowels`.
- Your program must accept a single command-line argument, which will be the word that you want to convert.
- If your program is executed without any command-line arguments or with more than one command-line argument, your program should print an error message of your choice (with `printf`) and return from `main` a value of 1 (which tends to signify an error) immediately.
- Your program must contain a function called `replace` which takes a `string` input and returns a `string` output.
- This function will change the following vowels to numbers: a becomes 6, e becomes 3, i becomes 1, o becomes 0 and u does not change.
- The input parameter for the `replace` function will be `argv[1]` and the return value is the converted word.
- The main function will then print the converted word, followed by \n.
- You may want to try using the switch statement in your `replace` function.
## How to do?
```js
// Write a function to replace vowels with numbers
// Get practice with strings
// Get practice with command line
// Get practice with switch

#include <cs50.h>
#include <stdio.h>
#include <string.h>
#include <ctype.h>

string replace(string input);

int main(int argc, string argv[])
{
    if(argc != 2){
        printf("Wrong command-line arguments\n");
        return 1;
    }
    string word = argv[1];

    string result = replace(word);

    printf("%s\n", result);
}

string replace(string input) {
    string output = input;

    for(int i = 0; i < strlen(input); i++) {
        char c = tolower(input[i]);

        switch (c)
        {
            case 'a':
                output[i] = '6';
                break;

            case 'e':
                output[i] = '3';
                break;

            case 'i':
                output[i] = '1';
                break;

            case 'o':
                output[i] = '0';
                break;

            default:
                output[i] = input[i];
                break;
        }
    }

    return output;
}
```

How to complier code: `gcc -o no-vowels no-vowels.c cs50.c`

Output:
```txt
no-vowels/ $ ./no-vowels hello
h3ll0

no-vowels/ $ ./no-vowels pseudocode
ps3ud0c0d3
```
# Password
As we all know by now, it’s important to use passwords that are not easy to guess! Many web apps now require passwords that require not only alphabetical characters, but also number and symbols.

In this lab, the user is prompted for a password, which will then be validated using a function check that you will complete. If the password contains at least one upper case letter, one lower case letter, a number, and a symbol (meaning a printable character that’s not a letter or number, such as ‘!’, ‘$’ and ‘#’), the function should return true. If not it should return false.

## Implementation Details
Your function will iterate through the password that’s supplied to it as an argument. Since you have to find at least one lower case letter, one upper case letter, one number and one symbol, you may want to create a boolean variable for each and set each to false before you iterate through the string. If you then find a number, for instance you can set that boolean to true. If all booleans are true at the end of the function, it means all criteria are met, and you would return true.

## How to solve?
1. Check four variables with value 'false': checkLower, checkUpper, checkNumber and checkSymbol
2. Loop through the string

    a. Check if there is a lower case letter. If so, change the variable checkLower to true

    b. Check if there is a upper case letter. If so, change the variable checkUpper to true

    c. Check if there is a number. If so, change the variable checkNumber to true

    d. Check if there is a symbol. If so, change the variable checkSymbol to true

3. Check if the four variables are true. If so, return true. Otherwards, return false
```c
// Check that a password has at least one lowercase letter, uppercase letter, number and symbol
// Practice iterating through a string
// Practice using the ctype library

#include <cs50.h>
#include <stdio.h>
#include <string.h>
#include <ctype.h>

bool valid(string password);

int main(void)
{
    string password = get_string("Enter your password: ");
    if (valid(password))
    {
        printf("Your password is valid!\n");
    }
    else
    {
        printf("Your password needs at least one uppercase letter, lowercase letter, number and symbol\n");
    }
}

// TODO: Complete the Boolean function below
bool valid(string password)
{
    bool checkLower = false;
    bool checkUpper = false;
    bool checkNumber = false;
    bool checkSymbol = false;

    for(int i = 0; i < strlen(password); i++) {
        if(islower(password[i])) {
            checkLower = true;
        }
        if(isupper(password[i])) {
            checkUpper = true;
        }
        if(isdigit(password[i])) {
            checkNumber = true;
        }
        if(!isalnum(password[i])) {
            checkSymbol = true;
        }
    }

    if(checkLower == true && checkUpper == true && checkNumber == true && checkSymbol == true) {
        return true;
    }

    return false;
}
```

How to complier code: `gcc -o password password.c cs50.c`

Output:
```txt
Enter your password: h3ll(
Your password needs at least one uppercase letter, lowercase letter, number and symbol!

Enter your password: h3LL0!
Your password is valid!
```
# Full Source code
1. [hours](hours/hours.c)
2. [no-vowels](no-vowels/no-vowels.c)
3. [password](password/password.c)