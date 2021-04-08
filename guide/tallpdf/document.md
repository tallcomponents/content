# Document

The Document class is the top-level class of the TallPDF.NET object model.



## Create and Save

A typical PDF generation scenario consists of the following basis steps: 1. instantiate a Document; 2. add content and configure document settings; 3. save the document to any stream (typically disk, memory or HTTP response). This is shown in the following code snippets:


```
// create document
Document doc = new Document();

// CODE OMITTED – CONTENT IS ADDED HERE

// save to disk
using ( FileStream file = new FileStream(
   "out.pdf", FileMode.Create, FileAccess.Write))
{
   document.Write( file );
}
```

Code sample: Create a new document, add content (omitted) and save it to disk in C#


```
' create a document
Dim doc As New Document

' CODE OMITTED – CONTENT IS ADDED HERE 

' save to disk
Using file As FileStream = New FileStream( _
   "out.pdf", FileMode.Create, FileAccess.Write)
   doc.Write( file )
End Using
```

Code sample: Create a new document, add content (omitted) and save it to disk in VB.NET



#### Write Overloads

Document.Write has a number of overloads. Depending on what overload you use, the PDF document is generated in either pull or push mode. This is explained in more detail in [Event-Driven Generation](event-driven-generation).


## DocumentInfo

Property DocumentInfo lets you set document level information such as the author and title. This is shown in the next code:


```
Document doc;

doc.DocumentInfo.Author = "Chris Sharp";
doc.DocumentInfo.Title = "TallPDF.NET 4.0 Developer Guide";
```

Code sample: Set document info in C#


```
Dim doc As New Document

doc.DocumentInfo.Author = "Chris Sharp"
doc.DocumentInfo.Title = "TallPDF.NET 4.0 Developer Guide"
```

Code sample: Set document info in VB.NET



## Security

The Security property lets you assign passwords and limit user privileges. The following code creates a document that requires a password to open and cannot be printed:


```
Document doc = new Document();

PasswordSecurity sec = new PasswordSecurity();
sec.Print = false;
sec.OwnerPassword = "gjh456j45";
sec.UserPassword = "egryt646g";
doc.Security = sec;
```

Code sample: Set security in C#


```
Dim doc As New Document

Dim sec As New PasswordSecurity()
sec.Print = False
sec.OwnerPassword = "gjh456j45"
sec.UserPassword = "egryt646g"
doc.Security = sec
```

Code sample: Set security in VB.NET


A typical scenario is to set the user password to the empty string and the owner password to a non-empty string. This will not prompt the user for a password when she opens the document, but a password is required to change security settings.


A common question is how to generate a PDF document that cannot be saved. Unfortunately. The PDF format does not foresee in this feature.


By default the Security property is null (or Nothing) meaning that the document has no security settings.



## ViewerPreferences

The ViewerPreferences property allows you to control the way that a PDF reader application displays the PDF initially. E.g. you can set the initial zoom or hide toolbars or set the reader to fullscreen mode. Here are some typical code samples:


```
Document doc = new Document();

ViewerPreferences vp = new ViewerPreferences();
vp.PageLayout = PageLayout.TwoColumnLeft;
doc.ViewerPreferences = vp;
```

Code sample: Setting viewer preferences in C#


```
Dim doc As New Document

Dim vp As New ViewerPreferences
' display pages in 2 columns - odd page left
vp.PageLayout = PageLayout.TwoColumnLeft
doc.ViewerPreferences = vp
```

Code sample: Setting viewer preferences in VB.NET



## Actions

PDF allows you to associate actions with events. Chapter Actions discusses this topic in more detail . For now it suffices to say that the following properties allow you to associate actions with document-level events:
&nbsp;<ul><li>
AfterPrintAction: This action will be executed by the PDF reader application after the PDF document has been printed.</li><li>
AfterSaveAction: This action will be executed by the PDF reader application after the PDF document has been saved.</li><li>
BeforeCloseAction: This action will be executed by the PDF reader application just before the PDF document is closed.</li><li>
BeforePrintAction: This action will be executed by the PDF reader application just before the PDF document is printed.</li><li>
BeforeSaveAction: This action will be executed by the PDF reader application just before the PDF document is saved.</li><li>
OpenActions: This sequence of actions will be executed by the PDF reader during opening the PDF document.</li></ul>&nbsp;
**Note**that these actions are not executed by TallPDF.NET. They are only associated with events and the actions are executed by the PDF reader application when the corresponding event occurs.


The following code sample generates a PDF document so that a text field is filled with the current date just before it is printed. This way, you can always see on the print out when it was printed. How to add fields to a PDF document is discussed in Section Field Shapes in Chapter [Drawings and Shapes](drawings-and-shapes).


```
Document document = new Document();

JavaScriptAction action = new JavaScriptAction( 
  string.Format( 
    "this.getField('printed').value = 'Printed: {0}'", DateTime.Now));
document.BeforePrintAction = action;

Section section = document.Sections.Add();

Drawing drawing = new Drawing(200, 100);
section.Paragraphs.Add(drawing);

TextFieldShape textField = new TextFieldShape(10, 50, 150, 30, "printed");
drawing.Shapes.Add(textField);

using (FileStream file = new FileStream(
   @"..\..\out.pdf", FileMode.Create, FileAccess.Write))
{
   document.Write(file);
}
```

Code sample: Add a BeforePrintAction to a document in C#



## JavaScripts

The property JavaScripts lets you declare document level JavaScript. This typically contains common functions and constants that can be reused from other JavaScript actions. See JavaScript actions for more a more detailed discussion. The following code sample shows how to declare a JavaScript function at document level:


```
JavaScript javaScript = new JavaScript("function foo() { return 10; }");
document.JavaScripts.Add( javaScript );
```

Code sample: Declare a document-level JavaScript function in C#


```
Dim javaScript As New JavaScript("function foo() { return 10; }")
doc.JavaScripts.Add(javaScript)
```

Code sample: Declare a document-level JavaScript function in VB.NET



## Sections

The property Sections lets you add Section objects to a document. A document has atleast one Section. A section contains the actual content as paragraph objects. Sections are discussed in more detaill in the next Chapter.


