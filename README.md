
📦 1. Package and Imports

package basics;
Keeps your class organized under a named package (basics).

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.border.EmptyBorder;
import java.util.ArrayList;
import java.util.List;
Imports UI classes from Swing and AWT:

JFrame, JPanel, JButton, etc. for UI
BorderLayout, GridLayout, etc. for layout
MouseAdapter, ActionListener for event handling
List and ArrayList for managing player moves

🧱 2. Class Declaration

public class XandO extends JFrame {
Defines your main game class, which inherits from JFrame (a window).

🎨 3. Theme Constants

private static final Color PRIMARY   = new Color(30, 144, 255);   // Dodger-blue
private static final Color SECONDARY = Color.WHITE;
private static final Font TITLE_FONT = new Font("SansSerif", Font.BOLD, 24);
private static final Font BTN_FONT   = new Font("SansSerif", Font.BOLD, 40);
Defines consistent fonts and colors used across the UI.

🎮 4. Game State

private final List<Integer> playerOne = new ArrayList<>();
private final List<Integer> playerTwo = new ArrayList<>();
private int turn = 0;
private String playerOneName = "Player 1";
private String playerTwoName = "Player 2";
Stores game data:

Each player’s moves (cell positions 1–9)
turn: decides whose move it is
Names of the players (from input)

🧩 5. UI Components

private final CardLayout cards = new CardLayout();
private final JPanel root = new JPanel(cards);
private final JButton[][] cells = new JButton[3][3];
private final JPanel gridPanel = new JPanel(new GridLayout(3, 3, 5, 5));
CardLayout allows screen switching (welcome ↔ game board).

root panel holds all the screens.
cells holds the 9 grid buttons.
gridPanel is the game board UI with 5px spacing between buttons.

🏗️ 6. Constructor: XandO()

public XandO() {
    super("Tic‑Tac‑Toe ✨");
    setDefaultCloseOperation(EXIT_ON_CLOSE);
    setSize(400, 500);
    setLocationRelativeTo(null);
    setResizable(false);

    buildWelcome();  // welcome screens
    buildGrid();     // 3x3 game grid

    add(root);                // Add root panel to frame
    cards.show(root, "welcome");  // Show welcome screen first
}
Sets up the frame size, close behavior, and builds both screens.

Starts on the welcome screen.

🙋‍♂️ 7. Welcome Screen: buildWelcome()
➤ Step 1: Intro Panel

JPanel introPanel = new JPanel();
introPanel.setLayout(new BoxLayout(introPanel, BoxLayout.Y_AXIS));
introPanel.setBorder(new EmptyBorder(80, 40, 40, 40));
introPanel.setBackground(PRIMARY);
Shows a welcome message with a “Next” button.

➤ Step 2: Name Panel

JPanel namePanel = new JPanel();
namePanel.setLayout(new BoxLayout(namePanel, BoxLayout.Y_AXIS));
namePanel.setBorder(new EmptyBorder(40, 40, 40, 40));
namePanel.setBackground(PRIMARY);
Asks for player names and has a “Start Game 🚀” button.

➤ Action Listeners

nextBtn.addActionListener(e -> cards.show(root, "name"));
startBtn.addActionListener(e -> {
    playerOneName = ...;
    playerTwoName = ...;
    resetGame();
    popupTransition();
    cards.show(root, "game");
});
Transitions from intro → name → game, capturing input and triggering animation.

🔲 8. Game Grid: buildGrid()

for (int r = 0; r < 3; r++) {
    for (int c = 0; c < 3; c++) {
        JButton cell = new JButton("");
        cell.addActionListener(e -> handleMove(cell, pos));
        ...
    }
}
Creates a 3x3 grid of buttons (numbered 1 to 9).

Each button click triggers handleMove.

java
Copy
Edit
JButton restart = new JButton("↩ Restart");
restart.addActionListener(e -> resetGame());
Restart button reinitializes the game.

🧠 9. Game Logic: handleMove()

private void handleMove(JButton btn, int pos) {
    if (turn == 0) { playerOne.add(pos); btn.setText("X"); }
    else { playerTwo.add(pos); btn.setText("O"); }

    animateClick(btn);
    btn.setEnabled(false);

    if (checkWin(playerOne))      showWinner(playerOneName);
    else if (checkWin(playerTwo)) showWinner(playerTwoName);
    else if (playerOne.size() + playerTwo.size() == 9) showWinner("No one");
    else turn ^= 1;  // flip turn
}
Registers a move, checks for winner or draw, updates turn.

✨ 10. Click Animation: animateClick()

Timer t = new Timer(40, null);
...
btn.setBackground(new Color(255, ...));
Temporarily changes the background color in 10 steps to show a click effect.

💬 11. Popup Transition: popupTransition()
JDialog splash = new JDialog(this, false);
...
splash.setLocation(...);
Timer t = new Timer(15, null);
Shows a temporary popup “Let’s Play!” that slides downward.

✅ 12. Win Checker: checkWin()
int[][] c = { {1,2,3}, {4,5,6}, ..., {3,5,7} };
Checks if any of the winning 3-number combos are in a player’s move list.

🏆 13. Show Winner Dialog: showWinner()
JDialog d = new JDialog(this, "Result", true);
...
d.setVisible(true);
Shows a dialog box with the winner name and a “Play again” button.

Slides downward with animation.

🔄 14. Reset Game: resetGame()

playerOne.clear();
playerTwo.clear();
turn = 0;
for (JButton[] row : cells) {
    for (JButton b : row) {
        b.setText("");
        b.setEnabled(true);
        ...
    }
}
Clears all buttons, resets player lists, and resets turn.

🎨 15. Helper Methods
addHoverEffect(JButton b): adds hover background color change

styliseButton(JButton b): applies consistent styles to all buttons

styliseTextField(JTextField t, String hint): styles text fields with borders and placeholder tooltip

🏁 16. Main Method

public static void main(String[] args) {
    SwingUtilities.invokeLater(() -> new XandO().setVisible(true));
}
Entry point to the program. Ensures GUI runs on the Event Dispatch Thread (EDT).

✅ Summary
Section	Purpose
buildWelcome	Handles welcome screen and name input
buildGrid	Builds 3x3 game UI and restart button
handleMove	Controls gameplay logic (moves, win, draw)
animateClick	Adds button-click visual effect
checkWin	Checks for winning combinations
showWinner	Shows popup with winner or draw
resetGame	Restarts the game
Helpers	Clean reusable UI design methods
