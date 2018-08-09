# Selecting and dragging text

If one sets the CursorMode of the StandardPagesViewer to SelectText, users will be able to select text on a page by dragging the mouse over it. The result of this will be that:

- The text gets highlighted.
- The text gets added to the current selection.
- It becomes possible to drag the text to another interactor (pro only) or application that supports dropping text on it (WordPad for example).

See for example the Reader sample. If one clicks on the toolbar icon with the caret, one will be able to select text. The selected text can subsequently be copied via control-C, or dragged.

![Copying or Dragging selected text](/guide/pdfcontrols/winforms/media/Copying-or-Dragging-selected-text.png)

## Form Fields (professional only)

The professional edition also has support for editing form fields, including text field. The usual cut, copy and paste shortcuts work for text fields. Similar to ordinary text on a (static) page, text in form fields can also be selected and dragged. This can take various forms:
&nbsp;<ul><li>
Text selections from other applications, or from static text on a PDF page can be dragged into a text field. A caret will be displayed at the position that the text will be inserted.</li><li>
Text selections can be dragged within a text field. This will move the selected text. As soon as the cursor leaves the text selection during dragging, a caret will be displayed at the position that the text will be moved to.</li><li>
Text selections can be dragged to other text fields. This will move the selected text.</li><li>
Text selections can be moved or copied to other applications.</li></ul>

## Accessing selected text

The current text selection can be accessed programmatically. The DocumentViewer has a read-only SelectedText property that holds the selected text. In the professional edition this can be either the selected text on a page, or the selected text in a form field.


The professional edition additionally has a Selection property. This is basically a collection that can hold various types of selections (not just text). It has the following properties that are related to text selections:
&nbsp;<ul><li>
Text: this property returns the same value as DocumentViewer.SelectedText. In addition however, it can be used to modify the text of the current selection. At the moment, setting the text will only have effect for text selections in form fields.</li><li>
Glyphs: this is the collection of currently selected glyphs. Each glyph contains information about a single character. Next to the character itself, it contains information about the position of the character on the page, and its font size. The professional edition also contains detailed information about the font of the glyph, as well as the pens and brushes that were used to draw it.</li></ul>&nbsp;
