# Drawings and shapes

A drawing is a specialization of Paragraph. Therefore all members of paragraph are members of drawing as well and you can add it to a section, header, footer, area and table cell.

## Drawing

A drawing is a canvas on which shapes are placed at exact locations. You access the collection of shapes through the Shapes property which is of type ShapeCollection. You compose a drawing by adding shapes to this collection.

The following code snippet shows how to add a drawing to a section and how to add shapes to a drawing.

```
Document doc = new Document();
Section section = new Section();
doc.Sections.Add( section );

// add a new drawing of size 200 x 200 pt
Drawing drawing = new Drawing( 200, 200 );
section.Paragraphs.Add( drawing );

// add a pie shape with a red outline and blue interior
PieShape pie = new PieShape();
pie.Pen = new Pen( System.Drawing.Color.Red, 2 );
pie.Brush = new SolidBrush( System.Drawing.Color.Blue );
drawing.Shapes.Add( pie );
```

Code sample: Add a drawing in C#

## Shape

All shapes inherit directly or indirectly from Shape. It has members that control positioning and docking. Docking is explained in more detail in Section Docking. A shape is positioned using the X and Y properties. X and Y are interpreted in the coordinate space of the container. The container is either a ShapeCollection or a Drawing.

The figure below shows the partial shape class hierarchy. The sub hierarchies of the abstract classes BarcodeShape, PathShape and FieldShape are discussed later.

![partial-shape-class-hierarchy](/guide/tallpdf/media/partial-shape-class-hierarchy.png)

Note that because ShapeCollection is a shape as well, it is possible to build a hierarchy of shapes. Especially interesting is that you can inherit a class called, let's say, ChartShape from ShapeCollection. This new class will use other shapes to build a chart and expose convenient properties that are specific to a chart. We call such a specialized shape a Custom Shape. This mechanism allows you to extend TallPDF.NET and build your own custom shape library.

## ContentShape

ContentShape is an abstract base class that adds the properties in Table _Public properties specific to ContentShape_ to specify rotation.

### Public properties specific to ContentShape
&nbsp;<table><tr><th>
Public properties</th><th></th></tr><tr><td>
Rotation</td><td>
Amount of clockwise rotation in degrees.</td></tr><tr><td>
RotationOffsetX,RotationOffsetY</td><td>
The horizontal and vertical offset of the rotation center relative to Shape.X and Shape.Y. These let you set the point to rotate around.</td></tr></table>&nbsp;
This class organization reflects that the classes LinkShape, NoteShape and FieldShape cannot be rotated since they inherit directly from Shape.

## ImageShape

An image shape allows you to add a raster image like a BMP or JPG bitmap. It is possible to set the width and rotate the image around its lower-left corner. By default the aspect ratio of the image is kept. In this case the Height property returns the image height as calculated from the width and the aspect ratio of the original image. If you set KeepAspectRatio to false, you can specify the height explicitly and the image will in general appear distorted.

## TextShape

TextShape lets you draw a single line of text. The X and Y properties are inherited from Shape and correspond to the lower-left corner. It is possible to set the color, font and font size of the text. Furthermore, you can rotate the text since it inherits from ContentShape. Finally, it is also possible to retrieve the extend of the text through the read-only properties Width and Height. You should access these properties after setting Font, FontSize and Text.

The following table lists the public properties of TextShape.

### Public properties specific to TextShape
&nbsp;<table><tr><th>
Public properties</th><th></th></tr><tr><td>
Text</td><td>
The text that is displayed.</td></tr><tr><td>
Width,Height</td><td>
Read-only properties that reflect the size of the text with the current settings.</td></tr><tr><td>
TextColor, Font, FontSize, Italic, Bold</td><td>
Appearance properties.</td></tr><tr><td>
Direction</td><td>
Read direction (left-to-right or right-to-left).</td></tr><tr><td>
BoundingBox</td><td>
The bounding box after rotation is applied.</td></tr></table>

## MultilineTextShape

MultilineTextShape lets you draw multiline text with mixed formatting. Just like TextParagraph it has a property Fragments. Each fragment has properties that specify both content and appearance. All fragments will be displayed concatenated and broken across lines as needed.

As opposed to the TextShape you can set the Width. This determines where lines wrap. In addition you can set the Height property but this is only useful if the multiline text shape is auto-sized. This is the case if and only if there is exactly one fragment that has a zero font size.

As opposed to most other shapes, the inherited X and Y properties coincide with the top-left corner. The property "MeasuredHeight" returns the height after all content has been rendered using the specified formatting properties and width.

## SimpleXhtmlShape

SimpleXhtmlShape specialized MultilineTextShape. Basically, you can pass XHTML data which is then transformed into the Fragments collection of the base class.

## PageShape

PageShape allows you to use a page from an existing PDF document on as a drawing primitive. The shape will retain all original data so the page can be sized without loosing any detail.

To include the page at original size in the document:
&nbsp;<ul><li>
Create a Section with the the same page size as the original Page.</li><li>
Set all margins to zero.</li><li>
Add a Drawing paragraph to the section and make it span the entire page.</li><li>
Add the PageShape to the Drawing and make it span the entire Drawing.</li></ul>&nbsp;
Ofcourse, you can vary the above steps to achieve any imposition scenario.

## ShapeCollection

There is a special shape called ShapeCollection which is a group of shape objects. Because a ShapeCollection is a Shape, it allows nesting of shapes to any depth. The following table shows the properties that are specific to ShapeCollection.

### Public properties specific to ShapeCollection
&nbsp;<table><tr><th>
Public properties (typical collection members omitted)</th><th></th></tr><tr><td>
Width,Height</td><td>
Width and Height of the shape collection with respect to the parent coordinate space. This parent is either a ShapeCollection or a Canvas.</td></tr><tr><td>
VirtualWidth,VirtualHeight</td><td>
These are the width and height of the shape collection as seen by the child shapes. If these are different than Width and Height, a scaling is applied. VirtualWidth and VirtualHeight decouple the dimensions of the child shapes from the actual size of the parent shape collection.</td></tr><tr><td>
Clip</td><td>
Clip the child shapes to the Width and Height.</td></tr></table>&nbsp;
Each shapes collection corresponds to a rectangle. This rectangle has its lower left corner at (X,Y) and its upper-right corner at (X+Width,Y+Height). This rectangle clips the child shapes if an only if ShapeCollection.Clip is set.

Each ShapeCollection collection, including the top-most, introduces a new coordinate system for all child shapes. The origin of this new coordinate system coincides with the lower-left corner of the shapes collection. The upper-right right corner of the shapes collection is seen by the child shapes as (VirtualWidth,VirtualHeight).


The top-most ShapeCollection collection inherits the Width and Height of the containing Drawing class. Setting the Width and Height of the top-most ShapeCollection collection has no effect.

![nesting-of-shapes](/guide/tallpdf/media/nesting-of-shapes.png)

## Pens and Brushes

Before we discuss the PathShape and its derived classes, you should understand pens and brushes. A pen is used to draw a line or curve. A brush is used to fill areas. You can attach a pen and a brush to a PathShape. So if you add a rectangle shape to a drawing, the four lines are drawn using a pen and the area inside is filled using a brush. The same holds for circles, ellipses and pies.



#### Pen

A pen is defined by a width, color, pattern and the cap and join styles. Width and color are obvious. The pattern is an array of integers that specifiy alternating lengths of ink followed by no-ink. The array is applied repeatedly to draw the full length of the line to which the pen is applied. You can also specifiy at which position in the array to start drawing.

![the-pattern-of-a-pen-is-defined-by-a-phase-and-a-dashes-array](/guide/tallpdf/media/the-pattern-of-a-pen-is-defined-by-a-phase-and-a-dashes-array.png)

The cap and join styles define how line ends are connected and how open line ends are drawn, respectively.


#### Brush

Brush is an abstract base class. The following concrete implementations are available:
&nbsp;<ul><li>
SolidBrush fills the area with a single color. This brush has a single property Color.</li><li>
GradientBrush is an abstract class for gradient filled areas with 2 colors:
&nbsp;<ul><li>
AxialGradientBrush


A brush that paints a gradient fill that changes from one color to another along a straight line.</li><li id="SubItem2">
RadialGradientBrush


A brush that paints a gradient fill that changes from one color to another between an inner and an outer circle.</li></ul></li></ul>

## PathShape

PathShape allows you to build an arbitrary open or closed shape consisting of lines and bezier curves. PathShape has a property PathShapeCollection to which you can add LineShape and BezierShape objects. The Shape.X and Shape.Y properties of the segments are ignored. Instead, the end point of the previous segment is the start point of the next. The start point of the first segment is the position of the containing PathShape. PathShape uses both a pen and a brush.


The figure below shows the PathShape class hierarchy.

![pathshape-class-hierarchy](/guide/tallpdf/media/pathshape-class-hierarchy.png)

#### LineShape

LineShape is a straight line. It starts at (Shape.X,Shape.Y) and it ends at LineShape.X1,LineShape.Y1). LineShape uses a pen but does not use a brush.



#### BezierShape

A bezier shape has a starting point (Shape.X,Shape.Y) and an end point (BezierShape.X3,BezierShape.Y3). The start and end point are controled by (BezierShape.X1,BezierShape.Y1) and (BezierShape.X2,BezierShape.Y2), respectively. BezierShape uses a pen but does not use a brush.



#### ArcShape

An arc shape is a segment of an ellipse outline. (Shape.X,Shape.Y) is the center point of this ellipse. ArcShape.Start and ArcShape.Sweep mark the starting and sweeping angle. Angle 0 corresponds to the right-most point. The sweeping angle is counter-clockwise. 360 corresponds to a full ellipse. The ellipse has a horizontal radius, Rx, and a vertical radius, Ry. ArcShape uses a pen but does not use a brush.



#### RectangleShape

Draws a rectangle with a lower-left corner at (Shape.X,Shape.Y). The width and height are set using the RectangleShape properties Width and Height. RectangleShape uses both a pen and a brush.



#### PieShape

PieShape specializes ArcShape: a pie is formed by drawing two straight lines from the center to the start and end point of the arc. PieShape uses both a pen and a brush.



#### EllipseShape

EllipseShape draws an ellipse with its center at (Shape.X,Shape.Y) and horizontal and vertical radi of EllipseShape.Rx and EllipseShape.Ry, respectively. EllipseShape uses both a pen and a brush.



## Field Shapes

Field shapes are used to add PDF forms fields to the generated document. The field shapes can be combined with the 'normal' drawing shapes to create complete forms. For example the TextShape can be used to add the caption for the forms' field shapes, and rectangles can be used to draw groups of check boxes, etc. FieldShape is the abstract base class (derived from Shape) all other field shapes. The following paragraphs discuss the specific field shapes in turn.



#### TextFieldShape

Generates a single line text entry field. By setting the 'Password' property to true, the value of the field will be hidden.



#### RadioButtonFieldShape

Creates a radio button. In a radio button group (identified by the FullName) there can be only one selected radio button.



#### CheckBoxFieldShape

Creates a checkbox, which can be either selected or not.



#### ListBoxFieldShape

Creates a list, of which a item can be selected.



#### DropDownListFieldShape

Creates a drop-down box from which an item can be selected.



#### PushButtonFieldShape

Creates a button which allows to send the form content to a server, reset the form content or perform another action. When sending the form to a server several submit formats can be selected such as HTTP (get/put), FDF, PDF and XFDF.



#### SignatureFieldShape

This shape adds an unsigned digital signature field to the generated document.



## BarcodeShape

BarcodeShape is the abstract base of a number of barcode shapes. The class hierarchy is shown in the figure below.

![barcodeshape-class-hierarchy](/guide/tallpdf/media/barcodeshape-class-hierarchy.png)

## Docking

Sometimes you do not want to or you cannot specify exact positions. Instead you want shapes to be stacked or docked in some direction. The top-level class Shape has a property Dock of type DockStyle. The default value is DockStyle.None meaning that the shape is positioned at exact coordinates. Setting the Dock property to any of the other values turns on docking behavior that is similar to how Winforms controls can be docked.


