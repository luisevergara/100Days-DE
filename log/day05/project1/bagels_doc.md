# Project Breakdown

1. **Define the Problem**:
   The problem is to create a logic game that lets a user guess a secret 3 digit number within the set number of guesses. The game should display all the rules so that the user understands, let the user input the a guess multiple times until the correct guess has been made or the max number of guesses has been surpased with no correct answer. 
   
   The 3 digit number has no repeated digits, and the rules of the game state that if one digit is correct but in a wrong position, the game will display a hint with the word "Pico", if one digit is correct and in the right position, the game will display a hint with the word "Fermi", if no digit is correct, then the game will display the word "Bagels".

   The number of guesses will not increment if the user types a guess with aditional digits or with float values. The numbers will be able to include a leading 0.

   After the game ends, the user may be able to play again or quit. 

2. **Break Down the Problem**:
    - Display game instructions for the user.
    - Create random secret 3 digit number for the user to guess.
    - Get user input for their guess.
    - Perform comparisons between guess and secret number
        - If there is a match, end the game.
        - If no match, provide hints depending on the rules.
            - If no match, then guess counter increments.
                - If the guess counter is > max set, end game.
    - Display the result of the game
    - Prompt the user to continue playing or end the game.

3. **Research and Gather Information**:
    - {} Empty Curly Brackets for Formatting.

4. **Plan and Design**:
CONSTANT NUM_DIGITS = 1 // Set the number of digits for the secret number
CONSTANT MAX_GUESSES = 3 // Set the maximum number of guesses allowed

FUNCTION main():
    DISPLAY "Bagels, a deductive logic game."
    DISPLAY "I am thinking of a NUM_DIGITS-digit number with no repeated digits."
    DISPLAY "Try to guess what it is. Here are some clues:"
    DISPLAY "When I say:   That means:"
    DISPLAY "  Pico        One digit is correct but in the wrong position"
    DISPLAY "  Fermi       One digit is correct and in the right position"
    DISPLAY "  Bagels      No digit is correct."
    
    REPEAT UNTIL False:
        secretNum = getSecretNum()
        DISPLAY "I have thought up a number."
        DISPLAY "You have MAX_GUESSES guesses to get it."
        
        numGuesses = 1
        REPEAT UNTIL numGuesses > MAX_GUESSES:
            guess = ''
            REPEAT UNTIL LENGTH(guess) = NUM_DIGITS AND IS_DECIMAL(guess):
                DISPLAY "Guess #numGuesses:"
                INPUT guess
                
            clues = getClues(guess, secretNum)
            DISPLAY clues
            numGuesses = numGuesses + 1
            
            IF guess = secretNum THEN
                EXIT LOOP
            IF numGuesses > MAX_GUESSES THEN
                DISPLAY "You ran out of guesses."
                DISPLAY "The answer was secretNum."
        
        DISPLAY "Do you want to play again? (yes or no)"
        IF NOT INPUT().lower().startswith('y') THEN
            EXIT LOOP
        DISPLAY "Thanks for playing!"

FUNCTION getSecretNum():
    numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    RANDOM_SHUFFLE(numbers)
    
    secretNum = ''
    FOR i = 0 TO NUM_DIGITS:
        secretNum = secretNum + numbers[i]
    RETURN secretNum

FUNCTION getClues(guess, secretNum):
    IF guess = secretNum THEN
        RETURN 'You got it'
    
    clues = []
    
    FOR i = 0 TO LENGTH(guess):
        IF guess[i] = secretNum[i] THEN
            APPEND 'Fermi' TO clues
        ELSE IF guess[i] IN secretNum THEN
            APPEND 'Pico' TO clues
    IF LENGTH(clues) = 0 THEN
        RETURN 'Bagels'
    ELSE
        SORT(clues)
        RETURN JOIN(clues, ' ')

IF __name__ = '__main__':
    CALL main()

5. **Set up the Environment**:
   - Random Module Needed

6. **Define Functions and Classes**

7. **Implement Core Logic**

8. **Test and Debug**:
   - Test the program with various inputs, including valid and invalid inputs
   - Use print statements or a debugger to identify and fix any issues or bugs
   - Test edge cases, such as negative dimensions or invalid shape types

9. **Refactor and Optimize**:
   - Review the code and look for opportunities to improve readability, maintainability, or performance
   - Consider refactoring the code to use a dictionary or other data structure to map shape types to area calculation functions, instead of using multiple `if`/`elif` statements

10. **Document Your Code**:
    - Add docstrings to explain the purpose and functionality of each function
    - Add comments to clarify the program flow and any important details or assumptions

11. **Test Thoroughly**:
    - Implement unit tests for the area calculation functions to ensure they work correctly for various inputs
    - Test the main program flow with different combinations of valid and invalid inputs

12. **Handle Errors and Edge Cases**:
    - Add error handling for invalid user inputs, such as non-numeric dimensions or unsupported shape types
    - Use try/except blocks to catch and handle exceptions gracefully

13. **Add User Interface (Optional)**

14. **Deploy and Distribute**

15. **Maintain and Update**