# Interactor factories

All interactor factories derive from the generic InteractorFactory(InteractorType) class. The base class will raise a Created event whenever a new interactor has been created.


This means that it is not always necessary to create a new interactor factory in order to customize interactors. It is also possible to subscribe to the Created event of one or more existing factories and modify the interactors that they produce. It is possible for example to add interactor layers in this way.


If this does not suffice, one will need to create one or more custom interactor factories and assign these to the appropriate property of the viewer.



## Document Interactor Factories

All document interactor factories derive from the WinFormsDocumentInteractorFactory class. This class provides a CreateDocumentInteractor() virtual that creates new instances of DocumentInteractor. Override this virtual if you want to create your own DocumentInteractor specialization.
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>The StandardPagesViewer will have its DocumentInteractorFactory property set to an instance of WinFormsStandardDocumentInteractoryFactory. This factory will create new instances of StandardDocumentInteractor. The latter implements the zoom modes of the StandardPagesViewer, and it may be used to implement future extensions of the StandardPagesViewer. We do not recommend assigning a different DocumentInteractorFactory in this case, as this may lead to undefined behavior.</td></tr></table>

## Page Interactor Factories

All page interactor factories derive from the WinFormsPageInteractorFactory class. This class provides several virtuals for creating a customized page interactor:
&nbsp;<ul><li>
CreatePageInteractor(): Override this virtual if you want to create your own PageInteractor specialization. By default, WinFormsPageInteractorFactory class creates an instance of PageInteractor.</li><li>
CreateBackGroundLayer(): Adds a background layer to a page. By default it adds a layer that just clears the entire page with the BackColor setting of the RenderSettings. Override this method if you want to provide a different background.</li><li>
CreateUnderLayLayer(): Adds an underlay on top of the Background layer. By default, this layer will draw the graphics that are present in the underlay of the page.</li><li>
CreateOverLayLayer(): The factory will always create a PageContentLayer on top of the underlay. The PageContentLayer is responsible for painting the page content. The CreateOverlayLayer() method will add an overlay layer on top of the content layer. By default this layer will draw the graphics that are present in the overlay of the page.</li><li>
CreateForegroundLayer(): Adds a foreground layer to a page. By default it adds a BorderLayer that draws a thin black border around the page.</li></ul>&nbsp;
All virtuals that create layers may also return null. If so, the layer will not be created.
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>The StandardPagesViewer will have its PageInteractorFactory property set to an instance of WinFormsStandardPageInteractoryFactory. This factory will create new instances of StandardPageInteractor. The latter implements the text selection and annotation selection features of the StandardPagesViewer, and it may be used to implement future extensions of the StandardPagesViewer. We do not recommend assigning a different PageInteractorFactory in this case, as this may lead to undefined behavior.</td></tr></table>

## Annotation Interactor Factories

All annotation interactor factories derive from the WinFormsAnnotationInteractorFactory class. This class provides a large number of virtuals for creating customized annotation interactors. The sections below will treat these in alphabetical order.
&nbsp;<ul><li>
CreateCheckBoxInteractor: This method will be invoked for each CheckBoxWidget. By default, it will create an instance of the CheckBoxInteractor class.</li><li>
CreateDateTimeTextBoxInteractor: This method will be invoked for each Widget that refers to a DateTimeField. By default it will create an instance of a DateTimeTextBoxInteractor.</li><li>
CreateDropDownListInteractor: This method will be invoked for each Widget that refers to a DropDownListField. By default it will create an instance of a DropDownListInteractor.</li><li>
CreateLinkInteractor: This method will be invoked for each Link. By default it will create an instance of a LinkInteractor.</li><li>
CreateListBoxInteractor: This method will be invoked for each Widget that refers to a ListBoxField. By default it will create an instance of a ListBoxInteractor.</li><li>
CreateMultiLineTextBoxInteractor: This method will be invoked for each Widget that refers to a TextField that has the Multiline property set to true. By default it will create an instance of a MultiLineTextBoxInteractor.</li><li>
CreateNoteInteractor: This method will be invoked for each Note markup. By default it will create an instance of a NoteInteractor.</li><li>
CreateNumericTextBoxInteractor: This method will be invoked for each Widget that refers to a NumericField. By default it will create an instance of a NumericTextBoxInteractor.</li><li>
CreatePasswordTextBoxInteractor: This method will be invoked for each Widget that refers to a PasswordField. By default it will create an instance of a PasswordTextBoxInteractor.</li><li>
CreatePopupInteractor: This method will be invoked for each Markup element that has a Popup, at the moment that the popup is opened. By default it will create an AnnotationInteractor, as there is no default implementation yet for popups.</li><li>
CreatePushButtonInteractor: This method will be invoked for each PushButtonWidget. By default it will create an instance of a PushButtonInteractor.</li><li>
CreateRadioButtonInteractor: This method will be invoked for each RadioButtonWidget. By default it will create an instance of a RadioButtonInteractor.</li><li>
CreateSignatureInteractor: This method will be invoked for each SignatureWidget. By default it will create an instance of a PasswordTextBoxInteractor.</li><li>
CreateSingleLineTextBoxInteractor: This method will be invoked for each Widget that refers to a TextField that has the Multiline property set to false. By default it will create an instance of a SingleLineTextBoxInteractor.</li><li>
CreateStampMarkupInteractor: This method will be invoked for each Stamp markup. By default it will create an instance of a StampInteractor.</li><li>
CreateTextMarkupInteractor: This method will be invoked for each Text markup. By default it will create an instance of a TextMarkupInteractor.</li><li>
CreateUnknownMarkupInteractor: This method will be invoked for each markup element that is not an instance of any of the above. By default this method will create an instance of an UnknownMarkupInteractor.</li></ul>&nbsp;
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>Although at this moment the StandardPagesViewer does not use a custom AnnotationInteractorFactory it is not recommended to assign a custom one, as future versions may use the AnnotationInteractorFactory to implement new features.</td></tr></table>&nbsp;
