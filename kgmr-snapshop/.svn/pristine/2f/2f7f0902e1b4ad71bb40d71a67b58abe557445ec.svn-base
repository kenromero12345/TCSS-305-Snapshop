/*
 * TCSS 305 - Autumn 2018
 * Assignment 4 - SnapShop
 */

package gui;

import filters.EdgeDetectFilter;
import filters.EdgeHighlightFilter;
import filters.Filter;
import filters.FlipHorizontalFilter;
import filters.FlipVerticalFilter;
import filters.GrayscaleFilter;
import filters.SharpenFilter;
import filters.SoftenFilter;
import image.PixelImage;
import java.awt.BorderLayout;
import java.awt.Dimension;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.SwingConstants;

/**
 * Application for editing images.
 * 
 * @author Ken Gil Romero
 * @version 11/2/18
 */
public class SnapShopGUI extends JFrame {
    
    /**
     * A generated serial version UID for object Serialization.
     */
    private static final long serialVersionUID = -629306918261849345L;
    
    /**
     * Filters the images can use.
     */
    private final List<Filter> myFilters;
    
    /**
     * Buttons that will filter the image.
     */
    private final List<JButton> myFilterButtons;
    
    /**
     * Dimension of the size of the application depending 
     * on the subcomponent's preferred size.
     */
    private Dimension myMinFrameSize;
   
    /**
     * Button for opening the image.
     */
    private JButton myButtonOpen;
    
    /**
     * Button for saving the current image.
     */
    private JButton myButtonSave;
    
    /**
     * Button for closing the image.
     */
    private JButton myButtonCloseImage; 
    
    /**
     * The image that is being edited.
     */
    private PixelImage myImage;
    
    /**
     * The file chooser for opening and saving an image.
     */
    private final JFileChooser myFileChooser;
    
    /**
     * The image that is being edited's  label.
     */
    private final JLabel myImageLabel;
    
    /**
     * Constructor of the class for setting up the application.
     */
    public SnapShopGUI() {
        // The JFrame's overloaded constructor can set the JFrame title.
        super("TCSS 305 - Assignment 4");
        
        myFileChooser = new JFileChooser(".");
        myImageLabel = new JLabel("", SwingConstants.CENTER);
        myFilters = new ArrayList<Filter>();
        myFilterButtons = new ArrayList<JButton>();
        
        addFilters();
        setUpFilterButtonsAndTheirActionListener();
        setUpNonFilterButtons();
        toggleButtons();
        doNonFilterActionListeners();
    }

    /**
     * Setting up the filter buttons and adding it to the list of buttons
     *  and setting their behavior.
     */
    private void setUpFilterButtonsAndTheirActionListener() {
        for (final Filter f: myFilters) {
            final JButton button = new JButton(f.getDescription());
            myFilterButtons.add(button);
            button.addActionListener(theEvent -> {
                f.filter(myImage);
                myImageLabel.setIcon(new ImageIcon(myImage));
            });
        }
    }

    /**
     * Adds the filters to the list of filters.
     */
    private void addFilters() {
        myFilters.add(new EdgeDetectFilter());
        myFilters.add(new EdgeHighlightFilter());
        myFilters.add(new FlipHorizontalFilter());
        myFilters.add(new FlipVerticalFilter());
        myFilters.add(new GrayscaleFilter());
        myFilters.add(new SharpenFilter());
        myFilters.add(new SoftenFilter());
    }
    
    /**
     * Toggling most of the buttons if they should be clickable.
     */
    private void toggleButtons() {
        final boolean bool = !myButtonSave.isEnabled();
        
        for (final JButton b : myFilterButtons) {
            b.setEnabled(bool);
        }
        myButtonSave.setEnabled(bool);
        myButtonCloseImage.setEnabled(bool);
    }

    /**
     * Setting up the buttons with their name.
     */
    private void setUpNonFilterButtons() {
        myButtonOpen = new JButton("Open...");
        myButtonSave = new JButton("Save As...");
        myButtonCloseImage = new JButton("Close Image");
    }
    
    /**
     * Lay out the components and makes this frame visible.
     */  
    private void setUpComponents() {
        final JPanel buttonPanelN = new JPanel();
        for (final JButton button : myFilterButtons) {
            buttonPanelN.add(button);
        }
        
        final JPanel buttonPanelS = new JPanel();
        buttonPanelS.add(myButtonOpen);
        buttonPanelS.add(myButtonSave);
        buttonPanelS.add(myButtonCloseImage);
        
        add(buttonPanelN, BorderLayout.NORTH);
        add(myImageLabel, BorderLayout.CENTER);
        add(buttonPanelS, BorderLayout.SOUTH);
    }
    
    /**
     * Start of the application.
     */
    public void start() {
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setUpComponents();
        packAndSetMinFrameSize();
        setVisible(true);
    }

    /**
     * Resizing the frame to the preferred dimensions 
     * of the subcomponents and setting the minimum size.
     */
    private void packAndSetMinFrameSize() {
        pack();
        if (myMinFrameSize == null) {
            myMinFrameSize = getSize();
        }
        setMinimumSize(getSize());
    }

    /**
     * The behavior of the non filter buttons.
     */
    private void doNonFilterActionListeners() {
        myButtonOpen.addActionListener(theEvent -> {
            if (myFileChooser.showOpenDialog(this) == JFileChooser.APPROVE_OPTION) {
                try {
                    myImage = PixelImage.load(myFileChooser.getSelectedFile());
                    myImageLabel.setIcon(new ImageIcon(myImage));
                    if (!myButtonSave.isEnabled()) {
                        toggleButtons();
                    }
                    setMinimumSize(myMinFrameSize);   
                    packAndSetMinFrameSize();
                } catch (final IOException e) {
                    JOptionPane.showMessageDialog(this,
                        "The selected file did not contain an image!");
                } 
            }
        });
        myButtonSave.addActionListener(theEvent -> {
            if (myFileChooser.showSaveDialog(this) == JFileChooser.APPROVE_OPTION) {
                try {
                    myImage.save(myFileChooser.getSelectedFile());
                } catch (final IOException e) {
                    JOptionPane.showMessageDialog(this,
                                    "Can't be save!");
                }
            } 
        });
        myButtonCloseImage.addActionListener(theEvent -> {
            myImageLabel.setIcon(null);
            setMinimumSize(myMinFrameSize);   
            pack();
            toggleButtons();
        });
    }
}