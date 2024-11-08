```c
#include "cs50.h"
#include <stdio.h>
#include <string.h>
#include <ctype.h> // For using tolower()

// Points associated with each letter of the alphabet in Scrabble
int POINTS[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};

int score(string word);

int main(int argc, string argv[]) {
    // Ensure the user provides exactly two arguments
    if (argc != 3) {
        printf("Usage: ./scrabble word1 word2\n");
        return 1;
    }

    // Calculate scores for both players
    int player1 = score(argv[1]);
    int player2 = score(argv[2]);

    printf("P1 Score = %i, P2 Score = %i\n", player1, player2);

    // Determine the winner
    if (player1 > player2) {
        printf("Player 1 wins!\n");
    } else if (player2 > player1) {
        printf("Player 2 wins!\n");
    } else {
        printf("Tie!\n");
    }

    return 0;
}

int score(string word) {
    int result = 0;

    // Iterate over each character in the word
    for (int i = 0, length = strlen(word); i < length; i++) {
        // Convert character to lowercase for uniformity
        char c = tolower(word[i]);

        // Check if character is a letter
        if (isalpha(c)) {
            // Calculate the index for POINTS array
            int index = c - 'a';
            result += POINTS[index];
        }
    }

    return result;
}

```
#### 1. **Character Encoding in C**

In C, characters are stored as integers using ASCII encoding, where each character is assigned a unique integer value. For example:

- `'a'` is represented by the ASCII value **97**.
- `'b'` is represented by the ASCII value **98**.
- `'z'` is represented by the ASCII value **122**.

These ASCII values are sequential, which is why subtracting `'a'` from any lowercase letter gives us a number that represents its position in the alphabet.

#### 2. **Calculating Index**

When you perform the operation `c - 'a'`, you are essentially computing the alphabetical position of the character `c`, where `'a'` maps to **0**, `'b'` maps to **1**, `'c'` maps to **2**, and so on up to `'z'`, which maps to **25**.

For example:

- If `c` is `'a'`, then `c - 'a'` is `97 - 97 = 0`.
- If `c` is `'b'`, then `c - 'a'` is `98 - 97 = 1`.
- If `c` is `'c'`, then `c - 'a'` is `99 - 97 = 2`.
- If `c` is `'z'`, then `c - 'a'` is `122 - 97 = 25`.

This result corresponds directly to an index in an array where each position represents the point value of a particular letter in Scrabble.