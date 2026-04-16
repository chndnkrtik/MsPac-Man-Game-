# MsPac-Man-Game-
JAVA project Report
code:
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class MsPacmanGame extends JPanel implements ActionListener, KeyListener {

    Timer timer;
    int tile = 30;
    int rows = 20, cols = 20;

    int playerX = 1, playerY = 1;
    int dx = 0, dy = 0;

    int ghostX = 10, ghostY = 10;

    int score = 0;

    int[][] map = new int[rows][cols];

    public MsPacmanGame() {
        setFocusable(true);
        addKeyListener(this);

        initMap();

        timer = new Timer(120, this);
        timer.start();
    }

    void initMap() {
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (i == 0 || j == 0 || i == rows - 1 || j == cols - 1 || (i % 4 == 0 && j % 4 != 0)) {
                    map[i][j] = 1; // wall
                } else {
                    map[i][j] = 0; // path
                }
            }
        }
    }

    public void paintComponent(Graphics g) {
        super.paintComponent(g);

        // Background
        g.setColor(Color.BLACK);
        g.fillRect(0, 0, getWidth(), getHeight());

        // Draw map
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (map[i][j] == 1) {
                    g.setColor(Color.BLUE);
                    g.fillRect(j * tile, i * tile, tile, tile);
                } else {
                    g.setColor(Color.WHITE);
                    g.fillOval(j * tile + 12, i * tile + 12, 6, 6);
                }
            }
        }

        // Player (Ms Pac-Man)
        g.setColor(Color.YELLOW);
        g.fillOval(playerY * tile, playerX * tile, tile, tile);

        // Ghost
        g.setColor(Color.RED);
        g.fillOval(ghostY * tile, ghostX * tile, tile, tile);

        // Score UI
        g.setColor(Color.WHITE);
        g.setFont(new Font("Arial", Font.BOLD, 18));
        g.drawString("Score: " + score, 10, rows * tile + 25);
    }

    public void actionPerformed(ActionEvent e) {
        movePlayer();
        moveGhost();

        if (playerX == ghostX && playerY == ghostY) {
            timer.stop();
            JOptionPane.showMessageDialog(this, "Game Over! Score: " + score);
        }

        repaint();
    }

    void movePlayer() {
        int nx = playerX + dx;
        int ny = playerY + dy;

        if (map[nx][ny] == 0) {
            playerX = nx;
            playerY = ny;
            score++;
        }
    }

    void moveGhost() {
        Random r = new Random();
        int[] d = {-1, 0, 1};

        int nx = ghostX + d[r.nextInt(3)];
        int ny = ghostY + d[r.nextInt(3)];

        if (nx >= 0 && ny >= 0 && nx < rows && ny < cols && map[nx][ny] == 0) {
            ghostX = nx;
            ghostY = ny;
        }
    }

    public void keyPressed(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_UP) { dx = -1; dy = 0; }
        if (e.getKeyCode() == KeyEvent.VK_DOWN) { dx = 1; dy = 0; }
        if (e.getKeyCode() == KeyEvent.VK_LEFT) { dx = 0; dy = -1; }
        if (e.getKeyCode() == KeyEvent.VK_RIGHT) { dx = 0; dy = 1; }
    }

    public void keyReleased(KeyEvent e) {}
    public void keyTyped(KeyEvent e) {}

    public static void main(String[] args) {
        JFrame frame = new JFrame("Ms Pac-Man");

        MsPacmanGame game = new MsPacmanGame();
        frame.add(game);

        frame.setSize(620, 700);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setResizable(false);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
