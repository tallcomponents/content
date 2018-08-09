# Searching and highlighting text

## Searching

Text in a document can be searched with the Document.Find method. There also exists a Page.Find method that allows one to search text on a particular page only. These methods takes a TextFindCriteria instance as its parameter, which allows one to specify the following:
&nbsp;<ul><li>
Backwards: set this to true if you want a text search to start at the end of the document instead of at the beginning. The default is false.</li><li>
MatchCase: set this to true if the text search must only deliver matches that use the exact character casing as specified in the Text property.</li><li>
MatchWholeWord: set this to true of the search must only deliver whole words.</li><li>
Text: specifies the text to be search.</li></ul>

#### TextMatchEnumerator

As text searches may deliver many matches for large documents, searching proceeds lazily. The result of the find call is a TextMatchEnumerator instance. This allows one to obtain – and process - the matches one-by-one.

Next to these standard Enumerator features, the TextMatchEnumerator also has a Progress event. It may take some time before a MoveNext() call returns. This event will be fired every time that a new page is encountered. This makes it possible to show some progress information for example.

#### TextMatch

Each text match contains some information about the text that was found. Specifically:
&nbsp;<ul><li>
TextFindCriteria: a reference to the criteria that resulted in this match.</li><li>
Glyphs: A collection of matching glyphs.</li><li>
Page: The page that the matching glyphs are on.</li></ul>

## Highlighting

Once a particular text match has been found, it can be highlighted in the DocumentViewer by passing the TextMatch instance as an argument to the SelectText method. Doing so has a number of effects:

- The viewer will scroll the text match into view.
- The text match will be assigned to the current selection.
- As a result of the latter, the text match will be highlighted.

Another effect of making the text the current selection is that the viewer will allow the text to be copied with ctrl-C, and that it will support dragging the text to another interactor or application that supports dropping text on it (such as Wordpad).

For an example of this, please see the Reader sample that is included in the distribution. The FindTextDialog implements text searching as described here.

![Searching and Highlighting text](/guide/pdfcontrols/winforms/media/searching-and-highlighting-text.png)

## Custom Glyph Comparing

In principle, text in a PDF document does not need to be stored in reading order. This means that the text search algorithm must first order all text. By default, the Find methods of our assemblies employ a heuristic that will work for relatively simple “western” texts, which consist of a single column and have a reading order from left to right, top to bottom.

Unfortunately, there is no known algorithm that is able to order all text of all possible documents in the proper reading order. Take a newspaper for example. An algorithm for ordering the text would not only need to know about reading orders, but in order to arrive at a perfect ordering it would also need to know about all sorts of layout rules, like headings, columns, captions, etc. This is beyond the scope of the PDFControls.NET.

To deal with this, we allow clients to create their own Glyph Comparer and pass it to the Find method. A Glyph comparer defines a Compare method that takes two glyphs as its arguments, and delivers a value that indicates which of the glyphs comes before the other in terms of reading order.