*Note: please add cs50.c and cs50.h files to your practice problems or your labworks.*

# Debug
```c
// Become familiar wih C syntax
// Learn to debug buggy code

#include <stdio.h>
#include <cs50.h>

int main(void)
{
    // Ask for your name and where live
    string name = get_string("What is your name? ");
    string location = get_string("Where do you live? ");

    // Say hello
    printf("Hello, %s, from %s!", name, location);
}
```

How to complier code: `gcc -o debug debug.c cs50.c`

Output:
```txt
What is your name? Carter
Where do you live? Cambridge
Hello, Carter, from Cambridge!
```
# Half
```c
// Calculate your half of a restaurant bill
// Data types, operations, type casting, return value

#include <cs50.h>
#include <stdio.h>

float half(float bill, float tax, int tip);

int main(void)
{
    float bill_amount = get_float("Bill before tax and tip: ");
    float tax_percent = get_float("Sale Tax Percent: ");
    int tip_percent = get_int("Tip percent: ");

    printf("You will owe $%.2f each!\n", half(bill_amount, tax_percent, tip_percent));
}

// TODO: Complete the function
float half(float bill, float tax, int tip)
{
    // 1. Calculate tax amount
    float taxAmount = bill * (tax / 100);
    // 2. Calculate bill + tax amount
    float billAmount = bill + taxAmount;
    // 3. Calculate tip amount
    float tipAmount = billAmount * ((float)tip / 100);
    // 4. Calculate total amount
    float totalAmount = billAmount + tipAmount;
    // 5. Calculate and return half of the total amount
    return totalAmount / 2;
}
```

How to complier code: `gcc -o half half.c cs50.c`

Output:
```txt
Bill before tax and tip: 12.50
Sale Tax Percent: 8.875 
Tip percent: 20
You will owe $8.17 each!
```
# Prime
```c
#include <cs50.h>
#include <stdio.h>

bool prime(int number);

int main(void)
{
    int min;
    do
    {
        min = get_int("Minimum: ");
    }
    while (min < 1);

    int max;
    do
    {
        max = get_int("Maximum: ");
    }
    while (min >= max);

    for (int i = min; i <= max; i++)
    {
        if (prime(i))
        {
            printf("%i\n", i);
        }
    }
}

bool prime(int number)
{
    // TODO
    if(number <= 1) {
        return false;
    }
    if(number == 2 || number == 3) {
        return true;
    }
    for(int i = 2; i < number; i++) {
        if(number % i == 0) {
            return false;
        }
    }
    return true;
}
```

How to complier code: `gcc -o prime prime.c cs50.c`

Output:
```txt
Minimum: 1
Maximum: 100
2
3
5
7
11
13
17
19
23
29
31
37
41
43
47
53
59
61
67
71
73
79
83
89
97
```

See source codes on:
1. [Debug](debug/debug.c)
2. [Half](half/half.c)
3. [Prime](prime/prime.c)