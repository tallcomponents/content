# Developer guide

This developer guide helps you understand the PDFControls.NET 2 class library. It walks through the main concepts and illustrates them with code samples.  


## Features

PDFControls.NET is a .NET class library that allows you to create a GUI application that handles PDF files. It consists of a single assembly that can be xcopy-deployed. It has no dependencies other than the .NET framework. Central to PDFControls.NET is a consistent and highly intuitive object model consisting of classes like Document, Page, Shape, Viewers, Layout Managers, etc. The focus of the development team is always to ease the task of integrating our class library in a larger application.


## Standard versus Professional

PDFControls.NET is available in two editions; Standard and Professional. Both editions are included in the PDFControls.NET distribution. You will find these in the folders “Standard Edition” and “Professional Edition” respectively. Each edition contains its own assembly, its own type reference help file and its own collection of code samples. PDFControls.NET standard is focused on viewing and printing. In particular, the standard edition does not support changing or saving a PDF document. PDFControls.NET Professional offers additional features, such as the ability to fill out forms, editing, and saving.



#### Standard Features

These are the primary features of the standard edition of PDFControls.NET:
&nbsp;<ul><li>
Viewing and Printing of PDF documents.</li><li>
Zooming and Panning</li><li>
Search, select and copy text, including drag-and-drop support.</li><li>
Navigation via bookmarks and links.</li><li>
Use of progressive drawing, ensuring responsiveness.</li><li>
Native .NET 2.0 support including 64-bit support.</li></ul>

#### Professional Features

The professional edition of PDFControls.NET extends the features of the standard edition in the following way:
&nbsp;<ul><li>
Interactive Form support</li><li>
Execution of embedded JavaScript, e.g. to support formatted and calculated fields.</li><li>
Access to the internal PDF document structure, allowing extensive document manipulation.</li><li>
Access to the base GUI elements (interactors), allowing easy customization of the user interface.</li></ul>

## What’s new in version 2?

Compared to version 1 version 2 of PDFControls.NET introduces the following new features:
&nbsp;<ul><li>
Interactor Classes: These allows you to control the looks and feel of your application more easily and to a higher degree than was possible before (professional only)</li><li>
Progressive Drawing: This means that your application will remain responsive, and that graphical content will be shown as soon as it becomes available.</li><li>
Crisp Text: Small text will be drawn crisper than it was in version 1.</li><li>
Layers: drawing of separate layers, as defined in a PDF document.</li><li>
Faster Image Rendering: large bi-level images will be drawn much faster than before.</li><li>
Better Printing Support: new controls allow the creation of a customized print dialog. Print jobs can be made smaller and faster via a font mapping mechanism.</li><li>
Editable Content: The graphics on a page can now be accessed and manipulated (professional only).</li><li>
Import Graphics: WMF, EMF and SVG can be imported (professional only).</li><li>
AES: Support has been added for AES encryption (128 and 256).</li><li>
Embedded Files: Embedded files can be extracted.</li><li>
LiveCycle Designer 8.1 and 82 ES support (static files only)</li></ul>

## Structure of this guide

This guide has been divided into several parts. Chapter [Interactors](interactors) and [Getting started](getting-started) consist of a general introduction, which is followed by three parts:
&nbsp;<ol><li>
Chapters [Viewing](viewing) to [JavaScript](javascript) discuss GUI programming. Chapters [Viewing](viewing) through [Fonts](fonts) discuss concepts that are applicable to both the standard as well as the professional edition. Chapters [Fonts](fonts) through [JavaScript](javascript) are only relevant for the professional edition.</li><li>
Chapters [Document structure](document-structure) to [Extract graphics](extract-graphics) discuss the PDF object model that the professional edition provides. These chapters are not relevant for the standard edition, except perhaps for chapter [Document structure](document-structure) as this gives an overview of the PDF document structure.</li><li>
Chapters [Upgrading from version 1 to 2](upgrading-from-version-1-to-2) to [Upgrading from Standard to Professional](upgrading-from-standard-to-professional) provide information about upgrading from PDFReaderControls 1 to PDFControls 2. In order to accommodate new features and to fix inconsistencies in the API, we have introduced a number of breaking changes.</li></ol>

## Please send us Feedback!

Please help us improve this document. If you have any questions, suggestions or find anything in this document is incorrect or missing then let us know at <a href="mailto:support@tallcomponents.com" title="Optional alternate text">support@tallcomponents.com</a>.



## Visit us Online

Visit our web site: <a href="http://www.tallcomponents.com" title="Optional alternate text">http://www.tallcomponents.com</a>.



## Naming Convention

We try to adhere as much as possible to Microsoft’s “Design Guidelines for Developing Class Libraries”. The guideline can be found here:


<a href="http://msdn2.microsoft.com/en-us/library/ms229042.aspx" title="Optional alternate text">http://msdn2.microsoft.com/en-us/library/ms229042.aspx</a>