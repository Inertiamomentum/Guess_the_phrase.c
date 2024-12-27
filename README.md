# Guess_the_phrase.c
#include <stdio.h>
#include <string.h>

#define MAX_TRIES 10  // Maximum number of incorrect guesses

void display_word(char word[], char guessed[]) {
    for (int i = 0; i < strlen(word); i++) {
        if (guessed[i] == 1) {
            printf("%c ", word[i]);
        } else {
            printf("_ ");
        }
    }
    printf("\n");
}

int count_words(char phrase[]) {
    int count = 1;  // Start with 1 since the first word is counted
    for (int i = 0; i < strlen(phrase); i++) {
        if (phrase[i] == ' ') {
            count++;
        }
    }
    return count;
}

int main() {
    char phrase[100];  // Array to store the phrase
    int length;
    
    printf("Enter a phrase to guess: ");
    fgets(phrase, sizeof(phrase), stdin);  // Allow the user to input a phrase
    phrase[strcspn(phrase, "\n")] = 0;  // Remove the newline character

    length = strlen(phrase);
    int word_count = count_words(phrase);  // Count the number of words in the phrase
    
    printf("The phrase contains %d words.\n", word_count);

    char guessed[length];           // To track guessed letters
    char guessed_letters[26];       // To store guessed letters
    int tries = 0;                   // Number of incorrect tries
    int correct_guesses = 0;         // Number of correct guesses

    // Initialize the guessed array to 0 (no letters guessed)
    for (int i = 0; i < length; i++) {
        guessed[i] = 0;  // 0 means not guessed, 1 means guessed
    }

    printf("Welcome to Guess the Phrase Game!\n");

    // Game loop
    while (tries < MAX_TRIES && correct_guesses < length) {
        char guess;
        int correct = 0;

        // Display the current progress
        display_word(phrase, guessed);

        printf("Guess a letter: ");
        scanf(" %c", &guess);  // Get user input (with space to skip newline)

        // Check if the letter has already been guessed
        for (int i = 0; i < 26; i++) {
            if (guessed_letters[i] == guess) {
                printf("You already guessed that letter!\n");
                correct = 1;
                break;
            }
        }

        // If it's a valid guess, check if it is correct
        if (!correct) {
            int match_found = 0;
            for (int i = 0; i < length; i++) {
                if (phrase[i] == guess && guessed[i] == 0) {
                    guessed[i] = 1;  // Mark the letter as guessed
                    correct_guesses++;
                    match_found = 1;
                }
            }

            if (!match_found) {
                tries++;
                printf("Incorrect guess! You have %d tries left.\n", MAX_TRIES - tries);
            } else {
                printf("Correct guess!\n");
            }

            // Add the guessed letter to the guessed_letters array
            for (int i = 0; i < 26; i++) {
                if (guessed_letters[i] == '\0') {
                    guessed_letters[i] = guess;
                    break;
                }
            }
        }

        printf("\n");
    }

    // End game condition
    if (correct_guesses == length) {
        printf("Congratulations! You guessed the phrase: %s\n", phrase);
    } else {
        printf("Sorry! You've run out of tries. The phrase was: %s\n", phrase);
    }

    return 0;
}

