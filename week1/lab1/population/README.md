*Note: please add cs50.c and cs50.h files to your practice problems or your labworks.*

Full source code
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int start, end, years = 0;
    // TODO: Prompt for start size
    do
    {
        printf("Enter starting population size (minimum 9): ");
        scanf("%d", &start);
    }
    while (start < 9);

    // TODO: Prompt for end size
    do
    {
        printf("Enter ending population size (minimum %d): ", start);
        scanf("%d", &end);
    }
    while (end < start);

    // TODO: Calculate number of years until we reach threshold
    while (start < end)
    {
        start = start + start / 3 - start / 4;
        years++;
    }

    // TODO: Print number of years
    printf("Years: %d\n", years);
    return 0;
}
```

How to complier code: `gcc -o population population.c cs50.c`

Output:
```txt
Enter starting population size (minimum 9): 100
Enter ending population size (minimum 100): 200
Years: 9
```