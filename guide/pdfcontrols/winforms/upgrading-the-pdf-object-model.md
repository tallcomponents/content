# Upgrading the PDF Object Model (Professional Only)

Most compile errors in the PDF object model will be easy to fix because types or members have been renamed or replaced by similar members. Below, we will discuss all possible compile errors.



## PageShape

The properties PageShape.SkewX and PageShape.SkewY have been removed. To skew the PageShape, you will have to use the new inherited ContentShape.Transform property. When compiling your PDFReaderControls.NET 1 code you may receive the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.PageShape' does not contain a definition for 'SkewX' and no extension method 'SkewX' accepting a first argument of type 'TallComponents.PDF.Shapes.PageShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.PageShape' does not contain a definition for 'SkewY' and no extension method 'SkewY' accepting a first argument of type 'TallComponents.PDF.Shapes.PageShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
PageShape pageShape = new PageShape(sourcePage, 100, 100, 300, 300, true);
pageShape.SkewX = 20;
pageShape.SkewY = -30;

As follows:

PageShape pageShape = new PageShape(sourcePage, 0, 0, 300, 300, true);
TransformCollection transforms = new TransformCollection();
transforms.Add(new SkewTransform(20, -30));
transforms.Add(new TranslateTransform(100, 100));
pageShape.Transform = transforms;
```


## ContentShape

The properties ContentShape.Rotation, ContentShape.RotationOffsetX and ContentShape.RotationOffsetY have been removed. To rotate a ContentShape, you will have to use the new ContentShape.Transform property. When compiling your PDFReaderControls.NET 1 code you may receive the following compile errors (instead of TextShape, the compile error may display another ContentShape specialization such as ImageShape, RectangleShape, etc.):
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.TextShape' does not contain a definition for 'Rotation' and no extension method 'Rotation' accepting a first argument of type 'TallComponents.PDF.Shapes.TextShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.TextShape' does not contain a definition for 'RotationOffsetX' and no extension method 'RotationOffsetX' accepting a first argument of type 'TallComponents.PDF.Shapes.TextShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.TextShape' does not contain a definition for 'RotationOffsetY' and no extension method 'RotationOffsetY' accepting a first argument of type 'TallComponents.PDF.Shapes.TextShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
TextShape text = new TextShape(100, 100, "Hello world", Font.HelveticaBold, 48);
text.Rotation = -45;
text.RotationOffsetX = 80;
text.RotationOffsetY = 20;

As follows:

TextShape text = new TextShape(
  0, 0, "Hello world", Font.HelveticaBold, 48);
TransformCollection transforms = new TransformCollection();
text.Transform = transforms;
transforms.Add(new RotateTransform(-45, 80, 20));
transforms.Add(new TranslateTransform(100, 100));
```


## Security

The property PasswordSecurity.Strong has been replaced by PasswordSecurity.EncryptionLevel. When compiling your PDFReaderControls.NET 1 code you may receive the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Security.PasswordSecurity' does not contain a definition for 'Strong' and no extension method 'Strong' accepting a first argument of type 'TallComponents.PDF.Security.PasswordSecurity' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
TallComponents.PDF.Security.PasswordSecurity security = 
  new TallComponents.PDF.Security.PasswordSecurity();
security.UserPassword = "tall";
security.Strong = true;

As follows:

TallComponents.PDF.Security.PasswordSecurity security = 
  new TallComponents.PDF.Security.PasswordSecurity();
security.UserPassword = "tall";
security.EncryptionLevel = EncryptionLevel.RC4_128bit;
```

Setting Security.Strong to false with PDFReaderControls.NET 1, corresponds to setting Security.EncryptionLevel to EncryptionLevel.RC4_40bit with PDFControls.NET 2. The default value of Security.EncryptionLevel is EncryptionLevel.AES_128bit.



## ValueField and specializations

The following changes have been made that are related to the default value of a ValueVield:
&nbsp;<ul><li>
ValueField.DefaultValueString has been renamed to DefaultValue.</li><li>
CheckBoxField.DefaultValue has been renamed to CheckBoxDefaultValue.</li><li>
DateTimeField.DefaultValue has been renamed to DateTimeDefaultValue</li><li>
DropDownListField.DefaultValue has been renamed to DropDownListDefaultValue</li><li>
ImageField.DefaultValue has been renamed to ImageDefaultValue</li><li>
NumericField.DefaultValue has been renamed to NumericDefaultValue</li><li>
RadioButtonField.DefaultValue has been renamed to RadioButtonDefaultValue</li></ul>&nbsp;
Due to this you may receive one of the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Forms.Fields.TextField' does not contain a definition for 'DefaultValueString' and no extension method 'DefaultValueString' accepting a first argument of type 'TallComponents.PDF.Forms.Fields.TextField' could be found (are you missing a using directive or an assembly reference?)</li><li>
Cannot implicitly convert type 'TallComponents.PDF.Forms.Fields.CheckState' to 'string'</li><li>
Cannot implicitly convert type 'System.DateTime' to 'string'</li><li>
Cannot implicitly convert type 'TallComponents.PDF.Forms.Fields.ListOption' to 'string'</li><li>
Cannot implicitly convert type 'System.Drawing.Bitmap' to 'string'</li><li>
Cannot implicitly convert type 'double' to 'string'</li><li>
Cannot implicitly convert type 'TallComponents.PDF.Forms.Fields.RadioButtonOption' to 'string'</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
string textName = "form1[0].#subform[0].TextField1[0]";
TextField text = document.Fields[textName] as TextField;
text.DefaultValueString = "tall";

string checkBoxName = "form1[0].#subform[0].CheckBox1[0]";
CheckBoxField checkBox = document.Fields[checkBoxName] as CheckBoxField;
checkBox.DefaultValue = CheckState.On;

string dateTimeName = "form1[0].#subform[0].DateTimeField1[0]";
DateTimeField dateTime = document.Fields[dateTimeName] as DateTimeField;
dateTime.DefaultValue = DateTime.Now;

string dropDownName = "form1[0].#subform[0].DropDownList1[0]";
DropDownListField dropDown = document.Fields[dropDownName] as DropDownListField;
dropDown.DefaultValue = dropDown.Options[0]; // select first item by default

string imageName = "form1[0].#subform[0].ImageField1[0]";
ImageField image = document.Fields[imageName] as ImageField;
image.DefaultValue = bitmap;

string numericName = "form1[0].#subform[0].NumericField1[0]";
NumericField numeric = document.Fields[numericName] as NumericField;
numeric.DefaultValue = 1.25;

string radioName = "form1[0].#subform[0].NumericField1[0]";
RadioButtonField radio = document.Fields[radioName] as RadioButtonField;
radio.DefaultValue = radio.Options[0]; // select first item by default

As follows:

string textName = "form1[0].#subform[0].TextField1[0]";
TextField text = document.Fields[textName] as TextField;
text.DefaultValue = "tall";

string checkBoxName = "form1[0].#subform[0].CheckBox1[0]";
CheckBoxField checkBox = document.Fields[checkBoxName] as CheckBoxField;
checkBox.CheckBoxDefaultValue = CheckState.On;

string dateTimeName = "form1[0].#subform[0].DateTimeField1[0]";
DateTimeField dateTime = document.Fields[dateTimeName] as DateTimeField;
dateTime.DateTimeDefaultValue = DateTime.Now;

string dropDownName = "form1[0].#subform[0].DropDownList1[0]";
DropDownListField dropDown = document.Fields[dropDownName] as DropDownListField;
dropDown.DropDownListDefaultValue = dropDown.Options[0]; // select first item

string imageName = "form1[0].#subform[0].ImageField1[0]";
ImageField image = document.Fields[imageName] as ImageField;
image.ImageDefaultValue = bitmap;

string numericName = "form1[0].#subform[0].NumericField1[0]";
NumericField numeric = document.Fields[numericName] as NumericField;
numeric.NumericDefaultValue = 1.25;

string radioName = "form1[0].#subform[0].NumericField1[0]";
RadioButtonField radio = document.Fields[radioName] as RadioButtonField;
radio.RadioButtonDefaultValue = radio.Options[0]; // select first item
```

Setting Security.Strong to false with PDFReaderControls.NET 1, corresponds to setting Security.EncryptionLevel to EncryptionLevel.RC4_40bit with PDFControls.NET 2. The default value of Security.EncryptionLevel is EncryptionLevel.AES_128bit.



## DashPattern.Phase

The type of DashPattern.Phase has changed from int to double. Most likely this will not cause a compile error because the setter will cast automatically from int to double. The getter however will not, so you may get the following compile error:
&nbsp;<ul><li>
Cannot implicitly convert type 'double' to 'int'. An explicit conversion exists (are you missing a cast?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
Pen pen = new Pen(RgbColor.Red, 2.0);
pen.Pattern.Phase = 3;
int phase = pen.Pattern.Phase;

As follows:

Pen pen = new Pen(RgbColor.Red, 2.0);
pen.Pattern.Phase = 3;
double phase = pen.Pattern.Phase;
```


## Fragment.Outline

Property Fragment.Outline has been removed. You should use Fragment.Pen instead. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.Fragment' does not contain a definition for 'Outline' and no extension method 'Outline' accepting a first argument of type 'TallComponents.PDF.Shapes.Fragment' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
Fragment fragment = new Fragment("Outline", Font.Helvetica, 36);
fragment.Outline = true;
fragment.TextColor = RgbColor.Red;

As follows:

Fragment fragment = new Fragment("Outline", Font.Helvetica, 36);
fragment.Brush = null; // no fill
fragment.Pen = new Pen(RgbColor.Red); // red outline
```


## Collection.Add

The return type of the Add method of all collection classes has been changed from int to void so that our collection classes are consistent with System.Collections.Generic.ICollection. If you store the returned index, this may result in the following compile error:
&nbsp;<ul><li>
Cannot implicitly convert type 'void' to 'int'</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
Document document = new Document();
Page page = new Page(PageSize.A4);
int pageIndex = document.Pages.Add(page);

As follows:

Document document = new Document();
Page page = new Page(PageSize.A4);
document.Pages.Add(page);
int pageIndex = document.Pages.IndexOf(page);
```


## Color operands

The type of the color operands of classes TallComponents.PDF.Colors.GrayColor, RgbColor and CmykColor has changed from byte to double. Most likely this will not cause a compile error because the setter will cast automatically from byte to double. The getter however will not, so you may get the following compile error:
&nbsp;<ul><li>
Cannot implicitly convert type 'double' to 'byte'. An explicit conversion exists (are you missing a cast?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
RgbColor rgbColor = RgbColor.Red;
byte red = rgbColor.R;
byte green = rgbColor.G;
byte blue = rgbColor.B;

byte gray = (byte)(0.2989 * red + 0.5870 * green + 0.1140 * blue);
GrayColor grayColor = new GrayColor(gray);

// not a very accurate conversion
byte cyan = (byte)(255 - red);
byte magenta = (byte)(255 - green);
byte yellow = (byte)(255 - blue);
byte black = Math.Min(Math.Min(cyan, magenta), yellow);
CmykColor cmykColor = new CmykColor(cyan, magenta, yellow, black);

As follows:

RgbColor rgbColor = RgbColor.Red;
double red = rgbColor.R;
double green = rgbColor.G;
double blue = rgbColor.B;

double gray = 0.2989 * red + 0.5870 * green + 0.1140 * blue;
GrayColor grayColor = new GrayColor(gray);

// not a very accurate conversion
double cyan = 255 - red;
double magenta = 255 - green;
double yellow = 255 - blue;
double black = Math.Min(Math.Min(cyan, magenta), yellow);
CmykColor cmykColor = new CmykColor(cyan, magenta, yellow, black);
```


## CmykColorHighPrecision

This class has been removed because the operands of CmykColor are now of type double. You may get the following compile error:
&nbsp;<ul><li>
The type or namespace name 'CmykColorHighPrecision' could not be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
CmykColorHighPrecision cmyk = 
  new CmykColorHighPrecision(110.0, 103.8, 76.4, 10.7);

As follows:

CmykColor cmyk = new CmykColor(110.0, 103.8, 76.4, 10.7);
```


## FreeHandShape

Properties FreeHandShape.Closed and FreeHandShape.Segments have been removed. You may get the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.FreeHandShape' does not contain a definition for 'Closed' and no extension method 'Closed' accepting a first argument of type 'TallComponents.PDF.Shapes.FreeHandShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.FreeHandShape' does not contain a definition for 'Segments' and no extension method 'Segments' accepting a first argument of type 'TallComponents.PDF.Shapes.FreeHandShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
You should rewrite the following PDFReaderControls.NET 1 code:


```
FreeHandShape freeHandShape = new FreeHandShape();
page.Overlay.Add(freeHandShape);

freeHandShape.X = 150;
freeHandShape.Y = 150;
freeHandShape.Closed = true;

freeHandShape.Pen = new Pen(GrayColor.Black);
freeHandShape.Brush = new SolidBrush(RgbColor.Red);

freeHandShape.Segments.Add(new FreeHandLineSegment(400, 150));
freeHandShape.Segments.Add(new FreeHandBezierSegment(
   250, 200, 300, 550, 400, 400));

As follows:

FreeHandShape freeHandShape = new FreeHandShape();
page.Overlay.Add(freeHandShape);

freeHandShape.Pen = new Pen(GrayColor.Black);
freeHandShape.Brush = new SolidBrush(RgbColor.Red);

FreeHandPath path = new FreeHandPath();
path.Closed = true;
freeHandShape.Paths.Add(path);

path.Segments.Add(new FreeHandStartSegment(150, 150));
path.Segments.Add(new FreeHandLineSegment(400, 150));
path.Segments.Add(new FreeHandBezierSegment(250, 200, 300, 550, 400, 400));
```

Note that you should now use the new class FreeHandPath combined with FreeHandShape.Paths. Also settings the X and Y properties of FreeHandShape has been replaced by the FreeHandStartSegment.



## FreeHandShape Clipping

Using PDFReaderControls.NET 1, adding a freehand clipping path was done by either adding a FreeHandShape to a ShapeCollection and setting the FreeHandShape.BehaveAsClippingPath to true or by adding segments to the ShapeCollection.ClipPath which is of type FreeHandSegmentCollection.


As of version 2, properties FreeHandShape.BehaveAsClippingPath and ShapeCollection.ClipPath have been removed. You should now instantiate a ClipShape and at it to a ShapeCollection.


You may get the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.FreeHandShape' does not contain a definition for 'BehaveAsClippingPath' and no extension method 'BehaveAsClippingPath' accepting a first argument of type 'TallComponents.PDF.Shapes.FreeHandShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ShapeCollection' does not contain a definition for 'ClipPath' and no extension method 'ClipPath' accepting a first argument of type 'TallComponents.PDF.Shapes.ShapeCollection' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
ImageShape image = new ImageShape(@"..\..\flowers.jpg");

Page page = new Page(image.Width, image.Height);
document.Pages.Add(page);

ShapeCollection shapes = new ShapeCollection(page.Width, page.Height);
page.Overlay.Add(shapes);

FreeHandShape freeHandShape = new FreeHandShape();
freeHandShape.BehaveAsClippingPath = true;
freeHandShape.X = 150;
freeHandShape.Y = 150;
freeHandShape.Closed = true;
freeHandShape.Segments.Add(
  new FreeHandLineSegment(400, 150));
freeHandShape.Segments.Add(
  new FreeHandBezierSegment(250, 200, 300, 550, 400, 400));

shapes.Add(freeHandShape);
shapes.Add(image);
Or the following equivalent PDFReaderControls.NET 1 code:
ImageShape image = new ImageShape(@"..\..\flowers.jpg");

Page page = new Page(image.Width, image.Height);
document.Pages.Add(page);

ShapeCollection shapes = new ShapeCollection(page.Width, page.Height);
page.Overlay.Add(shapes);

shapes.ClipPath.Add(new FreeHandStartSegment(150, 150));
shapes.ClipPath.Add(new FreeHandLineSegment(400, 150));
shapes.ClipPath.Add(new FreeHandBezierSegment(250, 200, 300, 550, 400, 400));

shapes.Add(image);

Should be rewritten as follows:

ImageShape image = new ImageShape(@"..\..\flowers.jpg");

Page page = new Page(image.Width, image.Height);
document.Pages.Add(page);

ShapeCollection shapes = new ShapeCollection(page.Width, page.Height);
page.Overlay.Add(shapes);

ClipShape clip = new ClipShape();
FreeHandPath path = new FreeHandPath();
clip.Paths.Add(path);

path.Segments.Add(new FreeHandStartSegment(150, 150));
path.Segments.Add(new FreeHandLineSegment(400, 150));         path.Segments.Add(new FreeHandBezierSegment(250, 200, 300, 550, 400, 400));

shapes.Add(clip);
shapes.Add(image);
```


## ArcShape

The following properties have been renamed:
&nbsp;<ul><li>
ArcShape.X1 has been renamed to StartX</li><li>
ArcShape.Y1 has been renamed to StartY</li><li>
ArcShape.X2 has been renamed to EndX</li><li>
ArcShape.Y2 has been renamed to EndY</li><li>
ArcShape.Rx has been renamed to RadiusX</li><li>
ArcShape.Ry has been renamed to RadiusY</li></ul>&nbsp;
You may receive the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'Rx' and no extension method 'Rx' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'Ry' and no extension method 'Ry' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'X1' and no extension method 'X1' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'Y1' and no extension method 'Y1' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'X2' and no extension method 'X2' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.ArcShape' does not contain a definition for 'Y2' and no extension method 'Y2' accepting a first argument of type 'TallComponents.PDF.Shapes.ArcShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
ArcShape arcShape = new ArcShape(
arcShape.Pen = new Pen(GrayColor.Black, 5);
arcShape.Brush = null;
arcShape.X = page.Width / 2;
arcShape.Y = page.Height / 2;
arcShape.Rx = 200;
arcShape.Ry = 100;
arcShape.Start = 170;
arcShape.Sweep = 200;

page.Overlay.Add(arcShape);

// add markers at start and end of arc
page.Overlay.Add(new EllipseShape(
  arcShape.X + arcShape.X1, arcShape.Y + arcShape.Y1,
  10, 10, null, new SolidBrush(RgbColor.Red)));
page.Overlay.Add(new EllipseShape(
  arcShape.X + arcShape.X2, arcShape.Y + arcShape.Y2,
  10, 10, null, new SolidBrush(RgbColor.Red)));
Should be rewritten as follows:
ArcShape arcShape = new ArcShape(page.Width / 2, page.Height / 2, 200, 100);
arcShape.Pen = new Pen(GrayColor.Black, 5);
arcShape.Brush = null;
arcShape.Start = 170;
arcShape.Sweep = 200;

page.Overlay.Add(arcShape);

// add markers at start and end of arc
page.Overlay.Add(new EllipseShape(
  arcShape.CenterX + arcShape.StartX, arcShape.CenterY + arcShape.StartY,
  10, 10, null, new SolidBrush(RgbColor.Red)));
page.Overlay.Add(new EllipseShape(
  arcShape.CenterX + arcShape.EndX, arcShape.CenterY + arcShape.EndY,
  10, 10, null, new SolidBrush(RgbColor.Red)));
```

Note that because PieShape inherits from ArcShape, the above applies to PieShape as well.



## BezierShape

The following properties have been renamed:
&nbsp;<ul><li>
BezierShape.X has been renamed to X0</li><li>
BezierShape.Y has been renamed to Y0</li></ul>&nbsp;
You may receive the following compile errors:
&nbsp;<ul><li>
Property or indexer 'TallComponents.PDF.Shapes.Shape.X' cannot be assigned to -- it is read only</li><li>
Property or indexer 'TallComponents.PDF.Shapes.Shape.Y' cannot be assigned to -- it is read only</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
BezierShape bezier = new BezierShape();
bezier.X = 400;
bezier.Y = 150;
bezier.X1 = 250;
bezier.Y1 = 200;
bezier.X2 = 300;
bezier.Y2 = 550;
bezier.X3 = 400;
bezier.Y3 = 400;
Should be rewritten as follows:
BezierShape bezier = new BezierShape();
bezier.X0 = 400;
bezier.Y0 = 150;
bezier.X1 = 250;
bezier.Y1 = 200;
bezier.X2 = 300;
bezier.Y2 = 550;
bezier.X3 = 400;
bezier.Y3 = 400;
```


## EllipseShape

The following properties have been renamed:
&nbsp;<ul><li>
EllipseShape.Rx has been renamed to RadiusX</li><li>
EllispeShape.Ry has been renamed to RadiusY</li></ul>&nbsp;
You may receive the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.EllipseShape' does not contain a definition for 'Rx' and no extension method 'Rx' accepting a first argument of type 'TallComponents.PDF.Shapes.EllipseShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.EllipseShape' does not contain a definition for 'Ry' and no extension method 'Ry' accepting a first argument of type 'TallComponents.PDF.Shapes.EllipseShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
EllipseShape ellipse = new EllipseShape();
ellipse.X = page.Width / 2;
ellipse.Y = page.Height / 2;
ellipse.Rx = 150;
ellipse.Ry = 100;

Should be rewritten as follows:

EllipseShape ellipse = new EllipseShape(
  page.Width / 2, page.Height / 2, 150, 100);
```


## LineShape

The following properties have been renamed:
&nbsp;<ul><li>
LineShape.X has been renamed to StartX</li><li>
LineShape.Y has been renamed to StartY</li><li>
LineShape.X1 has been renamed to EndX</li><li>
LineShape.Y1 has been renamed to EndY</li></ul>&nbsp;
You may receive the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.LineShape' does not contain a definition for 'X1' and no extension method 'X1' accepting a first argument of type 'TallComponents.PDF.Shapes.LineShape' could be found (are you missing a using directive or an assembly reference?)</li><li>
'TallComponents.PDF.Shapes.LineShape' does not contain a definition for 'Y1' and no extension method 'Y1' accepting a first argument of type 'TallComponents.PDF.Shapes.LineShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
LineShape line = new LineShape();
line.X = 100;
line.Y = 100;
line.X1 = page.Width - 100;
line.Y1 = page.Height - 100;

Should be rewritten as follows:

LineShape line = new LineShape(100, 100, page.Width - 100, page.Height - 100);
```


## TextShape

Property TextShape.TextColor has been removed and properies TextShape.Pen and TextShape.Brush have been added. You may get the following compile errors:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.TextShape' does not contain a definition for 'TextColor' and no extension method 'TextColor' accepting a first argument of type 'TallComponents.PDF.Shapes.TextShape' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
TextShape text = new TextShape(
  72, page.Height - 72, "Hello world", Font.HelveticaBold, 24);
text.TextColor = RgbColor.Blue;

Should be rewritten as follows:

TextShape text = new TextShape(
  72, page.Height - 72, "Hello world", Font.HelveticaBold, 24);
text.Pen = null; // no outline
text.Brush = new SolidBrush(RgbColor.Blue);
```

The TextShape constructor that takes a Color argument to set the TextColor property has been removed. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.TextShape' does not contain a constructor that takes '6' arguments</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
TextShape text = new TextShape(
  72, page.Height - 144, "Hello world", Font.HelveticaBold, 24, RgbColor.Red);
page.Overlay.Add(text);
Should be rewritten as follows:
TextShape text = new TextShape(
  72, page.Height - 144, "Hello world", Font.HelveticaBold, 24);
page.Overlay.Add(text);
text.Pen = null; // no outline
text.Brush = new SolidBrush(RgbColor.Red);
```


## SeparationColor

Class TallComponents.PDF.Colors.SpotColor has been renamed into SeperationColor. You may get the following compile error:
&nbsp;<ul><li>
The type or namespace name 'SpotColor' could not be found (are you missing a using directive or an assembly reference?</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
SpotColor spotColor = new SpotColor("spot1");

Should be rewritten as follows:

SeperationColor spotColor = new SeperationColor("spot1");
```


## TransparentColor

Class TallComponents.PDF.Colors.TransparentColor has been removed. You should assign null/Nothing instead. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Colors.Color' does not contain a definition for 'Transparent'</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
Widget widget = new Widget();
widget.BackgroundColor = Color.Transparent;

Should be rewritten as follows:

Widget widget = new Widget();
widget.BackgroundColor = null;
```


## From Color to DeviceColor

The type of the following properties has been changed from Color to DeviceColor: Annotation.BorderColor, Widget.BackgroundColor and Widget.TextColor. The reason is that the PDF specification only allows a DeviceColor to be used in these cases. You may get the following compile error:
&nbsp;<ul><li>
Cannot implicitly convert type 'TallComponents.PDF.Colors.Color' to 'TallComponents.PDF.Colors.DeviceColor'. An explicit conversion exists (are you missing a cast?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
Color borderColor = RgbColor.Red;
Color backColor = RgbColor.Green;
Color textColor = RgbColor.Blue;
Widget widget = new Widget();
widget.BorderColor = borderColor;
widget.BackgroundColor = backColor;
widget.TextColor = textColor;

Should be rewritten as follows:

DeviceColor borderColor = RgbColor.Red;
DeviceColor backColor = RgbColor.Green;
DeviceColor textColor = RgbColor.Blue;
Widget widget = new Widget();
widget.BorderColor = borderColor;
widget.BackgroundColor = backColor;
widget.TextColor = textColor;
```


## AnnotationShape

The AnnotationShape constructor that takes two doubles has been removed. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Shapes.Annotations.AnnotationShape' does not contain a constructor that takes '2' arguments</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
Note note = new Note();
note.Width = note.Height = 40;
page.Markups.Add(note);
AnnotationShape annotShape = new AnnotationShape(50, 50);
annotShape.Annotation = note;
note.Text = "An AnnotationShape put me here!";
Should be rewritten as follows:
Note note = new Note();
note.Width = note.Height = 40;
page.Markups.Add(note);
AnnotationShape annotShape = new AnnotationShape();
annotShape.Annotation = note;
note.Left = 50;
note.Bottom = 50;
note.Text = "An AnnotationShape put me here!";
```


## ShapeCollection

The 2.0 ShapeCollection constructor that takes a System.Drawing.Drawing2D.Matrix has been changed into a constructor that accepts a TallComponents.PDF.Transforms.Transform instead. You may get the following compile errors:
&nbsp;<ul><li>
The best overloaded method match for 'TallComponents.PDF.Shapes.ShapeCollection.ShapeCollection(double, double, TallComponents.PDF.Transforms.Transform)' has some invalid arguments</li><li>
Argument '3': cannot convert from 'System.Drawing.Drawing2D.Matrix' to 'TallComponents.PDF.Transforms.Transform'</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
System.Drawing.Drawing2D.Matrix matrix = new System.Drawing.Drawing2D.Matrix();
matrix.Scale(0.5f, 0.5f);
ShapeCollection shapes = new ShapeCollection(page.Width, page.Height, matrix);
page.Overlay.Add(shapes);

Should be rewritten as follows:

System.Drawing.Drawing2D.Matrix matrix = new System.Drawing.Drawing2D.Matrix();
matrix.Scale(0.5f, 0.5f);
Transform transform = new MatrixTransform(matrix);
ShapeCollection shapes = new ShapeCollection(
  page.Width, page.Height, transform);
page.Overlay.Add(shapes);
```


## Opacity

The type of properties TallComponents.PDF.Annotations.Markups.Markup.Opacity and TallComponents.PDF.Shapes.ContentShape.Opacity have changed from int to double. Since an int will be casted to a double automatically, this will not likely break your existing code. However, if you are using the getter, then you may get the following compile errors:
&nbsp;<ul><li>
Cannot implicitly convert type 'double' to 'int'. An explicit conversion exists (are you missing a cast?)</li></ul>&nbsp;
```
int opacitiy = note.Opacity;

Should be rewritten as follows:

double opacitiy = note.Opacity;

Or:

int opacitiy = (int) note.Opacity;
```


## DestinationCollection

Claas TallComponents.PDF.Navigation.DestinationCollection has been renamed to InternalDestinationCollection. An instance of this class is returned by property Document.NamedDestinations. You may get the following compile error:
&nbsp;<ul><li>
The type or namespace name 'DestinationCollection' could not be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
Document document = new Document();
DestinationCollection destinations = document.NamedDestinations;

Should be rewritten as follows:

Document document = new Document();
InternalDestinationCollection destinations = document.NamedDestinations;
```


## Font.CalculateLength

Method Font.CalculateLength has been replaced by Font.CalculateWidth. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.Fonts.Font' does not contain a definition for 'CalculateLength' and no extension method 'CalculateLength' accepting a first argument of type 'TallComponents.PDF.Fonts.Font' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
Font font = Font.Helvetica;
int fontSize = 12; // 12pt
string text = "Measure me!";
double width = fontSize * font.CalculateLength(text);
Console.WriteLine("{0} measures {1:f1} points", text, width);

Should be rewritten as follows:

Font font = Font.Helvetica;
int fontSize = 12; // 12pt
string text = "Measure me!";
double width = font.CalculateWidth(text, fontSize);
Console.WriteLine("{0} measures {1:f1} points", text, width);
```


## PageIndexDestination

TallComponents.PDF.Navigation.PageIndexDestination has been removed. You should now use InternalDestination instead. You may get the following compile error:
&nbsp;<ul><li>
The type or namespace name 'PageIndexDestination' could not be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
PageIndexDestination destination = new PageIndexDestination(3);
GoToAction gotoAction = new GoToAction();
gotoAction.Destination = destination;
link.MouseUpActions.Add(gotoAction);

Should be rewritten as follows:

InternalDestination destination = new InternalDestination();
destination.Page = document.Pages[3];
GoToAction gotoAction = new GoToAction();
gotoAction.Destination = destination;
link.MouseUpActions.Add(gotoAction);
```


## PageIndexDestination

TallComponents.PDF.Navigation.PageIndexDestination has been removed. You should now use InternalDestination instead. You may get the following compile error:
&nbsp;<ul><li>
The type or namespace name 'PageIndexDestination' could not be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
PageIndexDestination destination = new PageIndexDestination(3);
GoToAction gotoAction = new GoToAction();
gotoAction.Destination = destination;
link.MouseUpActions.Add(gotoAction);

Should be rewritten as follows:

InternalDestination destination = new InternalDestination();
destination.Page = document.Pages[3];
GoToAction gotoAction = new GoToAction();
gotoAction.Destination = destination;
link.MouseUpActions.Add(gotoAction);
```


## SimpleXhtmlShape

TallComponents.PDF.Shapes.SimpleXhtmlShape now derives directly from ContentShape. In version 1 it derived from MultilineTextShape. This means that all properties that were inherited from MultilineTextShape are no longer accessible.



## Glyph.Character

Property TallComponents.PDF.TextExtraction.Glyph.Character of type char has been replaced by Glyph.Characters of type char[]. This is because in case of a ligature, a single glyph may represent multiple characters. You may get the following compile error:
&nbsp;<ul><li>
'TallComponents.PDF.TextExtraction.Glyph' does not contain a definition for 'Character' and no extension method 'Character' accepting a first argument of type 'TallComponents.PDF.TextExtraction.Glyph' could be found (are you missing a using directive or an assembly reference?)</li></ul>&nbsp;
The following PDFReaderControls.NET 1 code:


```
GlyphCollection glyphs = page.Glyphs;
foreach (Glyph glyph in glyphs)
{
  char c = glyph.Character;
}

Should be rewritten as follows:

GlyphCollection glyphs = page.Glyphs;
foreach (Glyph glyph in glyphs)
{
  char c = glyph.Characters[0];
}
```


## TextMatch

Properties TallComponents.PDF.TextExtraction.TextMatch.FirstGlyph and Length have been removed because they are obsolete due to the Glyphs property that returns a GlyphCollection.


