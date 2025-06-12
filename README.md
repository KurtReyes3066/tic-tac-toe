package tictac;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class Tictac {
    private JFrame f;
    private JPanel mp, gp;
    private JButton[][] buttons = new JButton[3][3];
    private boolean player = true;
    private int scoreP1 = 0;
    private int scoreP2 = 0;
    private JLabel score;

public Tictac() {
        f = new JFrame("Pirate Kurt Tic Tac🏴☠");
        f.setSize(600, 700);
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        f.setLocationRelativeTo(null);
        showMenu();
        f.setVisible(true);
    }
    //menupanel
    private void showMenu() {
        mp = new JPanel();
        mp.setLayout(new GridLayout(3, 1, 10, 10));
        mp.setBorder(BorderFactory.createEmptyBorder(80, 100, 80, 100));
        mp.setBackground(new Color(40, 40, 40));
        JButton start = new JButton("🏴‍️ Start Battle ☠");
        JButton rules = new JButton("📜 Pirate Rules");
        start.setBackground(new Color(184, 134, 11));
        start.setForeground(Color.black);
        rules.setBackground(new Color(160, 82, 45));
        rules.setForeground(Color.black);

        start.setFont(new Font("Serif", Font.BOLD, 20));
        rules.setFont(new Font("Serif", Font.BOLD, 20));

        start.addActionListener(e -> {
            f.remove(mp);
            game();
            f.revalidate();
            f.repaint();
        });

        rules.addActionListener(e -> {
            JOptionPane.showMessageDialog(f,
                "📜 Pirate Rules:\n" +
                "📜️ Take turns marking the treasure (☠ or 🏴‍).\n" +
                "📜️ Align 3 in a row to win.\n" +
                "📜 If all spots are full and no win, it's a draw!",
                "Pirate Rules",
                JOptionPane.INFORMATION_MESSAGE);
        });
        mp.add(start);
        mp.add(rules);
        f.setContentPane(mp);
    }

    //gamepanel
    private void game() {
        
        JPanel main = new JPanel(new BorderLayout());

        
        score = new JLabel(scoreText(), SwingConstants.CENTER);
        score.setFont(new Font("Serif", Font.BOLD, 22));
        score.setOpaque(true);
        score.setBackground(new Color(60, 60, 60));
        score.setForeground(Color.WHITE);
        main.add(score, BorderLayout.NORTH);

        
    //gamepanel
        gp = new JPanel(new GridLayout(3, 3));
        Font font = new Font("Serif", Font.BOLD, 48);
        gp.setBackground(new Color(25, 25, 25));

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j] = new JButton("");
                buttons[i][j].setFont(font);
                buttons[i][j].setFocusPainted(false);
                buttons[i][j].setBackground(new Color(139, 69, 19));
                buttons[i][j].setForeground(Color.black);
                buttons[i][j].addActionListener(new ButtonClickListener(i, j));
                gp.add(buttons[i][j]);
            }
        }

        main.add(gp, BorderLayout.CENTER);

        //restart
        JButton restartB = new JButton("🔁 Restart Game");
        restartB.setFont(new Font("Serif", Font.BOLD, 18));
        restartB.setBackground(new Color(128, 0, 0));
        restartB.setForeground(Color.WHITE);
        restartB.addActionListener(e -> {
            scoreP1 = 0;  
            scoreP2 = 0;  
             reset();
    scoreLabel();
        });

        JPanel bottom = new JPanel();
        bottom.setBackground(new Color(25, 25, 25));
        bottom.add(restartB);

        main.add(bottom, BorderLayout.SOUTH);
        //menu button
        JButton menuB = new JButton("Menu");
menuB.setFont(new Font("Serif", Font.BOLD, 18));
menuB.setBackground(new Color(0, 100, 0));
menuB.setForeground(Color.WHITE);
menuB.addActionListener(e -> {
    scoreP1 = 0;
    scoreP2 = 0;
    f.remove(gp);  
    showMenu();
    f.revalidate();
    f.repaint();
});

bottom = new JPanel();
bottom.setBackground(new Color(25, 25, 25));
bottom.add(restartB);
bottom.add(menuB);  

main.add(bottom, BorderLayout.SOUTH);

        f.setContentPane(main);
        f.revalidate();
    }

 private class ButtonClickListener implements ActionListener {
        int row, col;

        ButtonClickListener(int r, int c) {
            row = r;
            col = c;
        }

  public void actionPerformed(ActionEvent e) {
            JButton button = buttons[row][col];
            if (!button.getText().equals("")) return;

            button.setText(player ? "☠" : "🏴‍");

            if (win()) {
                if (player) scoreP1++;
                else scoreP2++;
                JOptionPane.showMessageDialog(f, "🏆 PIRATE " + (player ? "☠" : "🏴‍") + " WIN! ARRRRRR!!", "☠️ Battle Over, ARRRRR!!", JOptionPane.INFORMATION_MESSAGE);
                reset();
                scoreLabel();
            } else if (draw()) {
                JOptionPane.showMessageDialog(f, "⚔️ Draw!", "☠️ Battle Over, ARRRRR!!", JOptionPane.INFORMATION_MESSAGE);
                reset();
                scoreLabel();
            }
            player = !player;
        }
    }

private boolean win() {
        String mark = player ? "☠" : "🏴‍";

        for (int i = 0; i < 3; i++) {
            if (check(mark, buttons[i][0], buttons[i][1], buttons[i][2]) ||
                check(mark, buttons[0][i], buttons[1][i], buttons[2][i]))
                return true;
        }
        return check(mark, buttons[0][0], buttons[1][1], buttons[2][2]) ||
               check(mark, buttons[0][2], buttons[1][1], buttons[2][0]);
    }

private boolean check(String player, JButton b1, JButton b2, JButton b3) {
        return b1.getText().equals(player) &&
               b2.getText().equals(player) &&
               b3.getText().equals(player);
    }

private boolean draw() {
        for (JButton[] row : buttons)
            for (JButton b : row)
                if (b.getText().equals(""))
                    return false;
        return true;
    }

private void reset() {
        for (JButton[] row : buttons)
            for (JButton b : row)
                b.setText("");
        player = true;
    }

private void scoreLabel() {
        score.setText(scoreText());
    }
private String scoreText() {
        return "☠: " + scoreP1 + "   |   🏴‍: " + scoreP2;
    }

public static void main(String[] args) {
        SwingUtilities.invokeLater(Tictac::new);
    }
}
