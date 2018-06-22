# Paragraphs

Paragraphs are the primary content building blocks of a document. All paragraph classes inherit from the abstract base class Paragraph. This base class has common properties such as HorizontalAlignment, SpacingBefore, SpacingAfter and KeepWithNext.

Paragraph specializations are Drawing, Image, Table, HorizontalLine, ParagraphCollection, RtfParagraph, XhtmlParagraph, TextParagraph, NumberedItem, BookmarkParagraph and Heading.

![paragraph-class-hierarchy](guide/tallpdf/media/paragraph-class-hierarchy.png)

The content inside Section, Area and Cell (Defined in Chapter <a href="tables">Tables</a>) objects is defined as a collection of Paragraph objects. Paragraph is an abstract class from which a number of paragraphs derive. This Chapter discusses the commonality of all these classes. The following chapters discuss specific paragraphs such as TextParagraph, Image, Table and Drawing in more detail.

The following code samples show how to instantiate different paragraph objects and add them to a section.

```
Document doc = new Document();
Section section = new Section();
doc.Sections.Add(section);

TextParagraph textParagraph = new TextParagraph();
// ... add fragment objects
section.Paragraphs.Add(textParagraph);

Image image = new Image("some.jpg");
// ... set image properties
section.Paragraphs.Add(image);

Table table = new Table();
// ... add rows, cells, etc.
section.Paragraphs.Add(table);

Heading heading = new Heading();
// ... set heading properties such as level
section.Paragraphs.Add(heading);
```

Code sample: Add paragraph objects to a section in C#

## Positioning

At what position a paragraph is rendered follows from the render process that precedes it; this is obviously beyond the control of an individual paragraph. However, when a paragraph is about to be rendered, there are a number of properties that control the position of the paragraph relative to the current position. These are:
&nbsp;<ul><li>
Paragraph.SpacingBefore adds additional white space between this paragraph and its predecessor.</li><li>
Paragraph.SpacingAfter adds additional white space between this paragraph and its successor. (Admittedly, this doesn't control the position of the current paragraph but that of the next.)</li><li>
Paragraph.LeftIndentation adds additional white space between the left edge of this paragraph and the left page margin.</li><li>
Paragraph.RightIndentation adds additional white space between the right edge of this paragraph and the right page margin.</li><li>
Paragraph.HorizontalAlignment aligns the paragraph with respect to the left and right margins of the page.</li></ul>&nbsp;
See: _Paragraph positioning relative to the cursor_

![paragraph-positioning-relative-to-the-cursor](guide/tallpdf/media/paragraph-positioning-relative-to-the-cursor.png)

## Flow Constraints

It is possible to apply flow contraints to paragraphs. These properties are:
&nbsp;<ul><li>
Paragraph.DoNotBreak instructs the layout engine to not break this paragraph across pages. Note that this property only applies to paragraphs that can be broken (text and tables)</li><li>
Paragraph.KeepWithNext instructs the layout engine to keep atleast some of this paragraph on the same pages as some of the next paragraph.</li><li>
Paragraph.StartOnNewPage instructs the layout engine to start this paragraph on a new page.</li></ul>&nbsp;
The DoNotBreak and KeepWithNext constraints are typically applied to headings (not by default). The KeepWithNext should also be applied to a Figure or Table that is followed by a caption. The StartOnNewPage constraint makes sense for e.g. a chapter heading.


The following three figures illustrate the DoNotBreak and KeepWithNext flow constraints.

![paragraph-flow-without-constraints](guide/tallpdf/media/paragraph-flow-without-constraints.png)

![do-not-break-flow-constraint](guide/tallpdf/media/do-not-break-flow-constraint.png)

![keep-with-next-flow-constraint](guide/tallpdf/media/keep-with-next-flow-constraint.png)

