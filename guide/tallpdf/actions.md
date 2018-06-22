# Actions

In order to make a document responsive to user input, you can add actions. These actions are provided through the Action class and its specializations.


The following actions are supported by TallPDF.NET:
&nbsp;<ul><li>
GoToAction: Jump to a paragraph in the same document or to a page in the same or another PDF document</li><li>
JavaScriptAction: Perform scripted operations against the document</li><li>
UriAction:Jump to a URI</li><li>
UriAction:Jump to a URI</li></ul>&nbsp;
Actions can be assigned to the following properties:
&nbsp;<ul><li>
Document.AfterOpen: Executed after the document has been opened</li><li>
Document.BeforePrint</li><li>
Document.AfterPrint</li><li>
Document.BeforeSave</li><li>
Document.AfterSave</li><li>
Document.BeforeClose</li><li>
LinkShape,MouseUpActions</li><li>
PushButtonFieldShape.MouseUpActions</li><li>
Fragment.Actions</li><li>
Paragraph.Actions</li></ul>

## GoToAction

The GoToAction jumps to a Paragraph inside the current document or to a page in the current or external PDF document. The destination is set by assigning a Destination object to the GoToAction.Destination property.


The following types of Destination exist.



#### ParagraphDestination

The ParagraphDestination points to another paragraph in the same document. The referenced paragraph may either be specifed by object reference or by using the an identifier (this is useful when using XML to define the document).



#### InternalPageDestination

The InternalPageDestination points to a page inside the same document. The page is specified by index. Due to the flow layout model, the exact page where content will be rendered is not know beforehand so the usefullness of this destination is limited.



#### RemoteDestination

The RemoteDestination points to a page in an external PDF document. It is specifed by the path of the PDF document and a page index. You can also specify additional settings such as zoom factor and the exact scroll position after jumping to the location.



## UriAction

The UriAction jumps to a given URI. This is typically a web site address.



## FormAction

FormAction is the abstract base class of the following actions:
&nbsp;<ul><li>
ResetFormAction: reset all form fields in the document to their default value</li><li>
SubmitFormAction: Submits the form data to a given destination. The URL and submit options such as the format can be specified as properties of this action.</li></ul>

## JavaScriptAction

This action execute JavaScript against the current document. A PDF document is fully scriptable on the client side. Typical usage includes setting a text field value after clicking a button or showing/hiding a field after entering/leaving a form field. Here is the full <a href="http://partners.adobe.com/public/developer/en/acrobat/sdk/pdf/javascript/AcroJS.pdf">JavaScript reference</a>.



#### JavaScript

It is possible to define document-level JavaScript. You typically define functions and constants at this level. You can then call and use these from JavaScriptAction objects.


