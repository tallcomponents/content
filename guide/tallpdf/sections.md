# Sections

A document consists of one ore more sections. The content of a section is defined by one or more paragraphs. The paragraph are rendered into one or more pages. The section defines the settings for all these pages. These setting include headers and footers, page size and margins and columns. The figure below shows how sections relate to pages.

![a-section-renders-its-paragraphs-into-one-or-more-pages](/guide/tallpdf/media/a-section-renders-its-paragraphs-into-one-or-more-pages.png)

The following code snippets show how to create a new document and add sections to the document.

```
// create document
Document doc = new Document();

// add a section
Section section = new Section();
doc.Sections.Add(section);
```

Code sample: Create a new document and add a section in C#


```
' create a document
Dim doc As New Document

' add a section
Dim section As New Section
doc.Sections.Add(section)
```

Code sample: Create a new document and add a section in VB.NET



## Page Size and Margins

All pages that are rendered from the same section have the same size and margins. The size is set through the PageSize property of Section. PageSize offers standard page formats through static (or shared) properties like PageSize.Letter and PageSize.A4.


The margins of the pages can be set through the Margins properties of Section. The paragraphs will be flowed inside these margins. The top margin and the bottom margin define the space reserved for the header and footer, respectively.

![page-size-and-margins](/guide/tallpdf/media/page-size-and-margins.png)

The following code snippets show how to set the page size and the margins of a section.

```
Section section = document.Sections.Add();
section.PageSize = PageSize.A4;
section.Margin.Left = new Unit(1.25, UnitType.Inch);
section.Margin.Right = new Unit(1.25, UnitType.Inch);
section.Margin.Top = new Unit(1, UnitType.Inch);
section.Margin.Bottom = new Unit(1, UnitType.Inch);
```

Code sample: Set the page size and margins of a section in C#


```
Dim section As Section
section = doc.Sections.Add();
section.PageSize = PageSize.A4
section.Margin.Left = New Unit(1.25, UnitType.Inch)
section.Margin.Right = New Unit(1.25, UnitType.Inch)
section.Margin.Top = New Unit(1, UnitType.Inch)
section.Margin.Bottom = New Unit(1, UnitType.Inch)
```

Code sample: Set the page size and margins of a section in VB.NET



## Headers and Footers

The Section class has 4 properties that allow you to define headers and footers:
&nbsp;<ul><li>
EvenHeader: The content of this header will be repeated at the top of each even page.</li><li>
OddHeader: The content of this header will be repeated at the top of each odd page.</li><li>
EvenFooter: The content of this footer will be repeated at the bottom of each even page.</li><li>
OddFooter: The content of this footer will be repeated at the bottom of each odd page.</li></ul>&nbsp;
This is illustrated in the following figure:

![headers-and-footers](/guide/tallpdf/media/headers-and-footers.png)

The content of both the header and the footer is defined by a collection of paragraphs that will be flowed from top to bottom in the header and footer areas. A header renders from the very top of the page down until the top margin of the section as shown in Figure . Because normally you want to have some whitespace above the header content, you should set the Header.Margin.Top property as shown in the code sample below.

```
Document doc = new Document();

Section section = new Section();
doc.Sections.Add(section);

Header oddHeader = new Header();
oddHeader.TopMargin = new Unit(0.2, UnitType.Inch);
section.OddHeader = oddHeader;

TextParagraph textParagraph = new TextParagraph();
textParagraph.Fragments.Add(new Fragment("odd header"));
oddHeader.Paragraphs.Add(textParagraph);
```

Code sample: Add headers and footers in C#

## Columns

Associated with a section is an array of columns. These are accessed through the Section.Columns property. By default, a section has 1 column. You can specifiy the width of each column and the spacing between each column pair (if any). Column settings are made per section. Paragraph of a multi-column section flow from one column to the next. This is illustrated in the next figure.

![columns](/guide/tallpdf/media/columns.png)

The following code samples show how to add and configure columns.

```
Document doc = new Document();

Section section = doc.Sections.Add();

section.Columns.Count = 3;
section.Columns[0].Width = 200;
section.Columns[1].Width = 100;
section.Columns[2].Width = 100;
section.Columns[0].Spacing = 20; // spacing to the right of column 0
section.Columns[1].Spacing = 20; // spacing to the right of column 1
```

Code sample: Add columns in C#


```
Dim doc As New Document()

Dim section As New Section
doc.Sections.Add(section)

section.Columns.Count = 3
section.Columns(0).Width = 200
section.Columns(1).Width = 100
section.Columns(2).Width = 100
section.Columns(0).Spacing = 20 ' spacing to the right of column 0
section.Columns(1).Spacing = 20 ' spacing to the right of column 1
```

Code sample: Add columns in VB.NET



## Page Boxes

In PDF you can define different boxes per page. The most common box is the media box which simply represents the entire page. Probably it is the only box you are interested in. You set the media box implicitly by setting Section.PageSize. The following figure illustrates the other boxes.

![page-boxes](/guide/tallpdf/media/page-boxes.png)

#### Section.CropBox

The CropBox is the area that should be shown to PDF users and can be printed. It contains the desired 'normal' content of the page. Within the MediaBox, but outside the crop box, cutting marks could be present. Defaults to the media box values.

The following code samples show how to set the crop box.

```
Document doc = new Document();
Section section = doc.Sections.Add();
section.PageSize = new PageSize(612, 792);
section.CropBox = new Rectangle(20, 20, 572, 752);
```

Code sample: Set the crop box in C#

```
Dim section As New Section
doc.Sections.Add(section)
section.PageSize = New PageSize(612, 792)
section.CropBox = New Rectangle(20, 20, 572, 752)
```

Code sample: Set the crop box in VB.NET



#### Section.bleedbox

The bleed box defines the area outside the cropbox, which is 'blank' and intended for post-printing operations (cutting, folding...). This way cutting machines have some margin for error, without either loosing content (eg. footer: page number) or including production markers (cutting lines, color calibration areas).

## Areas

An area is a rectangular region on a page. The content of an area consists of a collection of paragraphs. An Area is a generalization of header and footer. In fact, classes Header and Footer inherit from Area.Each section has a collection of foreground areas and a collection of background areas. They appear respectively in front of and behind the main content. An area is repeated on each page that matches the properties of the area.


A background area is typically used to add a stationary for a page (company logo, graphical borders), or a watermark. A foreground area can be used for stamping content (Confidential, Draft, etc).


The following code sample adds a foreground area that displays ‘Confidential’ across each page. Drawing and shapes are discussed in detail in Chapter <a href="drawings-and-shapes">Drawings and Shapes</a>.


```
Document document = new Document();
Section section = document.Sections.Add();

// area spans the entire page
Area area = new Area(
   0, section.PageSize.Height,
   section.PageSize.Width, section.PageSize.Height);
section.ForegroundAreas.Add(area);

// drawing spans the entire area
Drawing drawing = new Drawing(area.Width, area.Height);
area.Paragraphs.Add(drawing);

// add text “Confidential”
TextShape textShape = new TextShape();
textShape.Text = "Confidential";
textShape.Font = Font.Courier;
textShape.FontSize = 72;
textShape.TextColor = RgbColor.Red;
textShape.Opacity = 128; // 50%
textShape.Rotation = -45; // clockwise
textShape.X = 130;
textShape.Y = 180;
drawing.Shapes.Add(textShape);

// add some dummy content
for (int i = 0; i < 100; i++)
{
   TextParagraph text = new TextParagraph();
   text.Fragments.Add(new Fragment("A background area is typically used to add a stationary for a page (company logo, graphical borders), or a watermark. A foreground area can be used for stamping content (Confidential, Draft, etc)."));
   text.SpacingAfter = 10;
   section.Paragraphs.Add(text);
}

// save PDF to disk
using (FileStream file = new FileStream(
   "out.pdf", FileMode.Create, FileAccess.Write))
{
   document.Write(file);
}
```

Code sample: Stamp ‘Confidential’ on each page using a foreground area in C#



#### CrossreferenceSection

CrossreferenceSection is a class that specializes Section. An instance of this class can be added to the Document.Sections collection just as a normal Section. The CrossreferenceSection lets you generated sections such as a ‘Table of Contents’, a ‘List of Tables’ or ‘List of Figures’. See Section CrossreferenceSection in Chapter <a href="context-fields">Context Fields</a> for more details.


