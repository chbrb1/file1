# file1

# Project

package dodge;

import java.awt.Color;
import java.awt.Cursor;
import java.awt.Graphics;
import java.awt.Image;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseMotionAdapter;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;

@SuppressWarnings("serial")
public class Project1 extends JFrame {

	private Image screenImage;
	private Graphics screenGraphic;

	private Image background = new ImageIcon(Main.class.getResource("../../bin/image/introBackGround.jpg")).getImage();
	private JLabel menuBar = new JLabel(new ImageIcon(Main.class.getResource("../../bin/image/MenuBar.png")));
	// 기본 배경과 상단 바에 이미지를 사용

	private ImageIcon startButtonEImage = new ImageIcon(Main.class.getResource("../../bin/image/startButtonEntered.png"));
	private ImageIcon startButtonBImage = new ImageIcon(Main.class.getResource("../../bin/image/startButtonBasic.png"));
	private ImageIcon quitButtonEImage = new ImageIcon(Main.class.getResource("../../bin/image/quitButtonEntered.png"));
	private ImageIcon quitButtonBImage = new ImageIcon(Main.class.getResource("../../bin/image/quitButtonBasic.png"));
	private ImageIcon backButtonEImage = new ImageIcon(Main.class.getResource("../../bin/image/exitButtonEnter.png"));
	private ImageIcon backButtonBImage = new ImageIcon(Main.class.getResource("../../bin/image/exitButtonBasic.png"));
	//각 버튼들에 이미지를 넣기위하여 ImageIcon사용

	private JButton startButton = new JButton(startButtonBImage);
	private JButton quitButton = new JButton(quitButtonBImage);
	private JButton backButton = new JButton(backButtonBImage);
	// JButton으로 버튼 생성

	private int mouse_X, mouse_Y;

	public Project1() {

		setUndecorated(true); // 기존 메뉴바 삭제
		setTitle("Space Dodge");
		setSize(1280, 720);
		setResizable(false); // 사이즈를 임의로 변경못함
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
		setBackground(new Color(0, 0, 0, 0));
		setLayout(null);

		startButton.setBounds(100, 500, 400, 100);
		startButton.setBorderPainted(false);
		startButton.setContentAreaFilled(false);
		startButton.setFocusPainted(false);
		startButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				startButton.setIcon(startButtonEImage);
				startButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				// 마우스를 버튼에 갖다주었을 때 HANDCURSOR를 이용하여 손 모양이 나오게 설정
			}

			@Override
			public void mouseExited(MouseEvent e) {
				startButton.setIcon(startButtonBImage);
				startButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
				// 마우스가 버튼에 있지않을땐 default
			}

			@Override
			public void mousePressed(MouseEvent e) {
				startButton.setVisible(false);
				quitButton.setVisible(false);
				background = new ImageIcon(Main.class.getResource("../../bin/image/mainBackGround.jpg")).getImage();
			}
		});
		add(startButton);

		quitButton.setBounds(800, 500, 400, 100);
		quitButton.setBorderPainted(false);
		quitButton.setContentAreaFilled(false);
		quitButton.setFocusPainted(false);
		quitButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				quitButton.setIcon(quitButtonEImage);
				quitButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
				
			}

			@Override
			public void mouseExited(MouseEvent e) {
				quitButton.setIcon(quitButtonBImage);
				quitButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			// quit버튼을 누르면 프로그램이 종료
			public void mousePressed(MouseEvent e) {
				try {
					Thread.sleep(1000);
				} catch (InterruptedException ex) {
					ex.printStackTrace();
				}
				System.exit(0); 
			}
		});
		add(quitButton);

		backButton.setVisible(false);
		backButton.setBounds(20, 50, 30, 30);
		backButton.setBorderPainted(false);
		backButton.setContentAreaFilled(false);
		backButton.setFocusPainted(false);
		
		
		startButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {
				backButton.setIcon(backButtonEImage);
				backButton.setCursor(new Cursor(Cursor.HAND_CURSOR));
			}

			@Override
			public void mouseExited(MouseEvent e) {
				backButton.setIcon(backButtonBImage);
				backButton.setCursor(new Cursor(Cursor.DEFAULT_CURSOR));
			}

			@Override
			public void mousePressed(MouseEvent e) {
				backButton.setVisible(true);
			}
		});
		add(backButton);
		
		// 기존 메뉴바가 지정한 이미지로 덮혔기 때문에 마우스로 끌어서 이동 가능할 수 있게 mousePressed로 좌표를 따주고 
		// mouseDragged를 사용하여 원하는 위치로 끌어갈수 있게 기능 구현
		menuBar.setBounds(0, 0, 1280, 30); // 메뉴바 좌표, 크기 구현
		menuBar.addMouseListener(new MouseAdapter() {
			@Override
			public void mousePressed(MouseEvent e) {
				mouse_X = e.getX();
				mouse_Y = e.getY();
			}
		});

		menuBar.addMouseMotionListener(new MouseMotionAdapter() {
			@Override
			public void mouseDragged(MouseEvent e) {
				int x = e.getXOnScreen();
				int y = e.getYOnScreen();
				setLocation(x - mouse_X, y - mouse_Y);
			}
		});
		add(menuBar);
	}

	public void paint(Graphics g) {

		screenImage = createImage(1280, 720);
		screenGraphic = screenImage.getGraphics();
		screenDraw(screenGraphic);
		g.drawImage(screenImage, 0, 0, null);
	}

	public void screenDraw(Graphics g) {

		g.drawImage(background, 0, 0, null);
		paintComponents(g); // 항상 고정되는 이미지인 메뉴바를 설정하기위해 사용
		this.repaint();
	}
}
