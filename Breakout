import java.awt.*;
import java.awt.event.*;
import javax.swing.*;


public class Breakout extends JFrame {
    final int windowWidth = 400;
    final int windowHeight = 500;

    public static void main(String[] args){
        new Breakout();
    }

    public Breakout() {
        Dimension dimOfScreen =
               Toolkit.getDefaultToolkit().getScreenSize();

        setBounds(dimOfScreen.width/2 - windowWidth/2,
                  dimOfScreen.height/2 - windowHeight/2,
                  windowWidth, windowHeight);
        setResizable(false);
        setTitle("Software Development II");
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        MyJPanel panel= new MyJPanel();
        Container c = getContentPane();
        c.add(panel);
        setVisible(true);
    }

    public class MyJPanel extends JPanel implements
       ActionListener, MouseListener, MouseMotionListener {
        /* 全体の設定に関する変数 */
        Dimension dimOfPanel;
        Timer timer;
        ImageIcon iconRacket, iconBlock;
        Image imgRacket, imgBlock;

        /* ラケットに関する変数 */
        int myHeight, myWidth;
        int myX, myY, tempMyX;
        int gap = 100;
        int myBallX, myBallY;
        boolean isBallActive;
        int life = 3; //ボール残り
        int vx = 7; //x速度
        int vy = 10; //y速度

        /* ブロックに関する変数 */
        int numOfBlock = 30;
        int numOfAlive = numOfBlock;
        int blockWidth, blockHeight;
        int[] blockX = new int[numOfBlock];
        int[] blockY = new int[numOfBlock];
        boolean[] isBlockAlive = new boolean[numOfBlock];



        /* コンストラクタ（ゲーム開始時の初期化）*/
        public MyJPanel() {
            // 全体の設定
            setBackground(Color.black);
            addMouseListener(this);
            addMouseMotionListener(this);
            timer = new Timer(50, this);
            timer.start();



            // 画像の取り込み
            imgRacket = getImg("horizontal bar.png");

            myWidth = imgRacket.getWidth(this);
            myHeight = imgRacket.getHeight(this);

            imgBlock = getImg("block.png");
            blockWidth = imgBlock.getWidth(this);
            blockHeight = imgBlock.getHeight(this);

            // ラケットとブロックの初期化
            initBall();
            initBlock();
        }

        /* パネル上の描画 */
        public void paintComponent(Graphics g) {
            dimOfPanel = getSize();
            super.paintComponent(g);
            g.drawString("", 100, 100);

            // 各要素の描画
            drawRacket(g);       // ラケット
            drawBall(g);     // ボール
            drawBlock(g);    // ブロック

            g.setColor(Color.yellow);
            g.drawString("残り:" + String.valueOf(life),10, 20);

            // ブロックを全て消滅させた時の終了処理
            if (numOfAlive == 0) {
              g.drawString("Game Clear! クリックして再スタート", windowWidth/4, windowHeight/2);
            timer.stop();
            }

            if (life==0){
              g.drawString("Game Over! クリックして再スタート", windowWidth/4, windowHeight/2);
              timer.stop();
            }
        }

        /* 一定時間ごとの処理（ActionListener に対する処理）*/
        public void actionPerformed(ActionEvent e) {
            repaint();
        }

        /* MouseListener に対する処理 */
        // マウスボタンをクリックする
        public void mouseClicked(MouseEvent e) {
        }

        // マウスボタンを押下する
        public void mousePressed(MouseEvent e) {
            if (!isBallActive) {
                myBallX = tempMyX + myWidth/2;
                myBallY = myY;
                isBallActive = true;
                vy = 15;
            }

            if (life==0 || numOfAlive == 0){
              initBlock();
              initBall();
              life = 3;
              numOfAlive = numOfBlock;
              timer.start();
        }
      }

        // マウスボタンを離す
        public void mouseReleased(MouseEvent e) {
        }

        // マウスが領域外へ出る
        public void mouseExited(MouseEvent e) {
        }

        // マウスが領域内に入る
        public void mouseEntered(MouseEvent e) {
        }

        /* MouseMotionListener に対する処理 */
        // マウスを動かす
        public void mouseMoved(MouseEvent e) {
            myX = e.getX();
        }

        // マウスをドラッグする
        public void mouseDragged(MouseEvent e) {
            myX = e.getX();
        }

        /* 画像ファイルから Image クラスへの変換 */
        public Image getImg(String filename) {
            ImageIcon icon = new ImageIcon(filename);
            Image img = icon.getImage();

            return img;
        }

        /* ラケットの初期化 */
        public void initBall() {
            myX = windowWidth / 2;
            myY = windowHeight - 100;
            tempMyX = windowWidth / 2;
            isBallActive = false;
        }

        /* ブロックの初期化 */
        public void initBlock() {
            int k=0;
            for (int i=0; i<6; i++) {
              for (int j=0;j<5;j++){
                  blockX[k] = 70*i;
                  blockY[k] = 50+20*j;
                 k=k+1;
              }
            }

            for (int i=0; i<numOfBlock; i++) {
                isBlockAlive[i] = true;
            }
        }

        /* ラケットの描画 */
        public void drawRacket(Graphics g) {
            if (Math.abs(tempMyX - myX) < gap) {
                if (myX < 0) {
                    myX = 0;
                } else if (myX+myWidth > dimOfPanel.width) {
                    myX = dimOfPanel.width - myWidth;
                }
                tempMyX = myX;
                g.drawImage(imgRacket, tempMyX, myY, this);
            } else {
                g.drawImage(imgRacket, tempMyX, myY, this);
            }
        }

        /* ボールの描画 */
        public void drawBall(Graphics g) {
            if (isBallActive) {
                // ボールの配置

                myBallY -= vy;
                myBallX -= vx;

                g.setColor(Color.white);
                g.fillOval(myBallX, myBallY, 9, 9);

                // ボールの各ブロックへの当たり判定
                for (int i=0; i<numOfBlock; i++) {
                    if (isBlockAlive[i]) {
                        if ((myBallX >= blockX[i]) &&
                            (myBallX <= blockX[i]+blockWidth) &&
                            (myBallY >= blockY[i]) &&
                            (myBallY <= blockY[i]+blockHeight)) {
                            isBlockAlive[i] = false;
                            vy=-vy;
                            numOfAlive--;
                        }
                    }
                }

                // ボールとラケットの当たり判定
                if ((myBallX >= myX) &&
                    (myBallX <= myX+myWidth) &&
                    (myBallY >= myY) &&
                    (myBallY <= myY+myHeight)) {
                    vy=-vy;
                }
                // ボールがウィンドウ壁に触れたときのボールの反射
                if (myBallY < 0) {
                  vy = -vy;
                }

                if (myBallX < 0 || myBallX > windowWidth) {
                  vx = -vx;
                }

                //ボールが画面下に落ちた時
                if (myBallY > windowHeight){
                  isBallActive = false;

                  life = life - 1;
                }
            }
        }

        /* ブロックの描画 */
        public void drawBlock(Graphics g) {
            for (int i=0; i<numOfBlock; i++) {
                if (isBlockAlive[i]) {
                    g.drawImage(imgBlock, blockX[i],
                                          blockY[i], this);
                }
            }
        }
    }
}
