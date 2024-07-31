import java.util.Random;
import java.util.Scanner;

public class NumberGuessingGame {
    private static final int MIN_NUM = 1;
    private static final int MAX_NUM = 100;
    private static final int MAX_ATTEMPTS = 10;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();
        int score = 0;
        int rounds = 0;
        boolean playAgain = true;

        while (playAgain) {
            System.out.println("Round " + (rounds + 1));
            boolean roundWon = playRound(scanner, random);
            if (roundWon) {
                score++;
            }
            rounds++;
            
            System.out.print("Do you want to play again? (yes/no): ");
            String response = scanner.next().trim().toLowerCase();
            if (!response.equals("yes")) {
                playAgain = false;
            }
        }

        System.out.println("\nGame over! You played " + rounds + " rounds and won " + score + " times.");
        scanner.close();
    }

    private static boolean playRound(Scanner scanner, Random random) {
        int secretNumber = random.nextInt(MAX_NUM - MIN_NUM + 1) + MIN_NUM;
        int attempts = 0;

        System.out.println("Guess the number between " + MIN_NUM + " and " + MAX_NUM + ". You have " + MAX_ATTEMPTS + " attempts.");

        while (attempts < MAX_ATTEMPTS) {
            System.out.print("Enter your guess: ");
            int guess = getValidGuess(scanner);
            attempts++;

            if (guess == secretNumber) {
                System.out.println("Congratulations! You've guessed the number in " + attempts + " attempts.");
                return true;
            } else if (guess < secretNumber) {
                System.out.println("Too low!");
            } else {
                System.out.println("Too high!");
            }
        }

        System.out.println("Sorry, you've used all " + MAX_ATTEMPTS + " attempts. The number was " + secretNumber + ".");
        return false;
    }

    private static int getValidGuess(Scanner scanner) {
        while (!scanner.hasNextInt()) {
            System.out.print("Invalid input. Please enter a valid integer: ");
            scanner.next(); // Clear the invalid input
        }
        int guess = scanner.nextInt();
        if (guess < MIN_NUM || guess > MAX_NUM) {
            System.out.print("Please enter a number between " + MIN_NUM + " and " + MAX_NUM + ": ");
            return getValidGuess(scanner); // Recursively get valid input
        }
        return guess;
    }
}
