# Developer Guide

PDFRasterizer.NET is a 100% .NET (verifiable) solution for printing PDF documents unattended and converting PDF documents to a raster image, WPF graphics or an XPS document.


This developer guide will help you understand the PDFRasterizer.NET 4.0 component. It walks through the main concepts and illustrates them with code samples. This guide is not intended as a type reference. A full type reference is included in the download and can be viewed online at <a href="https://www.tallcomponents.com/pdfrasterizer/help/">https://www.tallcomponents.com/pdfrasterizer/help/</a>.



## What’s new
&nbsp;<ul><li>
Only medium or high trust required (was full trust in 2.1)</li><li>
Progressive drawing</li><li>
Cancellable drawing</li><li>
Asynchronous drawing</li><li>
Convert to (multi-page) color TIFF (was only Black&White in 2.1)</li><li>
LZW compression for TIFF output</li><li>
More advanced font substitution</li><li>
Convert to XPS</li><li>
Convert to WPF graphics</li><li>
Extract embedded files (also known as attachments)</li><li>
Layer support</li><li>
Diagnostics</li></ul>

## Conventions used in this document

Throughout this document, formatting is used to discern between different types of information. Code samples use a mono-space font.You might have to adjust code slightly after copy/pasting code from this document. Important side notes are formatted using a gray background and a note icon.


```
// Code sample
```
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>These blocks contain tips and warnings that may save you time and trouble.</td></tr></table>

## Please send us Feedback!

Please help us improve this document. If you have any questions, suggestions or find anything in this document is incorrect or missing then let us know at <a href="mailto:support@tallcomponents.com" title="Optional alternate text">support@tallcomponents.com</a> or send us a support request through our website at <a href="https://www.tallcomponents.com/support/submit-issue">www.tallcomponents.com/support/submit-issue</a>.



## Online resources
&nbsp;<ul><li>
Our website: <a href="https://www.tallcomponents.com">www.tallcomponents.com</a></li><li>
PDFRasterizer.NET 3.0 product page: <a href="https://www.tallcomponents.com/pdfrasterizer">www.tallcomponents.com/pdfrasterizer</a></li></ul>

## Naming Convention

We try to adhere as much as possible to Microsoft’s “Design Guidelines for Developing Class Libraries”. The guideline can be found here:


<a href="http://msdn2.microsoft.com/en-us/library/ms229042.aspx" title="Optional alternate text">http://msdn2.microsoft.com/en-us/library/ms229042.aspx</a>



## Licensing

PDFRasterizer.NET 3.0 runs either in licensed or unlicensed mode. It runs in licensed mode if a valid license key is present. How to obtain a valid license key is described in detail in our <a href="https://www.tallcomponents.com/products/licensingguide.pdf">licensing guide</a>.


In unlicensed mode, the component is fully functional and does not expire. It does however impose a few restrictions that makes it unsuitable for use in a production environment. Raster images that are rendered from a PDF page are stamped diagonally with an evaluation notice. Extracted images have a red cross imposed. When converting to WPF, only the first page is converted normally. All other pages are rendered as a single raster image so no vector graphics are preserved.



## Restrictions

The following restrictions appy to PDFRasterizer.NET 3.0:
&nbsp;<ul><li>
Due to GDI+ and XPS restrictions, only the most basic form of transparency is supported.</li><li>
Overprint and rendering intents are ignored.</li><li>
Shading patterns Function-based shadings (type 1), Free-form Gouraud-shaded triangle meshes (type 4), Lattice-form Gouraud-shaded triangle meshes (type 5), Coons patch meshes (type 6) and Tensor-product patch meshes (type 7) are not supported.</li><li>
When printing, spool jobs may be very large due to conversion of glyphs to curves. This happens when there is no matching system font installed and no font substitution if configured. See also <a href="text-and-fonts">Text and Fonts</a></li></ul>&nbsp;
