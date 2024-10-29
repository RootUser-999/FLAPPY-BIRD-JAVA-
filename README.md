# FLAPPY-BIRD-JAVA-
package finalproject;

import javax.swing.JFrame;
import javax.swing.ImageIcon;

public class FinalProject {

    public static void main(String[] args) {
        ImageIcon image=new ImageIcon("D:\\coding\\oop java\\FinalProject\\src\\finalproject\\flap.png");
        JFrame frame = new JFrame();
        MyPanel panel = new MyPanel();
        frame.add(panel);
        frame.setIconImage(image.getImage());
        frame.setSize(1080, 720);
        frame.setResizable(false);
        
        frame.setTitle("Flappy");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
package finalproject;

import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Rectangle;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.Timer;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

public class MyPanel extends JPanel {
    private int frmWidth = 1270;
    private int frmHeight = 710;
    private int x = 100;
    private int y = 350;
    private int score = 0;
    JLabel label;
    JLabel label2;
    
    JButton retryBtn;

    private int obstacleX1 = 400;
    private int obstacleX2 = 400;
    private int obstacleX3 = 600;
    private int obstacleX4 = 600;
    private int obstacleX5 = 800;
    private int obstacleX6 = 800;
    private int obstacleX7 = 1000;
    private int obstacleX8 = 1000;
    private int obstacleX9 = 1200;
    private int obstacleX10 = 1200;

    private int obstacleSpeed = 4;
    private int gravity = 2;

    private ImageIcon backgroundImage;
    private ImageIcon obstacle;
    private ImageIcon obstacle2;
    private ImageIcon flap;

    private Timer timer;

    private boolean gameRunning = false;

   public MyPanel() {
        backgroundImage = new ImageIcon("D:\\coding\\oop java\\FinalProject\\src\\finalproject\\backs.png");
        obstacle = new ImageIcon("D:\\coding\\oop java\\FinalProject\\src\\finalproject\\ob.png");
        obstacle2 = new ImageIcon("D:\\coding\\oop java\\FinalProject\\src\\finalproject\\up.png");
        flap = new ImageIcon("D:\\coding\\oop java\\FinalProject\\src\\finalproject\\flap.png");
        
        JButton startBtn = new JButton("Start");
        startBtn.setFocusable(false);
        startBtn.setForeground(Color.black);
        startBtn.setBackground(Color.orange);
        startBtn.setFont(new Font("Arial Rounded MT Bold", Font.BOLD, 20));
        startBtn.setBounds(frmWidth / 2 - 50, 200, 100, 50);
        startBtn.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                startBtn.setVisible(false);
                startGame();
            }
        });
        
        //these two are needed to listen key from keyboard    make it active to respond to keyboard event
         this.setFocusable(true); 
         this.requestFocusInWindow(); 
        this.add(startBtn);
       
        
       //short cut to access mouse click methon of mouseAdapter 
        this.addMouseListener(new java.awt.event.MouseAdapter() {
            @Override
            public void mouseClicked(java.awt.event.MouseEvent evt) {
                if (gameRunning) {
                    y -= 30;
                    repaint();
                }
            }
        });
        
        //e.getKeyCode(); can be used for specific button
        this.addKeyListener(new java.awt.event.KeyAdapter(){
            @Override
            public void keyPressed(KeyEvent e){
                if (gameRunning) {
                    y -= 30;
                    repaint();
                }
                
            }
        });
        
    timer = new Timer(20, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                moveObstacles();
                Gravity();
                collisionDetect();
                updateScoreLabel();
                
                
            }
        });
    }

    
    
    private void startGame() {
        gameRunning = true;
        timer.start();
    }

    public void Gravity() {
        if (gameRunning) {
            y += gravity;
            if (y > 610) {
                y = 610;
            } 
            else if (y < 0) {
                y = 0;
            }
        }
    }

    

    private void moveObstacles() {
        if (gameRunning) {
            obstacleX1 -= obstacleSpeed;
            obstacleX2 -= obstacleSpeed;
            obstacleX3 -= obstacleSpeed;
            obstacleX4 -= obstacleSpeed;
            obstacleX5 -= obstacleSpeed;
            obstacleX6 -= obstacleSpeed;
            obstacleX7 -= obstacleSpeed;
            obstacleX8 -= obstacleSpeed;
            obstacleX9 -= obstacleSpeed;
            obstacleX10 -= obstacleSpeed;

            if (obstacleX1 < -150) {
                obstacleX1 = frmWidth-100;

            }
            if (obstacleX2 < 0) {
                obstacleX2 = frmWidth;
                score++;
            }
            if (obstacleX3 < 0) {
                obstacleX3 = frmWidth;

            }
            if (obstacleX4 < 0) {
                obstacleX4 = frmWidth;
                score++;
            }
            if (obstacleX5 < 0) {
                obstacleX5 = frmWidth;

            }
            if (obstacleX6 < 0) {
                obstacleX6 = frmWidth;
                score++;
            }
            if (obstacleX7 < 0) {
                obstacleX7 = frmWidth;
            }
            if (obstacleX8 < 0) {
                obstacleX8 = frmWidth;
                score++;
            }
            if (obstacleX9 < 0) {
                obstacleX9 = frmWidth;
            }
            if (obstacleX10 < 0) {
                obstacleX10 = frmWidth;
                score++;
            }
            repaint();
 
        }
    }

    private void updateScoreLabel() {
        if (gameRunning) {
            if (label != null) {
                remove(label);
            }

            label = new JLabel("Score=" + score);
            label.setFont(new Font("Bauhaus 93", Font.BOLD, 18));
            label.setBounds(200, 50, 200, 100);
            label.setForeground(Color.yellow);
            add(label);
        }
    }

    @Override
    protected void paintComponent(Graphics g) {
        if (gameRunning) {
            g.drawImage(backgroundImage.getImage(), 0, 0, frmWidth, frmHeight, this);
            g.drawImage(flap.getImage(), x, y, 50, 50, this);
            g.drawImage(obstacle.getImage(), obstacleX1, 440, 100, 210, this);
            g.drawImage(obstacle2.getImage(), obstacleX2, 0, 100, 300, this);
            g.drawImage(obstacle.getImage(), obstacleX3, 350, 100, 300, this);
            g.drawImage(obstacle2.getImage(), obstacleX4, 0, 100, 200, this);
            g.drawImage(obstacle.getImage(), obstacleX5, 450, 100, 200, this);
            g.drawImage(obstacle2.getImage(), obstacleX6, 0, 100, 300, this);
            g.drawImage(obstacle2.getImage(), obstacleX7, 0, 100, 250, this);
            g.drawImage(obstacle.getImage(), obstacleX8, 350, 100, 300, this);
            g.drawImage(obstacle2.getImage(), obstacleX9, 0, 100, 280, this);
            g.drawImage(obstacle.getImage(), obstacleX10, 350, 100, 300, this);
            
        }
    }
 public void collisionDetect() {
    if (gameRunning) {
        Rectangle flapBounds = new Rectangle(x, y, 40, 40);
        Rectangle obstacleBounds1 = new Rectangle(obstacleX1, 440, 100, 210);
        Rectangle obstacleBounds2 = new Rectangle(obstacleX2, 0, 100, 300);
        Rectangle obstacleBounds3 = new Rectangle(obstacleX3, 350, 100, 300);
        Rectangle obstacleBounds4 = new Rectangle(obstacleX4, 0, 100, 200);
        Rectangle obstacleBounds5 = new Rectangle(obstacleX5, 450, 100, 200);
        Rectangle obstacleBounds6 = new Rectangle(obstacleX6, 0, 100, 300);
        Rectangle obstacleBounds7 = new Rectangle(obstacleX7, 0, 100, 250);
        Rectangle obstacleBounds8 = new Rectangle(obstacleX8, 350, 100, 300);
        Rectangle obstacleBounds9 = new Rectangle(obstacleX9, 0, 100, 275);
        Rectangle obstacleBounds10 = new Rectangle(obstacleX10, 350, 100, 300);

        if (flapBounds.intersects(obstacleBounds1) || flapBounds.intersects(obstacleBounds2) ||
            flapBounds.intersects(obstacleBounds3) || flapBounds.intersects(obstacleBounds4) ||
            flapBounds.intersects(obstacleBounds5) || flapBounds.intersects(obstacleBounds6) ||
            flapBounds.intersects(obstacleBounds7) || flapBounds.intersects(obstacleBounds8) ||
            flapBounds.intersects(obstacleBounds9) || flapBounds.intersects(obstacleBounds10)) {

            gameRunning = false;
            label2 = new JLabel();
            label2.setText("Game Over \nTotal Score: " + this.score);
            label2.setFont(new Font("Times New Roman", Font.BOLD, 25));
            label2.setForeground(Color.RED);
            label2.setBounds(frmWidth / 2 - 200, frmHeight / 2 - 100, 400, 100);
            add(label2);
            
           retryBtn = new JButton("Retry");
           retryBtn.setForeground(Color.black);
           retryBtn.setBackground(Color.orange);
            retryBtn.setBounds(frmWidth / 2 - 100, frmHeight / 2-20, 100, 30);
            retryBtn.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    restartGame();
                }
            });
            add(retryBtn);
            }
        
}}
    
public void restartGame() {
    if (!gameRunning) {
        x = 100;
        y = 350;
        score = 0;

        obstacleX1 = 400;
        obstacleX2 = 400;
        obstacleX3 = 600;
        obstacleX4 = 600;
        obstacleX5 = 800;
        obstacleX6 = 800;
        obstacleX7 = 1000;
        obstacleX8 = 1000;
        obstacleX9 = 1200;
        obstacleX10 = 1200;

        obstacleSpeed = 3;

        label2.setVisible(false);
        retryBtn.setVisible(false);

        startGame();
    }
}

        }
    




          

