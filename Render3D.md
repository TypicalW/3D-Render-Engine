/**/Making a GUI Wrapper with 2 sliders vertical and horizontal to rotate the rendered image**



\[ public static void main(String\[] args) {

&nbsp;       JFrame frame = new JFrame();

&nbsp;       Container pane = frame.getContentPane();

&nbsp;       pane.setLayout(new BorderLayout());



&nbsp;       // slider to control horizontal rotation

&nbsp;       JSlider headingSlider = new JSlider(0, 360, 180);

&nbsp;       pane.add(headingSlider, BorderLayout.SOUTH);



&nbsp;       // slider to control vertical rotation

&nbsp;       JSlider pitchSlider = new JSlider(SwingConstants.VERTICAL, -90, 90, 0);

&nbsp;       pane.add(pitchSlider, BorderLayout.EAST);



&nbsp;       // panel to display render results

&nbsp;       JPanel renderPanel = new JPanel() {

&nbsp;               public void paintComponent(Graphics g) {

&nbsp;                   Graphics2D g2 = (Graphics2D) g;

&nbsp;                   g2.setColor(Color.BLACK);

&nbsp;                   g2.fillRect(0, 0, getWidth(), getHeight());



&nbsp;                   // rendering magic will happen here

&nbsp;               }

&nbsp;           };

&nbsp;       pane.add(renderPanel, BorderLayout.CENTER);



&nbsp;       frame.setSize(400, 400);

&nbsp;       frame.setVisible(true);

&nbsp;   }

} ]



**// All the Imported Libraries and their work**





\[import java.util.List;

import java.util.ArrayList;

import java.awt.\*;

import java.awt.geom.\*;

import java.awt.image.BufferedImage;

import javax.swing.\*;]



java.util.List is an interface in Java's java.util package, Repopresents an ordered collection (Dynamically growing array)

java.util.ArrayList is a concrete implementation of List, Backed by a dynamic array

java.awt.\* -> awt == Abstract window Toolkit, Java's original GUI and graphics library

importing all classes Graphics==basic drawing context, Point==2D point with x,y coordinates.

Swing builds on top of AWT.

java.awt.\*.geom.\*; geom -> contains geometric shapes and 2D math utilities. Providdes more precise control over graphics than basic Graphics from awt.

java.awt.image.BufferedImage represents an image stored in memory, lets you draw graphics offscreen, manipulate pixels or save as a file, Useful for double-buffering to prevent flickering in animations.

Usage:: optionally can use a BufferedImage to draw the 3D frame off screen, then render it to JPanel for smooth animation.

javax.swing.\* is modern Java GUI Toolkit built on top of AWT, imports JFrame,JPanel,Timer ; It is lighter and more Flexible than AWT





**//Understanding Code-Sections line by line**



JFrame frame = new JFrame();

frame.setDefaultCloseOperation(JFrame.EXIT\_ON\_CLOSE)







-> its the main application window where the 3D scene will be displayed.

JFrame is a class in the javax.swing that represents a window on screen, part of Swing lets you create GUI.

frame is the main window, setDefaultCloseOperation tells Java what to do upon clicking on the windows X button, here terminating the whole application when the window closes.



Container pane = frame.getContentPane();

Container is a component that can hold other components, acts as a box component for other UI Element, JFrame's content pane is JPanel, JScrollPane.



pane.setLayOut(new BorderLayout());

its a Layout Manager for how components are arranged inside a container



pane.add(render,Panel, BorderLayout.CENTER);

Adds  your 3D render panel to the center region of the window, automatically resized when the window is resized.



//**Adding Horizontal and Vertical Slides**



// Horizontal Rotation Control using slider

&nbsp;       JSlider headingSlider = new JSlider(0, 360, 180);

&nbsp;       pane.add(headingSlider, BorderLayout.SOUTH);



// Vertical Rotation Control using slider

&nbsp;       JSlider pitchSlider = new JSlider(SwingConstants.VERTICAL,-90,90,0);

&nbsp;       pane.add(pitchSlider, BorderLayout.EAST);





JSlider is a Swing component lets user select a numeric value by moving a slider, can be used to control things dynamically at runtime (here: rotation angles);

{new Jslider(min,max,value)}

and pane.add() adds it to the direction defined.



Here 2 Sliders are created and added to the Container pan, which are horizontal and vertical.





// **Render Result Panel which displays render results;**

&nbsp;JPanel renderPanel = new JPanel() {

&nbsp;               public void paintComponent(Graphics g) {

&nbsp;                   Graphics2D g2 = (Graphics2D) g;

&nbsp;                   g2.setColor(Color.BLACK);

&nbsp;                   g2.fillRect(0, 0, getWidth(), getHeight());



&nbsp;                   // rendering magic will happen here

&nbsp;               }

&nbsp;           };

JPanel {name} = new JPanel(){...} creates an anonymous subclass of JPanel, by subclassing it you can customize its behaviour, in this case to draw 3D Graphics. {...} defines the new class body for this subclass without giving it a separate name.

Note: An anonymous subclass in Java is an inner class that has no name and is both declared and instantiated in a single expression, one-off object with slight modigication, like overriding a method without writing a whole new java file for it.

anonymous subclass extends class it uses.

It has no name,is single use, concise with self contained logic, cannot be static, cannot have a constructor.

Lambda Expressions have become more popular and even more concise alternative for anonymous subclasses that implement a function interface.



Here::

public void paintComponent(Graphics g){...}

overriding the paintComponent method. paintComponent is an important method for custom drawing in Java swing, whenever the window needs to be repainted, Swing calls this method for your component, by overriding it, telling java when its time to draw this panel and to do custom drawing instructions instead.



Inside the Method: g2.setColor(Color.BLACK);



g2.fillRect(0, 0, getWidth(), getHeight());



which are the custom drawing instructions inside the anonymous subclass.



///*Flow in this section of Code*

*Anonymous subclass is created in JPanel(){...}*

*paintComponent(g) is called by Swing automatically whenever renderPanel.repaint(), window is resized or uncovered.*

*Inside paintComponent, Sets color to black , clears the panel, converts vertices from 3D ->2D, draws edges connecting vertices, cube updates smoothly on screen*



//**Essential Model Classes ,Vertices and Triangles;**

\[class Vertex {

&nbsp;   double x;

&nbsp;   double y;

&nbsp;   double z;

&nbsp;   Vertex(double x, double y, double z) {

&nbsp;       this.x = x;

&nbsp;       this.y = y;

&nbsp;       this.z = z;

&nbsp;   }

}]

Vertices is a structure to store out 3 coordinates (X,Y,Z)



Triangle binds together 3 vertices and stores its color.



\[lass Triangle {

&nbsp;   Vertex v1;

&nbsp;   Vertex v2;

&nbsp;   Vertex v3;

&nbsp;   Color color;

&nbsp;   Triangle(Vertex v1, Vertex v2, Vertex v3, Color color) {

&nbsp;       this.v1 = v1;

&nbsp;       this.v2 = v2;

&nbsp;       this.v3 = v3;

&nbsp;       this.color = color;

&nbsp;   }

}]



//**Creates 4 triangles and adding them to a list to display a tetrahedron**

*Here Z axis means towards the user;*

List tris = new ArrayList<>();

tris.add(new Triangle(new Vertex(100, 100, 100),

&nbsp;                     new Vertex(-100, -100, 100),

&nbsp;                     new Vertex(-100, 100, -100),

&nbsp;                     Color.WHITE));

tris.add(new Triangle(new Vertex(100, 100, 100),

&nbsp;                     new Vertex(-100, -100, 100),

&nbsp;                     new Vertex(100, -100, -100),

&nbsp;                     Color.RED));

tris.add(new Triangle(new Vertex(-100, 100, -100),

&nbsp;                     new Vertex(100, -100, -100),

&nbsp;                     new Vertex(100, 100, 100),

&nbsp;                     Color.GREEN));

tris.add(new Triangle(new Vertex(-100, 100, -100),

&nbsp;                     new Vertex(100, -100, -100),

&nbsp;                     new Vertex(-100, -100, 100),

&nbsp;                     Color.BLUE));

Resulting shape is centered at (0,0,0) which is convienient as we rotate around the origin later.



//WireFrame Rendering 

Wireframe rendering is when you render the edges of your 3D object,

Instead of filling solid surfaces, you draw only the lines connecting vertices.

Advantage: Much simpler and faster, especially for learning.

//Orthographic Projection

3D graphics, projection = converting 3D coordinates (x,y,z) → 2D screen coordinates (x,y).

Orthographic projection means:

Ignore depth (z-coordinate).

Objects don’t shrink when they’re further away (no perspective).



\[g2.translate(getWidth() / 2, getHeight() / 2);

g2.setColor(Color.WHITE);

for (Triangle t : tris) {

&nbsp;   Path2D path = new Path2D.Double();

&nbsp;   path.moveTo(t.v1.x, t.v1.y);

&nbsp;   path.lineTo(t.v2.x, t.v2.y);

&nbsp;   path.lineTo(t.v3.x, t.v3.y);

&nbsp;   path.closePath();

&nbsp;   g2.draw(path);

}]



g2.translate(getWidth() / 2, getHeight() / 2);



Moves the origin to center of the panel instead of top left corner

set drawing color to white against already defined black background.



for(Triangle t : tris){...}

iterates over triangles 

























