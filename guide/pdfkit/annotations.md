# Annotations

An annotation is a rectangular area on a PDF page that the user can interact with. Examples are ‘links’, ‘sticky notes’ and ‘widgets’. All annotation classes and related types live in the TallComponents.PDF.Annotations namespace and nested namespaces.

## Annotation

Annotation is the abstract base class of all annotation classes. The figure below shows the class hierarchy of the annotations that we currently support:

![annotation-class-hierarchy](/guide/pdfkit/media/annotation-class-hierarchy.png)

Class Annotation has the following members that are common to all annotation types:

### Annotation members

<table>
	<tr><th colspan="2">Public properties</th></tr>
	<tr><td>Page</td><td>The page that contains this annotation. Read-only.</td></tr>
	<tr><td>Left, Bottom, Right, Top, Width and Height</td><td>The position and size in page space. Right and Top are read-only.</td></tr>
	<tr><td>BorderWidth</td><td>Width of the border. 0 means no border.</td></tr>
	<tr><td>BorderColor</td><td>Color of the border (must be RgbColor for non-widget annotation).</td></tr>
	<tr><td>BorderStyle</td><td>Style of the border (Solid, Dashed, etc.)</td></tr>
	<tr><td>Invisible</td><td>The annotation is not visible on screen.</td></tr>
	<tr><td>Locked</td><td>Whether the annotation is locked in the viewer application.</td></tr>
	<tr><td>Print</td><td>The annotation is printed.</td></tr>
</table>

<table>
	<tr><th colspan="2">Public methods</th><th></th></tr>
	<tr><td>Flatten</td><td>The page that contains this annotation. Read-only.</td></tr>
	<tr><td>Accept</td><td>Supports the Visitor pattern.</td></tr>
</table>

## Widget

As a special case, a widget annotation (or widget) is the visual representation of a field that provides interactive access to the field value. In theory, a field can have multiple widgets. Widgets will be discussed in more detail in [Forms](forms).

## Link

The link annotation is a clickable area that executes a sequence of actions. To jump to a destination you add a single action of type GoToAction. To jump to a URL you add a single action of type UriAction. Of course you can add any number of actions of any type.

In addition to the members inherited from Annotation, Link has the following specific members:

### Members specific to Link

<table>
	<tr><th colspan="2">Public properties</th><th></th></tr>
	<tr><td>HighlightStyle</td><td>The visual effect that is used when the mouse is pressed inside the annotation area.</td></tr>
	<tr><td>MouseUpActions</td><td>The sequence of actions to execute when the mouse button is released in the Link area.</td></tr>
</table>

Code sample _**Add a cross-reference link inside the same document**_ shows how to add a link to a page that points to another page. Code sample __Add a link to an external PDF document__ shows how to add a link to a page that points to an external PDF document.

## Markup

Markup annotations are used to mark-up a document. In a typical mark-up scenario, the author of a document asks a peer to review the document. The peer will add comments at specific location in the document, e.g. to mark-up a typo or to mark-up a figure that is not clear enough. Mark-ups can take several visual forms of which a simple textual note is the most common one.

Markup is the abstract base class of all markup annotations. It has a number of properties that are common to all markup annotations.

### Memebers specific to Markup

<table><tr><th>
Public properties</th><th></th></tr><tr><td>
Replies, InReplyTo</td><td>
Respectively, all mark-ups that reply to this mark-up and the mark-up to which this mark-up replies. These two properties allow you to create a complete thread of mark-ups. InReplyTo is read-only.</td></tr><tr><td>
Text</td><td>
The text of this mark-up. This text may contain simple formatting tags such as <b>bold</b>.</td></tr><tr><td>
Popup</td><td>
The pop-up that displays the text of this mark-up.</td></tr><tr><td>
Opacity</td><td>
The opacity of this mark-up (0 means fully transparent, 255 means fully opaque).</td></tr><tr><td>
Author, CreationDate and Subject</td><td>
The author, creation date and subject of this mark-up.</td></tr></table>

## Note

A note is the most common mark-up. The class Note is a concrete specialization of the abstract base class Markup. It has the following specific members:

### Members specific to Note

<table><tr><th>
Public properties</th><th></th></tr><tr><td>
IconName</td><td>
Viewer applications have predefined icons for at least the following names: Comment, Key, Note, Help, NewParagraph, Paragraph and Insert.</td></tr><tr><td>
StateModel</td><td>State model of this mark-up. Existing state models:
&nbsp;<ul><li>
Marked: according to this model, a mark-up is marked or not.</li><li>
Review: according to this model, a mark-up has one of the following states: None, Accepted, Rejected, Cancelled or Completed.</li><li>
Migration: according to this model, a mark-up has one of the following states: None, Confirmed or NotConfirmed.</li><li>
None. This mark-up has no state model.</li></ul></td></tr><tr><td>
Marked</td><td>
Meaningful if state model is Marked.</td></tr><tr><td>
ReviewState</td><td>
Meaningful if state model is Review.</td></tr><tr><td>
MigrationState</td><td>
Meaningful if state model is Migration.</td></tr></table>

## Popup

This is a special annotation that displays the text of a markup as a pop-up message. The pop-up allows the user to edit the text. A pop-up is associated with a markup through its Markup property. A markup is associated with a popup through its Popup property. Note that the displayed text is a property of the markup, not of the pop-up.

In addition to the members inherited from Annotation, Popup has the following specific members:

### Members specific to Popup

<table><tr><th>
Public properties</th><th></th></tr><tr><td>
Open</td><td>
Whether the pop-up should initially be displayed open.</td></tr><tr><td>
Markup</td><td>
The Markup to which this pop-up belongs. Read-only.</td></tr></table>

The following code sample creates a new document with a single page. The page has a note with an open pop-up noting that the page is empty.

```
// create a new document with a single empty page
Document document = new Document();
Page page = new Page( PageSize.Letter );
document.Pages.Add( page );

// add a new note
Note note = new Note( 300, 300, 10, 10 );
note.Text = "This page is empty.";
note.IconName = "Note";
page.Markups.Add( note );

// attach an open popup to the note
Popup popup = new Popup( 320, 350, 200, 100 );
popup.Open = true;
note.Popup = popup;

// save the new document
sing ( FileStream file = new FileStream( 
   @"..\..\note.pdf", 
   FileMode.Create, FileAccess.Write ) )
{
   document.Write( file );
}
```

Code sample: Create a new document with a single page, a note and a popup (AddNote)

![created-from-code-sample](/guide/pdfkit/media/created-from-code-sample.png)

**Created from Code sample _**Create a new document with a single page, a note and a popup**_**
