# Context Fields

Context fields are specially formatted pieces of text that are substituted during rendering.


For example, you may assign "See #l #0 on page #p" to the text property of a fragment.


(#l, #0 and #p are the fields.) If the fragment has its GoToAction or Reference pointing to a table that has its Label property set to "Table" and is rendered on page 6, then the fragment appears in PDF as "See Table 3 on page 6".


The following sub-sections discuss the different cases in which context fields can be used.



## Label and Caption Properties

The Label property of a Paragraph reflects the nature of the content. Typical examples are “Table” and “Code Sample”. The Caption property is normally a single line summarizing the content of the Paragraph. For example, if a table shows the telephone region numbers of say Spain, then a typical Caption would be “Telephone region numbers of Spain”. In TallPDF.NET you can set these properties so that you can include this text through a reference field in e.g. a cross references or a ‘List of Tables’ without duplicating these values.


The following code shows how to add an image and set its Caption and Label properties.


```
Image image = new Image( "c:\images\portraits\vincentvangogh.jpg" );
image.Label = "Figure";
image.Caption = "Self-portrait of Vincent van Gogh";
image.Width = section.PageSize.Width * 0.8;
image.HorizontalAlignment = HorizontalAlignment.Center;
image.KeepWithNext = true;
section.Paragraphs.Add( image );
```

Code sample: Add an image with a label and caption in C#


Next, you want to add a line below this image that is formatted like: “Figure 3. Self-portrait of Vincent van Gogh”. Without duplication the caption or the label values, you would use the following code:


```
TextParagraph text = new TextParagraph();
Fragment fragment = new Fragment("#l #0. #c");
Fragment.Reference = image;
text.Fragments.Add(fragment);
section.Paragraphs.Add(text);
```

Code sample: Add a text line below an image that uses context fields for formatting



## Bookmark Property of Heading

The Bookmark property of class Heading is of type string and specifies the format of the heading as respresented in the bookmark/outline tree of the PDF reader application.


The following table shows the different reference fields you may use and how they are interpreted.



### Context fields for the Heading.Bookmark property
&nbsp;<table><tr><th>
Reference field</th><th>
Replaced by</th></tr><tr><td>
#c</td><td>
Caption property of the Heading object.</td></tr><tr><td>
#l</td><td>
Label property of the Heading object.</td></tr><tr><td>
#<i></td><td>
The i-level heading number of the Heading object (1, 2, etc.).</td></tr><tr><td>
#a<i></td><td>
The i-level heading number of the Heading object formatted alphabetically, lowercase (a, b, etc.).</td></tr><tr><td>
#A<i></td><td>
The i-level heading number of the Heading object formatted alphabetically, uppercase (A, B, etc.).</td></tr><tr><td>
#i<i></td><td>
The i-level heading number of the Heading object formatted as roman numerals, lowercase (i, ii, etc.).</td></tr><tr><td>
#I<i></td><td>
The i-level heading number of the Heading object formatted as roman numerals, uppercase (I, II, etc.).</td></tr></table>&nbsp;
The following code sample adds a heading and format the bookmark title.


```
Heading heading = new Heading(0); // level 0
heading.SpacingBefore = 8;
heading.SpacingAfter = 4;
heading.Label = "Chapter";
heading.Caption = "Conceptual Overview";
heading.Fragments.Add(new Fragment("#l #0 - #c", Font.HelveticaBold, 16));
heading.Bookmark = "#0. #c";
section.Paragraphs.Add(heading);
```

Code sample: Add a heading and format the bookmark


The following table shows some specific samples.



### Heading.Bookmark samples
&nbsp;<table><tr><th>
This …</th><th>
Resolves to …</th></tr><tr><td>
#l #0 - #c</td><td>
Chapter 3 - Advantages</td></tr><tr><td>
#0.#1 #c</td><td>
2.3 Advantages</td></tr><tr><td>
#I0.#1 #c</td><td>
II.3 Advantages</td></tr><tr><td>
#0.#a1 #c</td><td>
2.c Advantages</td></tr></table>

## Fragment with HasContextFields

This case applies to the fields in the Text property of a Fragment object that has its HasContextFields property set and has its Reference field set to null.



### Context fields for fragments that do not refer to a paragrap
&nbsp;<table><tr><th>
Context field</th><th>
Replaced by</th></tr><tr><td>
#p</td><td>
Current page number (1-based).</td></tr><tr><td>
#P</td><td>
Total number of pages.</td></tr><tr><td>
#c<i></td><td>
<i> ranges from 0 to 9. The caption of the last i-level heading.</td></tr><tr><td>
#l<i></td><td>
<i> ranges from 0 to 9. The label of the last heading at level i.</td></tr><tr><td>
#<i></td><td>
<i> ranges from 0 to 9. The last heading number at level i (1, 2, etc.).</td></tr><tr><td>
#a<i></td><td>
<i> ranges from 0 to 9. The last heading number at level i formatted alphabetically, lowercase. (a, b, etc.)</td></tr><tr><td>
#A<i></td><td>
<i> ranges from 0 to 9. The last heading number at level i formatted alphabetically, uppercase. (A, B, etc.)</td></tr><tr><td>
#i<i></td><td>
<i> ranges from 0 to 9. The last heading number at level i formatted as roman numerals, lowercase. (i, ii, etc.)</td></tr></table>

## Fragment that Refers to a Heading

This case applies to the Text property of a Fragment of which either the Reference property or the GoToAction points to a Heading.



### Context fields for fragments that refer to a heading
&nbsp;<table><tr><th>
Context field</th><th>
Replaced by</th></tr><tr><td>
#c</td><td>
Caption of the heading.</td></tr><tr><td>
#l</td><td>
Label of the heading.</td></tr><tr><td>
#p</td><td>
The number of the page where the heading is rendered.</td></tr><tr><td>
#<i></td><td>
The i-level heading number of the heading (1, 2, etc.).</td></tr><tr><td>
#a<i></td><td>
The i-level heading number of the heading formatted alphabetically, lowercase (a, b, etc.).</td></tr><tr><td>
#A<i></td><td>
The i-level heading number of the heading formatted alphabetically, uppercase (A, B, etc.).</td></tr><tr><td>
#i<i></td><td>
The i-level heading number of the heading formatted as roman numerals, lowercase (i, ii, etc.).</td></tr><tr><td>
#I<i></td><td>
The i-level heading number of the heading formatted as roman numerals, uppercase (I, II, etc.).</td></tr></table>

### Context field samples for fragments that refer to a heading.
&nbsp;<table><tr><th>
This …</th><th>
Resolves to …</th></tr><tr><td>
See #l #0, #c</td><td>
See Chapter 3, Advantages</td></tr><tr><td>
See also #l #0.#1 on page #p</td><td>
See also Section 2.3 on page 11</td></tr></table>

## Fragment that Refers to a Paragraph

This case applies to the Text property of a Fragment that has a GoToAction or Reference that points to another paragraph such as a Table or Image. Note that in that case you always specify 0 as the level since paragraphs other than Heading paragraphs do not have a level.



### Context fields for fragments that refer to a paragraph
&nbsp;<table><tr><th>
Context field</th><th>
Replaced by</th></tr><tr><td>
#c</td><td>
Caption property of the Paragraph object.</td></tr><tr><td>
#l</td><td>
Label property of the Paragraph object.</td></tr><tr><td>
#p</td><td>
Number of the page on which the Paragraph is rendered.</td></tr><tr><td>
#0</td><td>
Label number of the Paragraph object.</td></tr><tr><td>
#a0</td><td>
Label number of the Paragraph object formatted alphabetically, lowercase (a, b, etc.).</td></tr><tr><td>
#A0</td><td>
Label number of the Paragraph object formatted alphabetically, uppercase (A, B, etc.).</td></tr><tr><td>
#i0</td><td>
Label number of the Paragraph object formatted as roman numerals, lowercase (i, ii, etc.).</td></tr><tr><td>
#I0</td><td>
Label number of the Paragraph object formatted as roman numerals, uppercase (I, II, etc.).</td></tr></table>&nbsp;
Context field samples for fragments that refer to a heading.



### Context field samples for fragments that refer to a heading.
&nbsp;<table><tr><th>
This …</th><th>
Resolves to …</th></tr><tr><td>
#l #0 - #c</td><td>
Figure 3 – Big losses</td></tr><tr><td>
See #l #0 on page #p</td><td>
See Table 3 on page 11</td></tr></table>

## Fragment as Part of a Heading

This case applies to the Text property of a Fragment that is part of a Heading. These fragments are treated as if they have a Reference that points to the enclosing Heading paragraph. See the Section _Fragment that Refers to a Heading_.



## CrossreferenceSection

The CrossreferenceSection lets you generate sections such as a ‘Table of Contents’, a ‘List of Tables’ or a ‘List of Figures’. The CrossreferenceSection was already introduced shortly in <a href="sections">Sections</a>. Because it relies heavily on the concepts introduced in this Chapter, we will discuss it in more detail now.


The mechanism of the CrossreferenceSection is based on the ComposeEntry event. This event is fired for each paragraph in the document. The event handler decides whether and how an entry has to be added to the CrossreferenceSection. E.g. if you want to generate a table of contents, then you would ignore all paragraphs that are not of type Heading.


The following code sample shows how to generate a simple table of contents.


```
CrossreferenceSection toc = new CrossreferenceSection();
toc.ComposeEntry += new ComposeEntryEventHandler( renderTocEntry );
doc.Sections.Add( toc );
```

Code sample: Add a CrossreferenceSection to a document in C#


The code below references the event handler renderTocEntry. Its implementation is shown in the next code sample:


```
static void renderTocEntry(
   CrossreferenceSection section, ComposeEntryEventArgs args )
{
   // check to see if the paragraph is a heading – if not, ignore
   Heading heading = args.CurrentParagraph as Heading;
   if ( !( args.CurrentParagraph is Heading ) ) return;

   // only includes level 0 and level 1 entries
   int level = heading.Level;
   if ( level > 1 ) return;

   // add an entry for this heading
   TextParagraph text = new TextParagraph();
   section.Paragraphs.Add( text );

   // the entry is a clickable link to the heading
   Fragment fragment = new Fragment();
   fragment.Actions.Add( new GoToAction( args.CurrentParagraph ) );
   text.Fragments.Add( fragment );

   // the content of the entry is formatted using context fields
   switch (level)
   {
      case 0:
         fragment.Text = "#0 #c - #p";
         fragment.FontSize = 12;
         break;
      case 1:
         fragment.Text = "#0.#1 #c - #p";
         fragment.FontSize = 10;
         break;
   }
}
```

Code sample: Handle the CrossreferenceSection.ComposeEntry event in C#


