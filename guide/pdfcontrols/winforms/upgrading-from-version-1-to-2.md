# Upgrading from version 1 to 2

First of all, please note that PDFReaderControls has been renamed to PDFControls. The original goal of providing software that allows one to create a PDF reader has long since been surpassed by functionality for filling out forms, editing PDF files, etc. Consequently, the original name is not quite appropriate anymore.


The most substantial change however, is the programming API of PDFControls.NET. The new API is not source-compatible with the old one. If you are using the old API, you will need to change your code to make it work using the new one. The purpose of this guide is to help you with this.
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>This chapter only deals with changes in the GUI part of PDFControls.NET. The next chapter will deal with changes in the PDF object model.</td></tr></table>

## Compatibility Sources

To ease the transition of the GUI part, we have provided source code that maps an important part of the old API onto the new one. Please see the sources in Code Samples/CS/PDFReaderControls 1 Compatibility. These will be referred to as the compatibility sources below. These sources attempt to map as much as functionality as possible, but it is impossible to map every detail of the old API.


If you find that it is difficult or even impossible to implement a feature that was more easily implemented in PDFReaderControls 1, please let us know. We will fix this as soon as we can, or think of an alternative way to do things. If applicable, we will also extend the compatibility sources, or provide additional samples.



## New Concepts

PDFReaderControls 1 basically consisted of a few WinForms viewer controls that could be incorporated in an application. These provided customization via a number of methods, properties and events. This is still true for PDFControls 2, but it also introduces a few fundamental changes, in particular for the professional edition.



#### Standard Edition

Compared to the professional edition, the programming API of the standard edition has changed relatively little. Most importantly, the standard programming API has been “synchronized” with the professional API, so that it is easier to upgrade standard application code to professional. This means that:
&nbsp;<ul><li>
The standard edition now also gives access to Page objects within a document.</li><li>
Pages have a DocumentInfo property that contains properties like Author and Title.</li><li>
The PagesViewer introduces the concept of an ActionHandler. Various events that were previously fired by the PagesViewer can now be handled in a custom ActionHandler.</li><li>
Various classes have been moved from the TallCompoments.PDF.ReaderControls namespace to the same namespace as their counterparts in the professional edition.</li></ul>

#### Professional Edition

Compared to the standard edition, the API of the professional edition has changed in a more fundamental way in order to provide better customization. Although possible to some extent, PDFReaderControls 1 did not allow easy customization of the elements that were displayed inside its WinForms controls. Customization was mostly limited to what was possible in the underlying PDF model.


The professional edition of PDFControls 2 introduces a number of new concepts that makes customization easier.
&nbsp;<ul><li>
Interactors. These are similar to controls in WinForms and UIElements in WPF. Interactors represent GUI elements inside the viewer controls that we provide. Their main feature is a set of overrideable event handlers that specify the looks and behavior of the interactor (OnMouseDown, OnPaint, etc.). After opening a PDF document, Interactors get created automatically for the document, its pages and its annotations.</li><li>
Interactor Factories: As interactors get created automatically, one needs a way to specify which interactors to create beyond what we provide by default. This is what interactor factories are for. There exist factories for interactors at document level, page level and annotation level. Assigning a different factory to a viewer control will allow you to use your own custom interactors.</li><li>
Layout Managers: layout managers control how sub-interactors are visually arranged with respect to their parent. Document interactors have a layout manager that controls the layout of its Page interactors. Page interactors have a layout manager that controls the layout of its annotations.</li><li>
Interactor Layers. Although it is possible to introduce new behavior by subclassing existing interactors, this is not practical if a similar feature is to be added for a large set of different interactor types (a border for example). Like interactors, layers also have a set of overridable event handlers (OnMouseDown, OnPaint, etc.). Layers can be attached to an existing interactor, and then they will override its handlers. It is possible to attach multiple layers. The handlers of the layer that was added last (the outermost layer) will take precedence.</li></ul>

## Known Issues

See below for a list of known issues:
&nbsp;<ul><li>
Although the Page class has been extended with VisualOverlay and VisualUnderlay properties, these are not yet rendered by the viewer controls of PDFControls.NET 2.</li></ul>

## Upgrade Steps

PDFControls.NET 2 is not source code compatible with PDFReaderControls.NET 1. Changes have been made to make the object model more intuitive, consistent and powerful. We also made the component more compliant with the Microsoft .NET coding guidelines for Class Libraries to further increase usability of the product.


For PDFReaderControls.NET 1 customers that upgrade to PDFControls.NET 2 this means code modifications are required. This and the following chapters help you to migrate code from version 1 to version 2. It describes the changes from PDFReaderControls.NET 1 to PDFControls.NET 2 and provides an upgrade strategy compiled from experiences gained during development of the product.


This chapter focuses on upgrade issues that are common to both PDFControls editions. The following two chapters will focus on issues that are specific for the standard edition and the professional edition respectively.


This chapter does not explain how to integrate the new PDFControls.NET 2 features. For this we refer to the Developer Guide.



#### .NET framework support

The table below shows the differences in .NET framework support



### .NET Framework compatibility
&nbsp;<table><tr><th>
.NET Framework version</th><th>
PDFReaderControls.NET 1.x</th><th>
PDFControls.NET 2.0</th></tr><tr><td>
.NET 1.0</td><td>
No</td><td>
No</td></tr><tr><td>
.NET 1.1</td><td>
Yes (.NET 1.1 build)</td><td>
No (If you are using .NET 1.1 or earlier, you cannot upgrade to PDFControls.NET 2)</td></tr><tr><td>
.NET 2.0 - .NET 3.5</td><td>
Yes (.NET 2.0 build)</td><td>
Yes (.NET 2.0 build)</td></tr></table>

#### Step 1: Backup existing project

Before upgrading make sure you have a backup of your current project. Next remove the \bin and \obj folders from your project folder (these will be automatically re-generated during the build). This is best practice as sometimes generated files are locked by a web server or tool. Visual Studio will not be able to update the library during the build in that case.



#### Step 2: Update reference to PDFControls.NET

Next, remove the reference to the PDFReaderControls.NET 1 assembly (Versions of PDFReaderControls.NET before 1.1 required the TallComponents.Imaging.dll. For versions 1.1 and higher, and for PDFControls.NET 2 this library is no longer needed and can be removed) (TallComponents.PDF.ReaderControls.dll) and add a reference to the PDFControls.NET 2 assembly (TallComponents.PDF.WinForms.dll).



#### Step 3: Consider using the compatibility sources

You may consider using the PDFReaderControls 1 compatibility sources that we provide as a convenience. See Code Samples/CS/PDFReaderControls 1 Compatibility/TallComponents.PDF.ReaderControls. This folder contains sources that implement many features of the old version 1 programming API on top of the new version 2 programming API. These sources attempt to provide a comprehensive mapping, but being a sample, we do not guarantee 100% coverage. These sources are subject to change in the sense that future versions may provide better coverage.


The advantage of using the compatibility sources is that they provide a quick starting point. The disadvantage is that they may not lead to the best possible design to take full advantage of the new PDFControls 2 features.


In case the compatibility sources do not cover all the features that you need, please proceed with the strategy below to implement them.



#### Step 4: Apply namespace changes

In PDFReaderControls.NET 1, a number of classes are located in the TallComponents.PDF.ReaderControls. In PDFControls.NET 2 this namespace has been removed. Two changes have occurred in this respect.
&nbsp;<ul><li>
New namespaces have been introduced in order to accommodate new features and to order these in a logical way. Some classes that were previously in TallComponents.PDF.ReaderControls have been moved to these namespaces.</li><li>
Some classes have been moved from the TallComponents.PDF.ReaderControls namespace into the TallComponents.PDF namespace. This has been done so that PDFControls.NET is more consistent with PDFKit.NET, and internally between the professional and standard edition. This makes it easier to upgrade from PDFKit.NET to PDFControls.NET professional, and from standard to professional. This change is most apparent in the standard edition of PDFControls.NET, as it did not have these namespaces before.</li></ul>&nbsp;
**New namespaces in the standard edition**


The following table summarizes the new namespaces for the standard edition. Compared to the professional edition, the standard edition lacks all functionality that is related to interactors. It does have a number of new namespaces in the TallComponents.Interaction and TallComponents.PDF ranges though. These correspond to the same namespaces that exist in the professional edition.



### New namespaces for the professional edition.
&nbsp;<table><tr><th>
Topic</th><th>
Namespace</th></tr><tr><td>
General interaction</td><td>
TallComponents.Interaction</td></tr><tr><td>
Layout managers</td><td>
TallComponents.Interaction.LayoutManagers</td></tr><tr><td>
WinForms specific interaction</td><td>
TallComponents.Interaction.WinForms</td></tr><tr><td>
WinForms controls</td><td>
TallComponents.Interaction.WinForms.Controls</td></tr><tr><td>
WinForms event</td><td>
TallComponents.Interaction.WinForms.Events</td></tr><tr><td>
General PDF document access</td><td>
TallComponents.PDF</td></tr><tr><td>
PDF action Handling</td><td>
TallComponents.PDF.Actions</td></tr><tr><td>
Print settings</td><td>
TallComponenrts.PDF.Printing</td></tr><tr><td>
PDF document navigation</td><td>
TallComponents.PDF.Navigation</td></tr><tr><td>
PDF rendering</td><td>
TallComponents.PDF.Rasterizer</td></tr><tr><td>
Font rendering</td><td>
TallComponents.PDF.Rasterizer.Fonts</td></tr><tr><td>
PDF security</td><td>
TallComponents.PDF.Security</td></tr><tr><td>
Column 1</td><td>
Column 2</td></tr><tr><td>
PDF text extraction</td><td>
TallComponents.PDF.TextExtraction.</td></tr><tr><td>
2-D transformations</td><td>
TallComponents.PDF.Transforms</td></tr></table>&nbsp;
**New namespaces in the professional edition**


The following table summarizes the new namespaces for the professional edition. The professional edition has no new namespaces in the TallComponents.PDF range, as these already existed in PDFReaderControls 1.



### New namespaces for the professional edition.
&nbsp;<table><tr><th>
Topic</th><th>
Namespace</th></tr><tr><td>
General interaction</td><td>
TallComponents.Interaction</td></tr><tr><td>
Layout managers</td><td>
TallComponents.Interaction.LayoutManagers</td></tr><tr><td>
WinForms specific interaction</td><td>
TallComponents.Interaction.WinForms</td></tr><tr><td>
WinForms controls</td><td>
TallComponents.Interaction.WinForms.Controls</td></tr><tr><td>
WinForms interaction events</td><td>
TallComponents.Interaction.WinForms.Events</td></tr><tr><td>
WinForms interactor factories</td><td>
TallComponents.Interaction.WinForms.Factories</td></tr><tr><td>
WinForms interactors</td><td>
TallComponents.Interaction.WinForms.Interactors</td></tr><tr><td>
WinForms interactor layers</td><td>
TallComponents.Interaction.WinForms.Layers</td></tr><tr><td>
Print settings</td><td>
TallComponenrts.PDF.Printing</td></tr><tr><td>
Font rendering</td><td>
TallComponents.PDF.Rasterizer.Fonts</td></tr><tr><td>
2-D transformations</td><td>
TallComponents.PDF.Transforms</td></tr></table>&nbsp;
**TallComponents.Interaction.Winforms**


The following table shows which PDFReaderControls.NET 1 classes have been moved to the TallComponents.Interaction.WinForms namespace.



### Classes moved from TallComponents.PDF.ReaderControls to TallComponents.Interaction.WinForms.
&nbsp;<table><tr><th>
Class</th><th>
Description</th></tr><tr><td>
DefaultActionHandler</td><td>
Handles the common actions for a pages viewer</td></tr></table>&nbsp;
**TallComponents.Interaction.Winforms.Events**


The following table shows which PDFReaderControls.NET 1 classes have been moved to the TallComponents.Interaction.WinForms.Events namespace. Some of these classes have also been renamed to be more consistent with the general event naming scheme.



### Classes moved from TallComponents.PDF.ReaderControls to TallComponents.Interaction.WinForms.Events.
&nbsp;<table><tr><th>
Class</th><th>
Description</th></tr><tr><td>
UnhandledExceptionEventArgs</td><td>
Event fired when an unhandled exception gets encountered.</td></tr><tr><td>
AfterBookmarkSelectedEventArgs</td><td>
Renamed to BookMarkEventArgs. Event fired after a bookmark has been selected.</td></tr><tr><td>
BookmarkTitleEditEventArgs (professional only)</td><td>
Renamed to BookmarkTitleEventArgs. Event fired before and after a bookmark title got edited.</td></tr><tr><td>
SetBookmarkIconEventArgs (professional only)</td><td>
Renamed to BookmarkIconEventArgs. Fired to request a custom bookmark icon.</td></tr></table>&nbsp;
**TallComponents.Interaction.Winforms.Controls**


The following table shows which PDFReaderControls.NET 1 classes have been moved to TallComponents.Interaction.WinForms.Controls. This is mostly the same for the standard and the professional edition alike.



### Classes moved from TallComponents.PDF.ReaderControls to TallComponents.Interaction.WinForms.Controls.
&nbsp;<table><tr><th>
Class</th><th>
Description</th></tr><tr><td>
PagesViewer</td><td>
Shows the pages of a PDF document. The professional edition allows users to fill out forms, and perform other types of interaction with the document.</td></tr><tr><td>
ThumbnailsViewer</td><td>
Shows thumbnails of the pages of a PDF document. Allows navigation to particular pages in a PagesViewer when connected to it.</td></tr><tr><td>
BookmarksViewer</td><td>
Shows the bookmarks of a PDF document. Allows navigation to particular pages in a PagesViewer when connected to it.</td></tr><tr><td>
PrintDialog</td><td>
Shows a print dialog for a particular document or PagesViewer (in the latter case it has the notion of a “current” page).</td></tr><tr><td>
CursorMode</td><td>
Determines the effect while moving, dragging or clicking the mouse if one uses the StandardPagesViewer. The plain PagesViewer does not have a predefined cursor mode, as it allows arbitrary mouse behavior to be defined via custom interactors, and a predefined CursorMode might interfere.</td></tr><tr><td>
TextSelectMode</td><td>
Determines the text selection mode when CursorMode = SelectText. Only available in the StandardPagesViewer. Also see CursorMode above.</td></tr><tr><td>
PageLayout</td><td>
Different ways of laying out pages. Only available in the StandardPagesViewer.</td></tr><tr><td>
ZoomMode</td><td>
The zoom mode. Only available in the StandardPagesViewer.</td></tr></table>&nbsp;
**TallComponents.PDF**


The following table shows which PDFReaderControls.NET 1 classes have been moved to the TallComponents.PDF namespace.



### Classes moved from TallComponents.PDF.ReaderControls to the TallComponents.PDF namespace.
&nbsp;<table><tr><th>
Class</th><th>
Description</th></tr><tr><td>
Document</td><td>
Represents the structure of a PDF document.</td></tr><tr><td>
HorizontalAlignment (standard only)</td><td>
Defines the horizontal alignment of graphical objects.</td></tr><tr><td>
VerticalAlignment (standard only)</td><td>
Defines the vertical alignment of graphical objects.</td></tr><tr><td>
VerifyIdentityEventArgs (professional only)</td><td>
Event fired to determine the status of a signature field.</td></tr></table>&nbsp;
**TallComponents.PDF.Printing**


The following table shows which PDFReaderControls.NET 1 classes have been moved to the TallComponents.PDF.Printing namespace.
&nbsp;<table><tr><th>
Class</th><th>
Description</th></tr><tr><td>
PrintSettings</td><td>
Defines overall settings used for printing.</td></tr><tr><td>
PagePrintSettings</td><td>
Defined page-specific settings used for printing. These may differ per page.</td></tr><tr><td>
QueryPrintPageSettings (professional only)</td><td>
Event fired to request PrintPageSettings for a particular page.</td></tr></table>&nbsp;
**Other classes that have been moved to TallComponents.PDF….**


Below, we will have listed the remaining classes that were previously in the TallComponents.PDF.ReaderControls namespace of the standard edition, and that have been moved in order to make the standard edition more compatible with the professional edition (i.e. the professional already contained these classes in these namespaces).
&nbsp;<table><tr><th>
Class</th><th>
New namespace</th></tr><tr><td>
Bookmark</td><td>
TallComponents.PDF.Navigation</td></tr><tr><td>
BookmarkCollection</td><td>
TallComponents.PDF.Navigation</td></tr><tr><td>
Destination</td><td>
TallComponents.PDF.Navigation</td></tr><tr><td>
PasswordSecurity</td><td>
TallComponents.PDF.Security</td></tr><tr><td>
Security</td><td>
TallComponents.PDF.Security</td></tr><tr><td>
Glyph</td><td>
TallComponents.PDF.TextExtraction</td></tr><tr><td>
TextFindCriteria</td><td>
TallComponents.PDF.TextExtraction</td></tr><tr><td>
TextMatch</td><td>
TallComponents.PDF.TextExtraction</td></tr><tr><td>
TextMatchEnumerator</td><td>
TallComponents.PDF.TextExtraction</td></tr><tr><td>
ProgressEventArgs</td><td>
TallComponents.PDF.TextExtraction</td></tr></table>

#### Step 5: Dealing with removed and changed classes.

The last steps involve dealing with removed or changed classes. These steps differ for the two editions, so they are described in the edition-specific chapters below.


