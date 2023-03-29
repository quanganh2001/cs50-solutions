*Note: please add cs50.c and cs50.h files to your practice problems and your labworks.*
# Recursive atoi
Image that you travel back in time to the 1970’s, when the C programming language was first created. You are hired as a programmer to come up with a way to convert strings to `int`s. (You may have used a function just like this in Week 2, called `atoi`). You want to be thorough in your development process and plan to try several approaches before deciding on the most efficient.

In this problem, you will start with a simple implementation of `atoi` that handles positive `int`s using loops. You want to rework this into an implementation that uses recursion. Recusive functions can be memory intensive and are not always the best solution, but there are some problems in which using recursion can provide a simpler and more elegant solution.

## Implementation Details
In the recursive version of `convert`, start with the last `char` and convert it into an integer value. Then shorten the `string`, removing the last `char`, and then recursively call `convert` using the shortened string as input, where the next `char` will be processed.
```js
#include <cs50.h>
#include <ctype.h>
#include <math.h>
#include <stdio.h>
#include <string.h>

int convert(string input);
int number = 0;
int main(void)
{
    string input = get_string("Enter a positive integer: ");

    for (int i = 0, n = strlen(input); i < n; i++)
    {
        if (!isdigit(input[i]))
        {
            printf("Invalid Input!\n");
            return 1;
        }
    }

    // Convert string to int
    printf("%i\n", convert(input));
}

int convert(string input)
{
    // TODO
    int count = strlen(input);
    int temporaryInt = 0;

    // recursive base case
    if (count == 0)
    {
        return number;
    }
    for (int i = count - 1; i >= 0; i--)
    {
        temporaryInt = input[i] - '0';
        // ASCII '0' = 48       '9' = 57 -> '9' - '0' = 9
        input[i] = '\0';
        // 123 
        convert(input);

        number = number * 10 + temporaryInt;
        return number;
    }
    return number;
}
```
Output:
```txt
Enter a positive integer: 98765
98765
```
# Average Temperatures
We seem to be breaking records every year for the hottest weather ever recorded. Climate scientists keep track of what are called “new normals” over multiple years so that we can better predict and prepare for conditions in the near future. The official normals are calculated for a uniform 30 year period, and consist of annual/seasonal, monthly, daily, and hourly averages and statistics of temperature, precipitation, and other climatological variables from almost 15,000 U.S. weather stations.

July is the hottest month of the year for most large US cities. Daytime temperatures above 80 degrees Fahrenheit regularly occur nearly everywhere. The exceptions are some cities along the [Pacific coast](https://www.ncei.noaa.gov/products/land-based-station/us-climate-normals).

In this problem, you will sort the average high temperature values for 10 cities, in decending order.

## Implementation Details
The main function initializes the `temps` array, calls the `sort_cities` function and prints out the array in sorted order. You will use an $O(n^2)$ sorting algorithm of your choice (possibly bubble sort, selection sort, or insertion sort) to sort the array by temperature, in descending order.
```c
// Practice working with structs
// Practice applying sorting algorithms

#include <cs50.h>
#include <stdio.h>

#define NUM_CITIES 10

typedef struct
{
    string city;
    int temp;
}
avg_temp;

avg_temp temps[NUM_CITIES];

void sort_cities(void);

int main(void)
{
    temps[0].city = "Austin";
    temps[0].temp = 97;

    temps[1].city = "Boston";
    temps[1].temp = 82;

    temps[2].city = "Chicago";
    temps[2].temp = 85;

    temps[3].city = "Denver";
    temps[3].temp = 90;

    temps[4].city = "Las Vegas";
    temps[4].temp = 105;

    temps[5].city = "Los Angeles";
    temps[5].temp = 82;

    temps[6].city = "Miami";
    temps[6].temp = 97;

    temps[7].city = "New York";
    temps[7].temp = 85;

    temps[8].city = "Phoenix";
    temps[8].temp = 107;

    temps[9].city = "San Francisco";
    temps[9].temp = 66;

    sort_cities();

    printf("\nAverage July Temperatures by City\n\n");

    for (int i = 0; i < NUM_CITIES; i++)
    {
        printf("%s: %i\n", temps[i].city, temps[i].temp);
    }
}

// TODO: Sort cities by temperature in descending order
void sort_cities(void)
{
    // Add your code here
    int n = NUM_CITIES;

    avg_temp temp;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < n-i-1; j++){
            if(temps[j].temp < temps[j+1].temp){
                temp = temps[j];
                temps[j] = temps[j + 1];
                temps[j + 1] = temp;
            }
        }
    }
}
```
Output:
```txt
Average July Temperatures by City

Phoenix: 107
Las Vegas: 105
Austin: 97
Miami: 97
Denver: 90
Chicago: 85
New York: 85
Boston: 82
Los Angeles: 82
San Francisco: 66
```
# Max
There are many situations where you’ll find it helpful to have a function that finds the maximum (and minimum) value in an array. Since there is no built-in `max` function in C, you’ll create one in this practice problem. You can then use it in upcoming problem sets where it may be helpful!

## Implementation Details
The main function initializes the array, asks the user to enter values, and then passes the array and the number of items to the `max` function. Complete the `max` function by iterating through every element in the array, and return the maximum value.
```c
// Practice writing a function to find a max value

#include <cs50.h>
#include <stdio.h>

int max(int array[], int n);

int main(void)
{
    int n;
    do
    {
        n = get_int("Number of elements: ");
    } 
    while (n < 1);

    int arr[n];

    for (int i = 0; i < n; i++)
    {
        arr[i] = get_int("Element %i: ", i);
    }

    printf("The max value is %i.\n", max(arr, n));
}

// TODO: return the max value
int max(int array[], int n)
{
    int max_value = array[0];

    for (int i = 0; i < n; i++){
        if(max_value < array[i]){
            max_value = array[i];
        }
    }
    return max_value;
}
```
Output:
```txt
Number of elements: 4
Element 0: -100
Element 1: -200
Element 2: -3
Element 3: -5000
The max value is -3.
```
# Snackbar
Imagine you’re at the beach and want to order a number of items from the snack bar. You have a limited amount of cash on you, and you want to get a total cost for your items before ordering. In `snackbar.c` you will complete two functions. First is `add_items` which will add at least the first 4 items on the Beach Burger Shack menu. Then you will complete `get_cost` which will implement a linear search algorithm to search for each item you choose, and return the corresponding price.

## Implementation Details
The main function is already complete. After calling `add_items` to initialize the `menu` array, it will print out the menu items and their prices, prompting you to keep selecting items until you press enter without typing anything in. You are to complete two functions, `add_items`, which adds at least the first four menu items, and `get_cost` to return the cost of each item. When you are creating a linear search algorithm in `get_cost`, do make sure that it is case insensitive.
```c
// Practice using structs
// Practice writing a linear search function

/**
 * Beach Burger Shack has the following 10 items on their menu
 * Burger: $9.5
 * Vegan Burger: $11
 * Hot Dog: $5
 * Cheese Dog: $7
 * Fries: $5
 * Cheese Fries: $6
 * Cold Pressed Juice: $7
 * Cold Brew: $3
 * Water: $2
 * Soda: $2
*/

#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>
#include <strings.h>

// Number of menu items
// Adjust this value (10) to number of items input below
#define NUM_ITEMS 10

// Menu itmes have item name and price
typedef struct
{
    string item;
    float price;
}
menu_item;

// Array of menu items
menu_item menu[NUM_ITEMS];

// Add items to menu
void add_items(void);

// Calculate total cost
float get_cost(string item);

int main(void)
{
    add_items();

    printf("\nWelcome to Beach Burger Shack!\n");
    printf("Choose from the following menu to order. Press enter when done.\n\n");

    for (int i = 0; i < NUM_ITEMS; i++)
    {
        printf("%s: $%.2f\n", menu[i].item, menu[i]. price);
    }
    printf("\n");

    float total = 0;
    while (true)
    {
        string item = get_string("Enter a food item: ");
        if (strlen(item) == 0)
        {
            printf("\n");
            break;
        }

        total += get_cost(item);
    }

    printf("Your total cost is: $%.2f\n", total);
}

// Add at least the first for items to the menu array
void add_items(void)
{
    menu[0].item = "Burger";
    menu[0].price = 9.5;

    menu[1].item = "Vegan Burger";
    menu[1].price = 11.0;

    menu[2].item = "Hot Dog";
    menu[2].price = 5.0;

    menu[3].item = "Cheese Dog";
    menu[3].price = 7.0;

    menu[4].item = "Fries";
    menu[4].price = 5.0;

    menu[5].item = "Cheese Fries";
    menu[5].price = 6.0;

    menu[6].item = "Cold Pressed Juice";
    menu[6].price = 7.0;

    menu[7].item = "Cold Brew";
    menu[7].price = 3.0;

    menu[8].item = "Water";
    menu[8].price = 2.0;

    menu[9].item = "Soda";
    menu[9].price = 2.0;
}

// Search through the menu array to find an item's cost
float get_cost(string item)
{
    for(int i = 0; i < NUM_ITEMS; i++){
        if(strcasecmp(item, menu[i].item) == 0){
            return menu[i].price;
        }
    }
    return 0.0;
}
```
Output:
```txt
Welcome to Beach Burger Shack!
Choose from the following menu to order. Press enter when done.

Burger: $9.50
Vegan Burger: $11.00
Hot Dog: $5.00
Cheese Dog: $7.00
Fries: $5.00
Cheese Fries: $6.00
Cold Pressed Juice: $7.00
Cold Brew: $3.00
Water: $2.00
Soda: $2.00

Enter a food item: burger
Enter a food item: fries
Enter a food item: soda
Enter a food item: 

Your total cost is: $16.50
```