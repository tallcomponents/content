# Forms

PDF documents may includes fields that the user may fill-out. Such PDF documents are referred to as PDF forms. A typical example is an IRS tax form such as the W-4.

![form](guide/pdfkit/media/form.png)

## Fields

A typical PDF form contains multiple fields of different types. E.g. text fields to enter you first and last name, a group of radio buttons to select your marital status and a checkbox to indicate whether you filed the same tax form last year.


The class TallComponents.PDF.Forms.Fields.Field represent a PDF form field. It is an abstract base class that has the following inheritance hierarchy:

![fields-hierarchy](guide/pdfkit/media/fields-hierarchy.png)

**Fields hierarchy (UnknowmField and UnknowBarcodeField omitted)**

All fields in a PDF document can be accessed through the Document.Fields property. It returns an instance of type TallComponents.PDF.Forms.Fields.FieldCollection. You may be surprised that the collection of fields is contained by a Document instead of by a Page. This is explained in the next section.

## Widgets

A widget is an annotation that provides the visual representation of a field. It also allows the user to interact with the field, e.g. to enter a text or select a date from a calendar control. In general, a field can have multiple widgets but in most cases it only has one. A field is contained by a document. A widget is contained by a page.

![document-fields-pages-and-widgets](guide/pdfkit/media/document-fields-pages-and-widgets.png)

**Document, fields, pages and widgets**

The following diagram shows the Widget inheritance hierarchy.

![widget-inheritance-hierarchy](guide/pdfkit/media/widget-inheritance-hierarchy.png)

**Widget inheritance hierarchy**

As said, a field is associated with one or more widgets. The following table shows what type of field corresponds to what type of widget.

### Correspondence between Field and Widget type

<table>
	<tr><th>Field</th><th>Widget</th><th>Remarks</th></tr>
	<tr><td>SignatureField</td><td>SignatureWidget</td><td>The SignatureWidget has additional properties to customize the appearance, e.g. display text only, image only, both. Display the date or not, display the reaon or not, etc.</td></tr>
	<tr><td>PushButtonField</td><td>PushButtonWidget</td><td>The PushButtonWidget has additional properties that allow you to specify a label, an icon and how the label and the icon should laid out (label above icon, icon only, label only, etc.).</td></tr>
	<tr><td>CheckBoxField</td><td>CheckBoxWidget</td><td>The CheckBoxWidget has an additional appearance to specify the check mark (diamond, check, start, etc.).</td></tr>
	<tr><td>RadioButtonField</td><td>RadioButtonWidget</td><td>Just like the CheckBoxWidget, the RadioButtonWidget has a property to specify the appearance of the chek mark. In addition, it has a property Option that represents one of the radio button options that can be selected.</td></tr>
	<tr><td>DateTimeField, DropDownListField, ImageField, ListBoxField, NumericField, PasswordField, TextField, Code128BarcodeField, Code2of5InterleavedBarcodeField, Code3of9BarcodeField</td><td>Widget</td><td>These fields are all visually represented by the same Widget class.</td></tr>
</table>

All widget types have a common base class Widget. Widget in turn inherits from Annotation, see <a href="annotations">Annotations</a>. The table below lists the members that are specific to Widget annotations.

### Public Widget members

<table>
	<tr><th>Public properties</th><th></th></tr>
	<tr><td>Field</td><td>The field that is associated with this widget. Read-only.</td></tr>
	<tr><td>Font, FontSize, TextColor, HorizontalAlignment, VerticalAlignment</td><td>Properties that specify how text is displayed.</td></tr>
	<tr><td>Invisible, Orientation, BackgroundColor</td><td>Appearance properties.</td></tr>
	<tr><td>Persistency</td><td>Allows you to specify how a widget is persisted when the document is written. You can leave a widget unchanged, you can remove it and finally it can be flattened. This means that after writing the document, the widget is replaced by static, non-interactive content.</td></tr>
	<tr><td>GotFocusActions, LostFocusActions</td><td>Actions to be executed when the widget gets or loses keyboard focus.</td></tr>
	<tr><td>MouseDownActions, MouseUpActions, MouseEnterActions, MouseExitActions</td><td>Actions to be executed per mouse event.</td></tr>
</table>

The following code snippet creates new document with a single page and a text field:

```
// create new document
Document document = new Document();
Page page = new Page( PageSize.Letter );
document.Pages.Add( page );

// add a new text field and add it to the new document
TextField textField = new TextField( "text1" );
textField.Value = "Hello!";
document.Fields.Add( textField );

// add a new widget and connect the widget to the text field and the page
Widget widget = new Widget( 200, 400, 200, 50 );
widget.TextColor = RgbColor.Red;
widget.BorderColor = RgbColor.Black;
widget.BorderStyle = BorderStyle.Solid;
widget.BorderWidth = 2;
textField.Widgets.Add( widget );
page.Widgets.Add( widget );

// save the new document
using ( FileStream file = new FileStream( 
   @"..\..\textfield.pdf", 
   FileMode.Create, FileAccess.Write ) )
{
   document.Write( file );
}
```

Code sample: Create a new document with a single page and a text field (AddTextField)

![text-field-as-created-by-code-sample](guide/pdfkit/media/text-field-as-created-by-code-sample.png)

**Textfield as created by Code sample _Create a new document with a single page and a text field_**

## Fill Fields

Filling a form field is as simple as setting the Value property of a field. Depending on the type of field, special XxxValue properties exist. Here are some code samples that show how to fill form fields:

```
using ( FileStream fileIn = new FileStream( 
   @"fields.pdf", FileMode.Open, FileAccess.Read ) )
{
   Document document = new Document( fileIn );

   TextField textField = document.Fields[ "Text1" ] as TextField;
   textField.Value = "Hello";

   CheckBoxField checkBox = document.Fields[ "Check Box2" ] as CheckBoxField;
   checkBox.CheckBoxValue = CheckState.On;

   RadioButtonField radioButton = document.Fields[ "Radio Button4" ] 
      as RadioButtonField;
   radioButton.RadioButtonValue = radioButton.Options[1]; // second option

   ListBoxField listBox = document.Fields[ "List Box7" ] as ListBoxField;
   listBox.ListBoxValue = 
      new ListOption[] { listBox.Options[1] }; // second option

   DropDownListField dropDown = document.Fields[ "Combo Box8" ] 
      as DropDownListField;
   dropDown.DropDownListValue = dropDown.Options[1]; // second option

   using ( FileStream fileOut = new FileStream( 
      @"out.pdf", FileMode.Create, FileAccess.Write ) )
   {
      document.Write( fileOut );
   }
}
```

Code sample: Fill fields (FillFields)

## Form Data

At each point a field has a value. This can be text entered into a text field or the signature state of a signature field. The collection of all values of all fields of a document is referred to as form data. Form data can be persisted in different formats (e.g The Acrobat JavaScript Scripting Reference mentions a form data format called ‘XFD’. We have no idea what this format is and we suspect that it is a typo (from XFDF which is included as well)), the most known being Form Data Format (FDF). The class FormData is the abstract base class of all classes that each represent one format of persisted form data. The following inheritance hierarchy shows the different FormData classes.

![Form Data-inheritance-hierarchy](guide/pdfkit/media/FormData-inheritance-hierarchy.png)

**FormData inheritance hierarchy**

### Export

Exporting form data in any of the supported format is really easy. Simply call the method Document.Export and specify what format you want:

```
using ( FileStream pdfFile = new FileStream( 
   "fw4.pdf", FileMode.Open, FileAccess.Read ) )
{
   Document document = new Document( pdfFile );
   // fill two fields
   TextField firstName = document.Fields["f1_09(0)"] as TextField;
   firstName.Value = "Chris";
   TextField lastName = document.Fields["f1_10(0)"] as TextField;
   lastName.Value = "Sharp";
   // export as FDF
   FdfFormData fdf = document.Export( FormDataFormat.Fdf ) as FdfFormData;
   fdf.Path = "fw4.pdf";
   // save FDF
   using ( FileStream fdfFile = new FileStream( 
      "fw4.fdf", FileMode.Create, FileAccess.Write ) )
   {
      fdf.Write( fdfFile );
   }
}
```

Code sample: Export form data as FDF (ExportFdf)

### Import

Importing existing form data into a PDF document is just as easy. Simply create a FormData instance from persisted form data (can be in memory) and pass this instance to Document.Import:

```
// open PDF file
using ( FileStream fileIn = new FileStream( 
   "fw4.pdf", FileMode.Open, FileAccess.Read ) )
{
   Document document = new Document( fileIn );

   // open FDF file
   using ( FileStream fdfFile = new FileStream( 
      "fw4.fdf", FileMode.Open, FileAccess.Read ) )
   {
      FdfFormData fdfData = new FdfFormData( fdfFile );
      // import FDF data
      document.Import( fdfData );

   // save modified PDF file 
   using ( FileStream fileOut = new FileStream( 
      "fw4_afterimport.pdf", FileMode.Create, FileAccess.Write ) )
   {
      document.Write( fileOut );
   }
}
```

Code sample: Import form data from FDF (ImportFdf)

## Adobe LiveCycle Designer Support

Dynamic XFA documents are not supported. If you open such a document with, an UnsupportPdfException is thrown. Static XFA documents are supported.

Numeric field, DateTime field, Image field and Barcode field are represented by the corresponding classes in the TallComponents.PDF.Forms.Fields namespace. These field types cannot be created. It is not possible to add these field types to an existing non-XFA (classic) PDF document. It is not possible to add new fields/widgets to static XFA document.

The property Document.DocumentType reflects the type of PDF document. Possible values are: Classic, StatcXfa and DynamicXfa. The enum value DynamicXfa will never be returned because an exception is already thrown at construction time.

To summarize, we support Adobe LiveCycle Designer document with the following restrictions:
<ul><li>
We support static XFA only (no dynamic XFA).</li><li>
We do not support repeated subforms. See ‘repeat subform for each data item’ flag in Binding properties.</li><li>
You cannot add new fields/widgets.</li><li>
You cannot remove fields by calling the FieldCollection.Remove() method. You can however remove fields by setting widget.Persistency to WidgetPersistency.Flatten or WidgetPersistency.Remove.</li><li>
When adding a page from a XFA document to an other document, the XFA fields are automatically converted to classic types (e.g. NumericField will be TextField).</li><li>
We do not support data-bindings. The default binding must be ‘Normal’. You can set field values through property ValueField.Value.</li><li>
We do not support events and script objects/variables.</li></ul>

The following table describes the relationship between the different versions of the Adobe LiveCycle Designer product, the different versions of the XFA format and the different versions of the Adobe PDF Reader and which ones are supported by PDFKit.NET 2.0 and 3.0.

<table><tr><th>
Adobe (LiveCycle) Designer</th><th>
Produces XFA version</th><th>
Requires Adobe PDF Reader</th><th>
Requires PDFKit.NET</th></tr><tr><td>
6.0</td><td>
2.0</td><td>
6.02</td><td>
2.0</td></tr><tr><td>
7.0</td><td>
2.1</td><td>
6.02</td><td>
2.0</td></tr><tr><td>
7.0 BETA</td><td>
2.2</td><td>
7.05</td><td>
2.0</td></tr><tr><td>
7.1</td><td>
2.4</td><td>
7.05</td><td>
2.0</td></tr><tr><td>
8.0</td><td>
2.5</td><td>
8.0</td><td>
2.0</td></tr><tr><td>
8.1</td><td>
2.6</td><td>
8.1</td><td>
3.0</td></tr><tr><td>
8.1.2</td><td>
2.7</td><td>
8.2</td><td>
3.0</td></tr><tr><td>
ES 8.2</td><td>
2.8</td><td>
9.0</td><td>
3.0</td></tr><tr><td>
No public release</td><td>
3.0</td><td>
9.1</td><td>
Not supported yet</td></tr></table>
