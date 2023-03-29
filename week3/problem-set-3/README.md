# Plurality
Elections come in all shapes and sizes. In the UK, the [Prime Minister](https://www.parliament.uk/education/about-your-parliament/general-elections/) is officially appointed by the monarch, who generally chooses the leader of the political party that wins the most seats in the House of Commons. The United States uses a multi-step [Electoral College](https://www.archives.gov/federal-register/electoral-college/about.html) process where citizens vote on how each state should allocate Electors who then elect the President.

Perhaps the simplest way to hold an election, though, is via a method commonly known as the “plurality vote” (also known as “first-past-the-post” or “winner take all”). In the plurality vote, every voter gets to vote for one candidate. At the end of the election, whichever candidate has the greatest number of votes is declared the winner of the election.

## Specification
<hr>
Complete the implementation of `plurality.c` in such a way that the program simulates a plurality vote election.

- Complete the `vote` function.
  - `vote` takes a single argument, a string called `name`, representing the name of the candidate who was voted for.
  - If name matches one of the names of the candidates in the election, then update that candidate’s vote total to account for the new vote. The vote function in this case should return true to indicate a successful ballot.
  - If name does not match the name of any of the candidates in the election, no vote totals should change, and the vote function should return false to indicate an invalid ballot.
  - You may assume that no two candidates will have the same name.
- Complete the `print_winner` function.
  - The function should print out the name of the candidate who received the most votes in the election, and then print a newline.
  - It is possible that the election could end in a tie if multiple candidates each have the maximum number of votes. In that case, you should output the names of each of the winning candidates, each on a separate line.

You should not modify anything else in   other than the implementations of the `vote` and `print_winner` functions (and the inclusion of additional header files, if you’d like).

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Max number of candidates
#define MAX 9

// Candidates have name and vote count
typedef struct
{
    string name;
    int votes;
}
candidate;

// Array of candidates
candidate candidates[MAX];

// Number of candidates
int candidate_count;

// Function prototypes
bool vote(string name);
void print_winner(void);

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: plurality [candidate ...]\n");
        return 1;
    }

    // Populate array of candidates
    candidate_count = argc - 1;
    if (candidate_count > MAX)
    {
        printf("Maximum number of candidates is %i\n", MAX);
        return 2;
    }
    for (int i = 0; i < candidate_count; i++)
    {
        candidates[i].name = argv[i + 1];
        candidates[i].votes = 0;
    }

    int voter_count = get_int("Number of voters: ");

    // Loop over all voters
    for (int i = 0; i < voter_count; i++)
    {
        string name = get_string("Vote: ");

        // Check for invalid vote
        if (!vote(name))
        {
            printf("Invalid vote.\n");
        }
    }

    // Display winner of election
    print_winner();
}

// Update vote totals given a new vote
bool vote(string name)
{
    for (int i = 0; i < candidate_count; i++)
    {
        if (strcmp(name, candidates[i].name) == 0)
        {
            candidates[i].votes++;
            return true;
        }
    }
    return false;
}

// Print the winner (or winners) of the election
void print_winner(void)
{
    int maxvotes = 0;

    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes > maxvotes)
        {
            maxvotes = candidates[i].votes;
        }
    }

    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes == maxvotes)
        {
            printf("%s\n", candidates[i].name);
        }
    }
    return;
}
```
Output:
```txt
Number of voters: 5
Vote: Alice
Vote: Charlie
Vote: Bob
Vote: Bob
Vote: Alice
Alice
Bob
```
# Runoff
## Vote function
- This function keeps track of all the preferences
- If at any point, the ballot is deemed to be invalid, the program exits
- The function takes arguments `voter`, `rank`, and `name`
- For each valid vote, we need to update the global preferences array to indicate that the voter has that candidate as their rank preference (where 0 is the first preference, 1 is the second preference, etc).
- If the preference is successfully recorded, the function should return true; the function should return false otherwise (if, for instance, name is not the name of one of the candidates).
- `preferences[i][j]` stores the index of the candidate who is the `jth` ranked preference for the `ith` voter.
````c
// Record preference if vote is valid
bool vote(int voter, int rank, string name)
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        if(strcmp(candidates[i].name, name) == 0)
        {
            preferences[voter][rank] = i;
            return true;
        }
    }
    return false;
}
````
## tabulate function
- We will loop through each vote that was made to add it to the candidates total count.
- As your program goes through each vote, what must it check?
- It must check that the candidate has not been eliminated
- If not eliminated, then it will add 1 to the candidate's total vote count
  - The system must then stop and move on to the next voter's inputs
- If eliminated, the system will check the next ranked preference of the voter
```c
// Tabulate votes for non-eliminated candidates
void tabulate(void)
{
    // TODO
    for (int i = 0; i < voter_count; i++)
    {
        for (int j = 0; j < candidate_count; j++)
        {
            if (candidates[preferences[i][j]].eliminated == false)
            {
                candidates[preferences[i][j]].votes++;
                break;
            }
        }
    }
    return;
}
```
## print_winner function
- With `vote`, we tracked the preferences of each voter
- With `tabulate`, we counted each of candidate's total vote count
- Now that we know each candidate's score, we just need to tell the system qualifying criteria to win the electron, and print the candidate's name
- We just need to loop through each candidate's total score and if it is more than half the total count, we will print their name
  - If there are 4 voters, a candidate with more than 2 votes will win
  - If there are 7 voters, a candidate with more than 3 votes will win
```c
// Print the winner of the election, if there is one
bool print_winner(void)
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes > voter_count/2)
        {
            printf("%s\n", candidates[i].name);
            return true;
        }
    }
    return false;
}
```
## find_min function
- We need a staring point as the `minimum` count, and loop through each candidate.
  - We will declare `int mon = voter_count`
- If the candidate is not eliminated and candidate's total vote count is lower than that of the `minimum count`, that becomes the new value of `minimum count`

<hr>

For example, in an electron where there are 9 voters, `int min = voter_count`

We begin looping through each candidate

- Alice = 4 votes
  - Since `candidates[0].votes < min`, we will update `min` to be 4
  - Up to this point, the `min` is 4
- Bob = 2 votes
  - Since `candidates[1].votes < min`, we will update `min` to be 4
  - Up to this point, the `min` is 2
- Charlie = 3 votes
  - Since `candidates[2].votes > min`, `min` remains unchanged at 2
  - Up to this point, the `min` remains as 2
```c
// Return the minimum number of votes any remaining candidate has
int find_min(void)
{
    // TODO
    int min = voter_count;
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes < min && candidates[i].eliminated == false)
        {
            min = candidates[i].votes;
        }
    }
    return min;
}
```
## is_tie function
- The function should return true if every candidate remaining in the election has the same number of votes, and should return false otherwise.

How do we track this?

[![image.png](https://i.postimg.cc/RZQy7Wzn/image.png)](https://postimg.cc/75hKDYhx)

- For the above, you can see that we have 2 candidates left running in the electron - Bob and Charlie
- They have the same number of votes = 2 which is also equals to the `minimum` count
  - Remember: `minimum count` stores the value of the lowest total vote count of participants who are not eliminated
- Our program needs to count the number of candidates remaining in the election and the number of candidates whose total vote count = `minimum count`
  - If both numbers are the same, there is a tie

<hr>

- Let the number of candidates still running for the electron be `int eliminate`
- Let the number of candidates whose total vote count equals to `minimum` count be `int counter`.
```c
// Return true if the election is tied between all candidates, false otherwise
bool is_tie(int min)
{
    // TODO
    int eliminate = 0;
    int counter = 0;
    for (int i = 0; i < candidate_count; i++)
    {
        if (!candidates[i].eliminated) // the candidate's elimination status is false
        {
            eliminate++; // We add 1 to eliminate
        }
        if (candidates[i].votes == min)
        {
            counter++;
        }
    }
    if (eliminate == counter)
    {
        return true;
    }
    return false;
}
```
## eliminate function
- The function takes an argument min, which will be the minimum number of votes that anyone in the election currently has.
- The function should eliminate the candidate (or candidates) who have min number of votes.
```c
// Eliminate the candidate (or candidates) in last place
void eliminate(int min)
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes == min)
        {
            candidates[i].eliminated = true;
        }
    }
    return;
}
```
Output:
```txt
./runoff Alice Bob Charlie
Number of voters: 5
Rank 1: Alice
Rank 2: Charlie
Rank 3: Bob

Rank 1: Alice
Rank 2: Charlie
Rank 3: Bob

Rank 1: Bob
Rank 2: Charlie
Rank 3: Alice

Rank 1: Bob
Rank 2: Charlie
Rank 3: Alice

Rank 1: Charlie
Rank 2: Alice
Rank 3: Bob

Alice
```
# Tideman
For this program, you’ll implement a program that runs a Tideman election, per the below.
## Specification
<hr>
Complete the implementation of `tideman.c` in such a way that it simulates a `Tideman` election.

- Complete the vote function.
  - The function takes arguments `rank`, `name`, and `ranks`. If name is a match for the name of a valid candidate, then you should update the `ranks` array to indicate that the voter has the candidate as their `rank` preference (where 0 is the first preference, 1 is the second preference, etc.)
  - Recall that `ranks[i]` here represents the user’s `i`th preference.
  - The function should return true if the rank was successfully recorded, and false otherwise (if, for instance, name is not the name of one of the candidates).
  - You may assume that no two candidates will have the same name.
- Complete the `record_preferences` function.
  - The function is called once for each voter, and takes as argument the `ranks` array, (recall that `ranks[i]` is the voter’s `i`th preference, where `ranks[0]` is the first preference).
  - The function should update the global preferences array to add the current voter’s preferences. Recall that `preferences[i][j]` should represent the number of voters who prefer candidate `i` over candidate `j`.
  - You may assume that every voter will rank each of the candidates.
- Complete the `add_pairs` function.
  - The function should add all pairs of candidates where one candidate is preferred to the `pairs` array. A pair of candidates who are tied (one is not preferred over the other) should not be added to the array.
  - The function should update the global variable `pair_count` to be the number of pairs of candidates. (The pairs should thus all be stored between `pairs[0]` and `pairs[pair_count - 1]`, inclusive).
- Complete the `sort_pairs` function.
  - The function should sort the `pairs` array in decreasing order of strength of victory, where strength of victory is defined to be the number of voters who prefer the preferred candidate. If multiple pairs have the same strength of victory, you may assume that the order does not matter.
- Complete the lock_pairs function.
  - The function should create the `locked` graph, adding all edges in decreasing order of victory strength so long as the edge would not create a cycle.
- Complete the `print_winner` function.
  - The function should print out the name of the candidate who is the source of the graph. You may assume there will not be more than one source.


You should not modify anything else in `tideman.c` other than the implementations of the `vote`, `record_preferences`, `add_pairs`, `sort_pairs`, `lock_pairs`, and `print_winner` functions (and the inclusion of additional header files, if you’d like). You are permitted to add additional functions to `tideman.c`, so long as you do not change the declarations of any of the existing functions.

## Functions
### Update ranks given a new vote (Vote function)
```c
// Update ranks given a new vote
bool vote(int rank, string name, int ranks[])
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        if (strcmp(name, candidates[i]) == 0)
        {
            ranks[rank] = i;
            return true;
        }
    }
    return false;
}
```
### Update preferences given one voter's ranks (record_preferences function)
```c
// Update preferences given one voter's ranks
void record_preferences(int ranks[])
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        for (int j = i + 1; j < candidate_count; j++)
        {
            preferences[ranks[i]][ranks[j]] += 1;
        }
    }
    return;
}
```
### Record pairs of candidates where one is preferred over the other (add_pairs function)
```c
// Record pairs of candidates where one is preferred over the other
void add_pairs(void)
{
    // TODO
    for (int i = 0; i < candidate_count; i++)
    {
        for (int j = 0; j < candidate_count; j++)
        {
            if (preferences[i][j] > preferences[j][i])
            {
                pairs[pair_count].winner = i;
                pairs[pair_count].loser = j;
                pair_count++;
            }
            else if (preferences[i][j] > preferences[j][i])
            {
                pairs[pair_count].winner = j;
                pairs[pair_count].loser = i;
                pair_count++;
            }
        }
    }
    return;
}
```
### Sort pairs in decreasing order by strength of victory (sort_pairs function)
```c
// Sort pairs in decreasing order by strength of victory
void sort_pairs(void)
{
    // TODO
    pair pairs_final[MAX * (MAX - 1) / 2];

    merge_sort(0, candidate_count, pairs, pairs_final);
}
```
### Lock pairs into the candidate graph in order, without creating cycles (lock_pairs function)
```c
// Lock pairs into the candidate graph in order, without creating cycles
void lock_pairs(void)
{
    // TODO
    for (int i = 0; i < pair_count; i++)
    {
        if (recursive_lock(pairs[i].winner, pairs[i].loser) == true)
        {
            locked[pairs[i].winner][pairs[i].loser] = true;
        }
    }
}
```
### Print the winner of the election (print_winner function)
```c
// Print the winner of the election
void print_winner(void)
{
    // TODO
    bool won_vote;
    int index;

    for (int i = 0; i < candidate_count; i++)
    {
        won_vote = true;

        for (int j = 0; j < candidate_count; j++)
        {
            if (locked[j][i] == true)
            {
                won_vote = false;
            }
        }

        if (won_vote == true)
        {
            index = i;
        }
    }

    printf("%s\n", candidates[index]);

    return;
}

void merge_sort(int i, int j, pair a[], pair aux[])
{
    //note function from https://hackr.io/blog/merge-sort-in-c
    if (j <= i)
    {
        return;
    }

    int mid = (i + j) / 2;

    merge_sort(i, mid, a, aux);
    merge_sort(mid + 1, j, a, aux);

    int pointer_left = i;
    int pointer_right = mid + 1;
    int k;

    for (k = i; k <= j; k++)
    {
        if (pointer_left == mid + 1)
        {
            aux[k] = a[pointer_right];
            pointer_right++;
        }
        else if (pointer_right == j + 1)
        {
            aux[k] = a[pointer_left];
            pointer_left++;
        }
        else if ((preferences[a[pointer_left].winner][a[pointer_left].loser]) >
                 (preferences[a[pointer_right].winner][a[pointer_right].loser]))
        {
            aux[k] = a[pointer_left];
            pointer_left++;
        }
        else
        {
            aux[k] = a[pointer_right];
            pointer_right++;
        }
    }

    for (k = i; k <= j; k++)
    {
        a[k] = aux[k];
    }
}

bool recursive_lock(int a, int b)
{
    if (a == b)
    {
        return false;
    }

    for (int i = 0; i < candidate_count; i++)
    {
        if (locked[i][a] == true)
        {
            if (recursive_lock(i, b) == false)
            {
                return false;
            }
        }
    }

    return true;
}
```
Output:
```txt
./tideman Alice Bob Charlie
Number of voters: 5
Rank 1: Alice
Rank 2: Charlie
Rank 3: Bob

Rank 1: Alice
Rank 2: Charlie
Rank 3: Bob

Rank 1: Bob
Rank 2: Charlie
Rank 3: Alice

Rank 1: Bob
Rank 2: Charlie
Rank 3: Alice

Rank 1: Charlie
Rank 2: Alice
Rank 3: Bob

Charlie
```
# Full Source Code
You can see source code at:

1. [Plurality](plurality/plurality.c)
2. [Runoff](runoff/runoff.c)
3. [Tideman](tideman/tideman.c)