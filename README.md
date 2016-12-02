# UnlockTheBox
Final Java Project for School (15 hrs)

Synopsis

This is the my final project for JAVA in school. They ask me to create a program of my own choosing. I chose to make a simple fast game to play. 

Code Example

//A program for a fast simple game. Click to move a character, get key to unlock a chest, all displayed on a gameboard.
//Created and Modified by Zach Iroz 12/2/2016

//Declare necessary imports to run program
import java.awt.event.*;
import javax.swing.*;
import java.awt.*;
import java.awt.image.*;
import java.awt.BorderLayout.*;
import java.awt.Color;
import java.lang.Object;
import java.io.File;
import javax.imageio.ImageIO;

//Declare class, extention and Implementation
public class UnlockTheBox extends JFrame implements ActionListener {
	//Declare Variables, Panes, Panels, Images, Buttons, Menu bars and options and Labels,
	final int ROWS = 5;
	final int COLS = 5;
	final int GAP = 2;
	final int NUM = ROWS * COLS;
	int x;
	boolean hasKey=false;
	JPanel pane1 = new JPanel(new BorderLayout());
	JPanel pane2 = new JPanel(new GridLayout(ROWS, COLS, GAP, GAP));
	JPanel[] panelArray = new JPanel[NUM];
	JSplitPane splitPane = new JSplitPane(JSplitPane.VERTICAL_SPLIT, pane1, pane2);
	ImageIcon characterIcon;
	ImageIcon chestIcon;
	ImageIcon keyIcon;
	JLabel characterLabel;
	JLabel chestLabel;
	JLabel keyLabel;
	JButton nb = new JButton("∆");
	JButton sb = new JButton("v");
	JButton eb = new JButton("»");
	JButton wb = new JButton("«");
	JButton cb = new JButton("Get Key to Unlock Box");
	int charLocation = 0;
	int chestLocation = 1;
	int keyLocation =2;
	JMenuBar mainBar = new JMenuBar();
	JMenu menu1 = new JMenu("File");
	JMenuItem newGame = new JMenuItem("New Game");
	
	
	public UnlockTheBox() {
		//add items to frames, panes, labels, buttons etc. and set dimensions, colors, images etc.
		super("Unlock The Box (click to move)");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setJMenuBar(mainBar);
		mainBar.add(menu1);
		menu1.add(newGame);
		add(splitPane);
		splitPane.setOneTouchExpandable(false);
		splitPane.setDividerLocation(150);
		Dimension minimumSize = new Dimension(200, 200);
		pane1.setMinimumSize(minimumSize);
		pane2.setSize(400, 400);
		
		//create image for character and scales to fit pane
		characterIcon = new ImageIcon("Rigby_character.png");
		Image characterImage = characterIcon.getImage();
		characterImage = characterImage.getScaledInstance(50, 75, java.awt.Image.SCALE_SMOOTH);
		characterIcon = new ImageIcon(characterImage);
		characterLabel = new JLabel(characterIcon);
		
		//create image for chest and scales to fit pane
		chestIcon = new ImageIcon("chest-treasure-box-container-wooden-pirate-closed.png");
		Image chestImage = chestIcon.getImage();
		chestImage = chestImage.getScaledInstance(75, 75, java.awt.Image.SCALE_SMOOTH);
		chestIcon = new ImageIcon(chestImage);
		chestLabel = new JLabel(chestIcon);
		
		//create image for key and scales to fit pane
		keyIcon = new ImageIcon("21606_MAJOR_KEY_Necklace.png");
		Image keyImage = keyIcon.getImage();
		keyImage = keyImage.getScaledInstance(25, 25, java.awt.Image.SCALE_SMOOTH);
		keyIcon = new ImageIcon(keyImage);
		keyLabel = new JLabel(keyIcon);

		//initialize panel array
		for(x = 0; x<NUM; ++x){
			panelArray[x] = new JPanel();
			//pane2.add(panelArray[x]);
			panelArray[x].setBorder(BorderFactory.createLineBorder(Color.black));
			panelArray[x].setVisible(true);
		}
		//Shuffle panel Array to Randomize game each time
		for(int a= 0; a < 25; ++a){
			JPanel tempPanel = new JPanel();
			
			int b = (int)(Math.random()*24);
			//checks Locations of character, chest and key before shuffle and tracks locations to where it's moved to
			if(a==charLocation && b != chestLocation && b != keyLocation){
			charLocation=b;
			}
			if(a==chestLocation && b != charLocation && b != keyLocation){
				chestLocation=b;
			}
			if(a==keyLocation && b!= chestLocation && b != charLocation){
				keyLocation=b;
			}//end locations tracking
			
			//shuffles panel array based on math.random
			tempPanel = panelArray[a];
			panelArray[a] = panelArray[b];
			panelArray[b] = tempPanel;
		}//end shuffle
		
		//Makes Starting character location border green to easily identify
		panelArray[charLocation].setBorder(BorderFactory.createLineBorder(Color.GREEN));
		
		//add items to panel array and instantiate array to pane
		panelArray[charLocation].add(characterLabel);
		panelArray[chestLocation].add(chestLabel);
		panelArray[keyLocation].add(keyLabel);
		for(x=0; x<NUM; ++x){
			pane2.add(panelArray[x]);
		}
		//add layouts and action listeners to buttons and menu 
		pane1.setLayout(new BorderLayout());
		pane1.add(nb, BorderLayout.NORTH);
	 	pane1.add(sb, BorderLayout.SOUTH);
		pane1.add(eb, BorderLayout.EAST);
		pane1.add(wb, BorderLayout.WEST); 
		pane1.add(cb, BorderLayout.CENTER);
		cb.setEnabled(false);
		nb.addActionListener(this);
		sb.addActionListener(this);
		eb.addActionListener(this);
		wb.addActionListener(this);
		newGame.addActionListener(this);
		
		
	}
	//Verify image was created and file could be found
	protected ImageIcon createImageIcon(String path,
										   String description) {
		java.net.URL imgURL = getClass().getResource(path);
		if (imgURL != null) {
			return new ImageIcon(imgURL, description);
		} else {
			System.err.println("Couldn't find file: " + path);
			return null;
		}
	}
	//Action Listener for buttons and Locations
	public void actionPerformed(ActionEvent e)
	{
		//declare variables and source
		int z=0;
		Object source = e.getSource();
		
		//Establish what the source of event was and make appropriate changes
		if(source  == nb){
			if(charLocation>4)
			{
				//moves character north
				z = charLocation;
				charLocation -= 5;
			}
		}
		else if(source == sb) {
			if(charLocation<20)
			{
				//moves character south
				z = charLocation;
				charLocation += 5;
			
			}
		}
		else if(source == eb) {
			if(charLocation % 5!=4)
			{
				//moves character east
				z = charLocation;
				charLocation += 1;
			}
		}
		else if(source == wb) {      
			if(charLocation % 5 != 0)
			{
				//moves character west
				z = charLocation;
				charLocation -= 1;
			}
		}else{
			//creates new instantiation of game based on new game selection
			UnlockTheBox unlockBox = new UnlockTheBox();
			unlockBox.setSize(400, 600);
			unlockBox.setVisible(true);
		}
		
		if(charLocation==keyLocation){
			//if charachter moves to key, removes key and changes status of hasKey to true
			hasKey=true;
			panelArray[charLocation].remove(keyLabel);
			panelArray[charLocation].revalidate();
			panelArray[charLocation].repaint();
			panelArray[charLocation].add(characterLabel);
						
		}
		if(charLocation==chestLocation && hasKey){
			//checks to see if has key when getting to chest then removes chest and changes character image and scale
			panelArray[charLocation].remove(chestLabel);
			panelArray[charLocation].revalidate();
			panelArray[charLocation].repaint();
			
			characterIcon = new ImageIcon("Rigby.png");
			Image characterImage = characterIcon.getImage();
			characterImage = characterImage.getScaledInstance(50, 75, java.awt.Image.SCALE_SMOOTH);
			characterIcon = new ImageIcon(characterImage);
			characterLabel.setIcon(characterIcon);
			panelArray[charLocation].add(characterLabel);
			//disables buttons
			nb.setEnabled(false);
			sb.setEnabled(false);
			eb.setEnabled(false);
			wb.setEnabled(false);
			//changes text on center button
			cb.setText("CONGRATULATIONS!!!");
						
		}else if(charLocation==chestLocation && !hasKey){
			//keeps character location from changing if no key and cant unlock the chest
			charLocation=z;
		}
		//repaints gameboard with all correct changes made after actions
		panelArray[(charLocation)].add(characterLabel);
		panelArray[z].setBorder(BorderFactory.createLineBorder(Color.BLACK));
		panelArray[z].revalidate();
		panelArray[z].repaint();
		for(x=0; x<NUM; ++x){
			pane2.remove(panelArray[x]);
			pane2.repaint();
			pane2.revalidate();
			pane2.add(panelArray[x]);
		}
		pane2.setVisible(true);
	}
}


Motivation

I enjoy fast simple games and I wanted to focus on the GUI of Java. I also wanted something that would fit within my schools alotted time frame to complete the project of 15 hours. I completed the project in less time so I could have maybe made it slightly more complex.


Tests

By downloading the UnlockTheBox.zip, unzip and launch and UnlockTheBoxDemo in coderunner on mac, then run the UnlockTheBoxDemo to test the game.
