# Text

Most likely, the major part of your document will consist of text. The primary class for adding text is TextParagraph. TextParagraph is a specialization of paragraph. Therefore, all members of paragraph are members of TextParagraph as well. So you can align it, apply text flow constraints and add it to a section, area, table cell, header and footer.

## Formatting

TextParagraph is a specialization of Paragraph. It is the primary class for generating a block of text that spans one or more line. A text paragraph consists of one or more fragments. A fragment consists of text and format options like font and text color. If a paragraph consists of text that is formatted with a single font, font size and text color, you need only a single fragment. If you want to mix formatting, you need to add multiple fragments. The following code sample shows how to instantiate a text paragraph and add some text:

```
Document document = new Document();
Section section = document.Sections.Add();

TextParagraph text = new TextParagraph();
text.Fragments.Add(new Fragment("You can mix", Font.Helvetica));
text.Fragments.Add(new Fragment("bold", Font.HelveticaBold));
text.Fragments.Add(new Fragment("and", Font.Helvetica));
text.Fragments.Add(new Fragment("italic.", Font.HelveticaOblique));
section.Paragraphs.Add( text );
```

Code sample: Mix text formatting in C#

## Layout

In addition to the layout options inherited from paragraph, you can set the following properties that control the layout of the text paragraph:

`TextParagraph.Justification`: If set, the word spacing is increased to fill each line. This is illustrated below.

![default-layout](/guide/tallpdf/media/default-layout.png) default layout

![justified-text](/guide/tallpdf/media/justified-text.png) justified

`TextParagraph.FirstLineIndentation`: The amount by which the first line only is indented. This is illustrated below.

![first-line-indentation](/guide/tallpdf/media/first-line-indentation.png) first-line indentation

`TextParagraph.FirstLineIndentation`: The amount by which the first line only is indented. This is illustrated below.

![hanging-indentation](/guide/tallpdf/media/hanging-indentation.png) hanging indentation

`TextParagraph.FirstLineIndentation`: The amount by which the first line only is indented. This is illustrated below.

![line-spacing](/guide/tallpdf/media/line-spacing.png) line spacing

## Preserve White Space

By default, each sequence of white space characters is substituted by a single space. You can cancel this behavior by setting the PreserveWhiteSpace option. This is especially useful when you include programming code in your document where you want to preserve spaces, tabs and line breaks. Set the TabSize property to specify the number of spaces that substitute each tab.

Note that if you set the Fragment.PreserverWhiteSpace property, then TextParagraph.Justification property has no effect.

## Hyphenation or Line Breaking

By default, TallPDF.NET breaks text at whitespace. In some cases however this is not desired. Custom hyphenation or line breaking can be achieved by subscribing to the TextParagraph.LineBreak event.

The arguments of the event include the word to be broken and the current breaking position. Also the maximum position before which the word has to be broken is given. You can also specify wether a hyphen symbol should be used.

## Cross-References

It is possible to add a cross-reference from a fragment to a paragraph. To do this add a GoToAction to the Actions property of Fragment. Alternatively, you can use the ID assigned to the Paragraph.ID and assign it to the GoToAction (This is primarily useful in XML code.) Referencing other paragraphs only works in Push-mode (not event-driven.)

You can embed context fields in the fragment text that will be resolved to attributes of the paragraph. Among these attributes are caption, label and the number of the page where the target paragraph is rendered.

You can also specify different types of destination, like links to external documents (RemoteDestination) or in-document links (InternalPageDestination). These handlers also work in event driven generation mode.

Typical uses of this feature are:

- Cross-references<
- Table of Content Entries
- List of Figures, Tables, etc.
- Auto-Numbering Figures, Tables, etc.

![auto-format-a-cross-reference](/guide/tallpdf/media/auto-format-a-cross-reference.png) 

## NumberedItem

NumberedItem is a specialization of TextParagraph. You can use numbered items to compose a numbered or bulleted list. With respect to TextParagraph, it has an extra fragment collection called NumberFragments. This collection is used to format the number or bullet part of the item. Mostly, this collection will contain only a single fragment. However, if you want to use a mix of fonts or colors in the number part, you can do so.

#### Positioning the bullet or number part

NumberedItem has a number of extra layout properties that let you control the position of the number part with respect to the body of the item. The following figures make this clear.

![alignment](/guide/tallpdf/media/alignment.png)![layout-options-voor-the-bullet-number-part-of-a-numbered-item](/guide/tallpdf/media/layout-options-voor-the-bullet-number-part-of-a-numbered-item.png)

#### Number format

Each numbered item is assigned one or more numbers. An item at level 0 is assigned a single number, an item at level 1 is assigned 2 numbers, etc. The render engine maintains a counter for each level and increments this counter after each assignment. When a counter is incremented, all counters at higher levels are automatically reset. Counters are 1-based. An item may reset the counter of its own level by setting its RestartNumbering property.

A fragment may include these numbers in its number fragment by using context fields (see Chapter [Context Fields](context-fields) for more details). The following shows a typical item list with two levels. The table that follows shows examples of how the bold-faced item may be formatted and resolved.
&nbsp;<ul><li>
first item at level 0</li><li>
second item at level 0
&nbsp;<ul><li>
first item at level 1</li><li id="SubItem2">
second item at level 1</li><li id="SubItem3">
**third item at level 1**</li><li id="SubItem4">
fourth item at level 1</li></ul></li><li>
third item at level 0</li></ul>

### Context fields for the number part of a numbered item
&nbsp;<table><tr><th>
Context field…</th><th>
Resolves to…</th></tr><tr><td>
#0</td><td>
2</td></tr><tr><td>
#0.#1</td><td>
2.3</td></tr><tr><td>
#0/#1</td><td>
2/3</td></tr><tr><td>
#a0</td><td>
b</td></tr><tr><td>
#A0.a1</td><td>
B.c</td></tr><tr><td>
#A0 (i1)</td><td>
B (iii)</td></tr><tr><td>
#I0-#a1</td><td>
II-c</td></tr><tr><td>
\xb7 (font Symbol)</td><td>
Bullet symbol</td></tr></table>

## Headings

Heading is a specialization of NumberedItem. The differences between heading and numbered item are:
&nbsp;<ul><li>
A heading appears in the bookmarks tree of the PDF reader application.</li><li>
Headings have their own set of counters that do not interfere with that of the numbered items, proper.</li><li>
All fragments in the Fragments collection refer automatically to the heading itself.</li></ul>

#### Bookmark appearance

A heading automatically appears as a bookmark in the navigation pane of the PDF viewer. You control the text of this bookmark by setting the Bookmark property to some text that typically includes fields. Example: "#l #0.#1 #c" resolves to something like "Section 3.2 Bookmark"



#### Context fields

All fragments in the Fragments collection refer automatically to the heading itself. This means that fields like #c, #l and #0 will automatically resolve to attributes of this heading. What you will typically do is set the Caption property to the title of this heading and include the #c field in a fragment. The reason for this is that whenever you refer to the heading, e.g. from the TOC, you use fields instead of fragment text. So in order to avoid redundancy you set the text in the Caption and Label properties and refer from your fragments using the #c and #l fields.


The following code sample demonstrates using the Heading class.


```
Document document = new Document();
Section section = document.Sections.Add();

string[] chapters = new string[] {"Vector formats", "Raster formats"};
foreach ( string chapter in chapters )
{
  Heading heading = new Heading(0);
  heading.SpacingBefore = 8;
  heading.SpacingAfter = 4;
  heading.Label = "Chapter";
  heading.Caption = chapter;
  heading.Fragments.Add(new Fragment("#l #0 - #c", Font.HelveticaBold, 16));
  heading.Bookmark = "#0. #c";
  section.Paragraphs.Add(heading);

  string[] topics = new string[] { "X", "Y", "Z" };
  foreach ( string topic in topics )
  {
    heading = new Heading(1);
    heading.SpacingBefore = 4;
    heading.Caption = topic;
    heading.Fragments.Add(new Fragment("#0.#1 #c", Font.HelveticaBold, 12));
    heading.Bookmark = "#0.#1. #c";
    section.Paragraphs.Add(heading);

    for (int k = 0; k < 2; k++)
    {
      TextParagraph text = new TextParagraph();
      text.Fragments.Add(new Fragment("A background area is typically used to add a stationary for a page (company logo, graphical borders), or a watermark. A foreground area can be used for stamping content (Confidential, Draft, etc)."));
      text.SpacingAfter = 10;
      section.Paragraphs.Add(text);
    }
  }
}
```

Code sample: Generate multi-level headings


See the figure below for how the generated PDF document looks in Acrobat 8.0.

![result-after-generating-multi-level-headings](/guide/tallpdf/media/result-after-generating-multi-level-headings.png)