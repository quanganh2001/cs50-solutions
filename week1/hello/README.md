*Note: please add cs50.c and cs50.h files to your practice problems or your labworks.*

How to complier code: `gcc -o hello hello.c cs50.c`

Full source code
```c
#include <stdio.h>
#include <cs50.h>

int main(void)
{
    string name = get_string("What's your name? ");
    printf("hello, %s\n", name);
}
```