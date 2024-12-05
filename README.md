# Java-Project-
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class BeatItGUI extends JFrame implements ActionListener {
    private static final int MAX_NUMBER = 100;
    private static final int MAX_ATTEMPTS = 10;
    
    private JTextField guessField;
    private JTextArea resultArea;
    private JButton guessButton;
    private JButton newGameButton;
    
    private int numberToGuess;
    private int attemptsLeft;
    private int score;
    private long startTime;
    
    public BeatItGUI() {
        setTitle("Beat It Game");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());
        
        guessField = new JTextField(10);
        resultArea = new JTextArea();
        resultArea.setEditable(false);
        guessButton = new JButton("Guess");
        newGameButton = new JButton("New Game");
        
        JPanel topPanel = new JPanel();
        topPanel.add(new JLabel("Enter your guess:"));
        topPanel.add(guessField);
        topPanel.add(guessButton);
        add(topPanel, BorderLayout.NORTH);
        
        add(new JScrollPane(resultArea), BorderLayout.CENTER);
        add(newGameButton, BorderLayout.SOUTH);
        
        guessButton.addActionListener(this);
        newGameButton.addActionListener(e -> startNewGame());
        
        startNewGame();
    }
    private void startNewGame() {
        Random random = new Random();
        numberToGuess = random.nextInt(MAX_NUMBER) + 1;
        attemptsLeft = MAX_ATTEMPTS;
        score = 0;
        startTime = System.currentTimeMillis();
        resultArea.setText("Welcome to the Beat It Game!\nGuess a number between 1 and " + MAX_NUMBER + ".\nYou have " + attemptsLeft + " attempts.");
    }
    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == guessButton) {
            try {
                int playerGuess = Integer.parseInt(guessField.getText());
                if (playerGuess < 1 || playerGuess > MAX_NUMBER) {
                    resultArea.append("\nPlease enter a number between 1 and " + MAX_NUMBER + ".");
                    return;
                }
                handleGuess(playerGuess);
            } catch (NumberFormatException ex) {
                resultArea.append("\nPlease enter a valid number.");
            }
        }
    }
    
    private void handleGuess(int playerGuess) {
        if (playerGuess == numberToGuess) {
            long endTime = System.currentTimeMillis();
            long timeTaken = (endTime - startTime) / 1000;
            score = 100 - (MAX_ATTEMPTS - attemptsLeft) * 10;
            resultArea.append("\nCongratulations! You guessed the number correctly.");
            resultArea.append("\nYour score: " + score);
            resultArea.append("\nTime taken: " + timeTaken + " seconds");
        } else {
            attemptsLeft--;
            if (attemptsLeft > 0) {
                if (playerGuess < numberToGuess) {
                    resultArea.append("\nToo low! You have " + attemptsLeft + " attempts left.");
                } else {
                    resultArea.append("\nToo high! You have " + attemptsLeft + " attempts left.");
                }
            } else {
                resultArea.append("\nSorry! You've used all your attempts. The number was " + numberToGuess + ".");
                long endTime = System.currentTimeMillis();
                long timeTaken = (endTime - startTime) / 1000; 
                resultArea.append("\nTime taken: " + timeTaken + " seconds");
            }
        }
        guessField.setText("");
    }
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            BeatItGUI game = new BeatItGUI();
            game.setVisible(true);
        });
    }
}
