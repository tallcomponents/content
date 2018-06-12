# Actions

PDF allows you to associate actions with events. These actions are provided through the Action class and its specializations.


The following actions are supported:
&nbsp;<ul><li>
FormAction: Submit or clear form data</li><li>
GoToAction: Jump to a destination in the same document or to a page in another PDF document</li><li>
HideAction: Show or hide an annotation</li><li>
ImportDataAction: Import an FDF file</li><li>
JavaScriptAction: Perform scripted operations against the document (Professional edition only)</li><li>
LaunchAction: Launch an application</li><li>
NamedAction: Execute a named action</li><li>
UriAction: Jump to a URI</li></ul>&nbsp;
Each action type is discussed in detail below.



## Events

It is the responsibility of the PDF reader to execute the actions that are associated with an event whenever the event occurs. The following properties are of type Action. The properties correspond to events.
&nbsp;<ul><li>
Document.OpenActions: These actions are executed after the document has been opened</li><li>
Document.AfterPrint: These actions are executed after the document has been printed</li><li>
Document.AfterSave: These actions are executed after the document has been saved</li><li>
Document.BeforeClose: These actions are executed before the document is closed</li><li>
Document.BeforePrint: These actions are executed before the document is printed</li><li>
Document.BeforeSave: These actions are executed before the document has been saved</li><li>
Link.MouseUpActions: These actions are executed when the mouse button is released while the pointer is inside the link</li><li>
Widget.GotFocusActions: These actions are executed when the widget receives keyboard focus</li><li>
Widget.LostFocusActions: These actions are executed when the widget loses keyboard focus</li><li>
Widget.MouseDownActions: These actions are executed when the mouse button is pressed while the pointer is inside the widget</li><li>
Widget.MouseEntrerActions: These actions are executed when the mouse pointer enters the widget</li><li>
Widget.MouseExitActions: These actions are executed when the mouse pointer leaves the widget</li><li>
Widget.MouseUpActions: These actions are executed when the mouse button is released while the pointer is inside the widget</li><li>
Bookmark.Actions: These actions are executed when the bookmark is selected</li></ul>&nbsp;
Note that all properties are of type ActionCollection. All actions in the collection are executed in order.



## FormAction

FormAction is the abstract base class of the following actions:
&nbsp;<ul><li>
ResetFormAction: reset all form fields in the document to their default value</li><li>
SubmitFormAction: Submits the form data to a given destination. The URL and submit options such as the format can be specified as properties of this action.</li></ul>&nbsp;
Note that all properties are of type ActionCollection. All actions in the collection are executed in order.



## GoToAction

The GoToAction jumps to a destination. The destination is set by assigning the GoToAction.Destination property.


The following types of destinations exist.



#### Internal destination

The InternalDestination points to a page inside the same document. It is possible to specify the exact location of the viewer, the zoom factor and the way that the page is displayed.



#### Remote destination

The RemoteDestination points to a page in an external PDF document. It is specified by the path of the PDF document and a page index. You can also specify additional settings such as zoom factor and the exact scroll position after jumping to the location.



#### Named destination

Class Document has a property called NamedDestinations which is a collection of name-destination pairs. The NamedDestination class lets you specify a destination by specifying the name of a destination in this collection.



## HideAction

This action hides or shows one or more annotations or fields.



## ImportDataAction

This action lets you import an FDF file by specifying the path.



## JavaScriptAction

This action executes JavaScript against the current document. A PDF document is fully scriptable on the client side. Typical usage includes setting a text field value after clicking a button or showing/hiding a field after entering/leaving a form field. Here is the full JavaScript reference.


The following code sample generates a PDF document so that a text field is filled with the current date just before it is printed. This way, you can always see on the print out when it was printed. (How to add fields to a PDF document is discussed in Section Field Shapes.)


```
Document document = new Document();

JavaScriptAction action = new JavaScriptAction( 
  string.Format( 
    "this.getField('printed').value = 'Printed: {0}'", DateTime.Now));
document.BeforePrintAction = action;

using (FileStream file = new FileStream(
   @"..\..\out.pdf", FileMode.Create, FileAccess.Write))
{
   document.Write(file);
}
```

Code sample: Add a BeforePrintAction to a document in C#



#### Document Level JavaScript

The property Document.JavaScripts lets you declare document level JavaScript. This typically contains common functions and constants that can be reused from other JavaScript actions. See JavaScript actions for a more detailed discussion. The following code sample shows how to add a JavaScript function at document level:


```
JavaScript javaScript = new JavaScript("function foo() { return 10; }");
document.JavaScripts.Add( javaScript );
```

Code sample: Add a document-level JavaScript function in C#


The document level javascript will need to be run at least once before the definitions in it become available to other javascript code. Existing document level javascript will be run automatically when the document gets opened (if allowed by the ScriptBehavior property), so existing definitions will be available automatically. If you add new definitions however, and you want to use these immediately without saving and reopening the document, you will need to run the document level javascript code explicitly. In order to do so, please execute the following code before executing any other javascript code that relies on it.


```
foreach (JavaScript javaScript in document.JavaScripts)
{           
  javaScript.Run(document);
}
```

Code sample: Explicitly running document level javascript in C# in order to declare new functions.



## LaunchAction

This action lets you launch an application. The path property of this class may point either to the application itself or to a document which is then opened by the associated application.



## LaunchAction

Adobe has specified a number of named actions. You can specify these through this class by setting the NamedAction.Name property to any of the following values:
&nbsp;<ul><li>
NextPage: go to the next page</li><li>
PrevPage: go to the previous page</li><li>
FirstPage: go to the first page</li><li>
LastPage: go to the last page</li></ul>

## UriAction

The UriAction jumps to a given URI such as http://www.tallcomponents.com.


