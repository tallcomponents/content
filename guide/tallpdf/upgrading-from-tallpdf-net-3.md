# Upgrading from TallPDF.NET 3.0

TallPDF.NET 4.0 is backwards compatible TallPDF.NET 3.0, except for these 2 changes
&nbsp;<ul><li>The API of the XhtmlParagraph has been modified to accommodate new features.</li><li>RTF conversion is no longer supported.</li></ul>&nbsp;
This means that TallPDF.NET 3.0 customers migrating to TallPDF.NET 4.0 may need to modify their code if they use the XhtmlParagraph class. Whether this is needed depends on the features they use. This chapter helps you make these changes.



## First Steps

Before starting to upgrade ensure to have a backup of your current project. Next remove the ‘bin’ (binary) and ‘obj’ (object) folders from your project folder (these will be automatically regenerated during the build). This is best practice as sometimes resources (executables or libraries) are locked by a web server or active tool. Visual studio will not be able to update the library during the build in that case. (Usually no problem as the correct version is already in place)


To start upgrading code from 3.0 to 4.0, you should begin by removing the reference to the TallPDF.NET 3.0 assembly and add a reference to the TallPDF.NET 4.0 assembly (TallComponents.PDF.Layout.dll).


Make sure you use the correct build for your project. TallPDF.NET 4.0 is available for .NET 2.0 and .NET 4.0. For .Net 4.0 there is a Client Profile edition and a WPF edition that does not refer to System.Drawing.



## XhtmlParagraph
&nbsp;<ul><li>The BasePath property has been moved to Settings.BasePath.</li><li>The FontPath property has been moved to Settings.FontPath.</li><li>The StyleSheet property has been replaced by Settings.StyleSheets.</li><li>The GetWebCredentials event has been replaced by the Settings.Credentials property. If you need to provide specific credentials for specific resources you can do so by providing a custom resources loader.</li><li>The UseInlineImage property no longer exists. This property was need to circumvent an issue in Adobe Reader. TallPDF 4.0 automatically avoids this issue, so any use of this property can be deleted.</li><li>The Item property no longer exists, because the XhtmlParagraph is no longer a subclass of ParagraphCollection. This should not introduce any incompatibilities because the contents of the collection played no role during PDF generation in earlier versions.</li><li>If you are using XML to generate an XhtmlParagraph, you will need to make sure that the Xhtml data is placed in a <text^gt; element withinm the XhtmlParagraph element. In older versions, the Xtml data could be placed directly in the XhtmlParahraph element. This is no longer possible because the XhtmlParagraph now also may contain a <settings> element, and this is not part of the Xhtml content.</li></ul>

## XhtmlParagraph

The RtfParagraph no longer exists. Please contact us if you need to convert Rtf to PDF.


