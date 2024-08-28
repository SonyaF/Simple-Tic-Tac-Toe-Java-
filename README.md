# Simple-Tic-Tac-Toe-Java-



import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        char[][] grid = new char[3][3];

        // Initialize the grid with empty cells
        initializeGrid(grid);

        // Display the initial empty grid
        printGrid(grid);

        char currentPlayer = 'X';  // Start with player 'X'
        boolean gameEnded = false;

        while (!gameEnded) {
            System.out.print("Enter the coordinates for " + currentPlayer + ": ");
            String coordinates = scanner.nextLine();
            String[] parts = coordinates.split(" ");

            // Check if the input contains two numeric values
            if (parts.length != 2 || !parts[0].matches("\\d") || !parts[1].matches("\\d")) {
                System.out.println("You should enter numbers!");
                continue;
            }

            int row = Integer.parseInt(parts[0]) - 1;
            int col = Integer.parseInt(parts[1]) - 1;

            // Check if the coordinates are within the valid range
            if (row < 0 || row > 2 || col < 0 || col > 2) {
                System.out.println("Coordinates should be from 1 to 3!");
                continue;
            }

            // Check if the selected cell is already occupied
            if (grid[row][col] != '_') {
                System.out.println("This cell is occupied! Choose another one!");
                continue;
            }

            // Place the current player's mark on the grid
            grid[row][col] = currentPlayer;

            // Print the updated grid
            printGrid(grid);

            // Analyze the game state
            String result = analyzeGame(grid);

            // Check the result of the game
            if (result.equals("X wins") || result.equals("O wins") || result.equals("Draw")) {
                System.out.println(result);
                gameEnded = true;
            } else {
                // Switch to the other player
                currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
            }
        }

        scanner.close();
    }

    private static void initializeGrid(char[][] grid) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                grid[i][j] = '_';  // Use '_' for empty cells
            }
        }
    }

    private static void printGrid(char[][] grid) {
        System.out.println("---------");
        for (int i = 0; i < 3; i++) {
            System.out.print("| ");
            for (int j = 0; j < 3; j++) {
                System.out.print(grid[i][j] + " ");
            }
            System.out.println("|");
        }
        System.out.println("---------");
    }

    private static String analyzeGame(char[][] grid) {
        int xCount = 0;
        int oCount = 0;
        boolean xWins = false;
        boolean oWins = false;

        // Counting X's and O's
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (grid[i][j] == 'X') xCount++;
                if (grid[i][j] == 'O') oCount++;
            }
        }

        // Check rows, columns, and diagonals for wins
        for (int i = 0; i < 3; i++) {
            if (grid[i][0] == grid[i][1] && grid[i][1] == grid[i][2]) {
                if (grid[i][0] == 'X') xWins = true;
                if (grid[i][0] == 'O') oWins = true;
            }
            if (grid[0][i] == grid[1][i] && grid[1][i] == grid[2][i]) {
                if (grid[0][i] == 'X') xWins = true;
                if (grid[0][i] == 'O') oWins = true;
            }
        }
        if (grid[0][0] == grid[1][1] && grid[1][1] == grid[2][2]) {
            if (grid[0][0] == 'X') xWins = true;
            if (grid[0][0] == 'O') oWins = true;
        }
        if (grid[0][2] == grid[1][1] && grid[1][1] == grid[2][0]) {
            if (grid[0][2] == 'X') xWins = true;
            if (grid[0][2] == 'O') oWins = true;
        }

        // Analyzing the state
        if (xWins && oWins || Math.abs(xCount - oCount) > 1) {
            return "Impossible";
        } else if (xWins) {
            return "X wins";
        } else if (oWins) {
            return "O wins";
        } else if (xCount + oCount == 9) {
            return "Draw";
        } else {
            return "Game not finished";
        }
    }
}
