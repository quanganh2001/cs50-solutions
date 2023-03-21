*Note: please add cs50.c and cs50.h files to your practice problems or your labworks.*

# Mario less
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int height;
    do {
        height = get_int("Height: ");
    }
    while (height <= 0 || height > 8);

    for (int i = 0; i < height; i++) {
        for (int j = 0; j < height; j++) {
            if (i + j < height - 1) {
                printf(" ");
            } else {
                printf("#");
            }
        }
        printf("\n");
    }
}
```

How to complier: `gcc -o mario mario.c cs50.c`

Output:
```txt
Height: 8
       #
      ##
     ###
    ####
   #####
  ######
 #######
########
```

# Mario more
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int height, row, column, space;
    do
    {
        height = get_int("Enter height here: ");
    }
    while (height < 1 || height > 8);

    for (row = 0; row < height; row++)
    {
        for (space = 0; space < height - row - 1; space++)
        {
            printf(" ");
        }
        for (column = 0; column <= row; column++)
        {
            printf("#");
        }
        printf("  ");
        for (column = 0; column <= row; column++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

How to complier code: `gcc -o mario mario.c cs50.c`

Output:
```txt
Enter height here: 8
       #  #
      ##  ##
     ###  ###
    ####  ####
   #####  #####
  ######  ######
 #######  #######
########  ########
```

# Cash
```c
#include <cs50.h>
#include <stdio.h>

int get_cents(void);
int calculate_quarters(int cents);
int calculate_dimes(int cents);
int calculate_nickels(int cents);
int calculate_pennies(int cents);

int main(void)
{
    // Ask how many cents the customer is owed
    int cents = get_cents();

    // Calculate the number of quarters to give the customer
    int quarters = calculate_quarters(cents);
    cents = cents - quarters * 25;

    // Calculate the number of dimes to give the customer
    int dimes = calculate_dimes(cents);
    cents = cents - dimes * 10;

    // Calculate the number of nickels to give the customer
    int nickels = calculate_nickels(cents);
    cents = cents - nickels * 5;

    // Calculate the number of pennies to give the customer
    int pennies = calculate_pennies(cents);
    cents = cents - pennies * 1;

    // Sum coins
    int coins = quarters + dimes + nickels + pennies;

    // Print total number of coins to give the customer
    printf("%i\n", coins);
}

int get_cents(void)
{
    // TODO
    int cents;

    do
    {
        cents = get_int("Cents 0wed: ");
    }
    while (cents < 0);
    return cents;
}

int calculate_quarters(int cents)
{
    // TODO
    int quarters = 0;
    while (cents >= 25)
    {
        cents = cents - 25;
        quarters++;
    }
    return quarters;
}

int calculate_dimes(int cents)
{
    // TODO
    int dimes = 0;
    while (cents >= 10)
    {
        cents = cents - 10;
        dimes++;
    }
    return dimes;
}

int calculate_nickels(int cents)
{
    // TODO
    int nickels = 0;
    while (cents >= 5)
    {
        cents = cents - 5;
        nickels++;
    }
    return nickels;
}

int calculate_pennies(int cents)
{
    // TODO
    int pennies = 0;
    while (cents >= 1)
    {
        cents = cents - 1;
        pennies++;
    }
    return pennies;
}
```

How to complier code: `gcc -o cash cash.c cs50.c`

Output:
```txt
Cents 0wed: -41
Cents 0wed: foo
Cents 0wed: 41
4
```

# Credit
```c
#include <cs50.h>
#include <stdio.h>

bool check_sum(long num);

int main(void)
{
    int digits = 0, single_digit = 0, two_digit = 0;
    bool checksum;
    long user_input = get_long("Number: ");
    checksum = check_sum(user_input);
    if (checksum == false)
        return 0;
    while(user_input > 0)
    {
        if (user_input < 10)
        {
            single_digit = user_input;
        }
        if (user_input > 10 && user_input < 100)
        {
            two_digit = user_input;
        }
        user_input /= 10;
        digits++;
    }
    printf("digits: %i\n", digits);
    printf("single digit: %i\n", single_digit);
    printf("two digit: %i\n", two_digit);
    if ((two_digit == 34 || two_digit == 37) && digits == 15)
    {
        printf("AMEX\n");
        return 0;
    }
    else if ((two_digit == 51 || two_digit == 52 || two_digit == 53 || two_digit == 54 || two_digit == 55) && digits == 16)
    {
        printf("MASTERCARD\n");
        return 0;
    }
    else if (single_digit == 4 && (digits == 13 || digits == 16))
    {
        printf("VISA\n");
        return 0;
    }
    else
        printf("INVALID\n");
    return 0;
}

bool check_sum(long num)
{
    int total = 0, buffer = 0;
    bool var = true;
    while(num > 0)
    {
        if (var == true)
        {
            total += num % 10;
            num /= 10;
            var = false;
        }
        else
        {
            buffer = num % 10;
            buffer *= 2;
            if (buffer >= 10)
            {
                total += buffer % 10;
                total += buffer / 10;
            }
            else
            {
                total += buffer;
            }
            var = true;
            num /= 10;
        }
    }
    printf("total: %i\n", total);
    if (total % 10 == 0)
        return true;
    printf("INVALID\n");
    return false;
}
```
How to complier code: `gcc -o credit credit.c cs50.c`

Output:
```txt
Number: 4003-6000-0000-0014
Number: foo
Number: 4003600000000014
VISA
```

See source code on:
1. [Mario less](mario-less/mario.c)
2. [Mario more](mario-more/mario.c)
3. [Cash](cash/cash.c)
4. [Credit](credit/credit.c)