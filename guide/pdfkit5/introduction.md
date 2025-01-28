# Developer Guide

This developer guide will help you understand the PDFKit.NET 5.0 class library. It walks through the main concepts and illustrates them with code samples. This guide is not a type reference. Instead, this is included in the <a href="https://tallcomponents.com/products/pdfkit5/download" title="Optional alternate text">evaluation download</a>.



## Features

PDFKit.NET is a 100% managed (verifiable) .NET class library for creating and manipulating PDF documents. It consists of just a single assembly that can be xcopy-deployed. It has no dependencies other than the .NET framework. Central to PDFKit.NET is a consistent and highly intuitive object model consisting of classes like Document, PageCollection, Page, Canvas, Shape, Bookmark, Annotation, Field, etc. The focus of the development team is always to ease the task of integrating our class libray in a larger application. These are PDFKit.NET’s primary features:
&nbsp;<ul><li>
Fill and flatten PDF forms</li><li>
Split and assemble PDF documents</li><li>
Apply security settings such as passwords</li><li>
Stamp content on new and existing pages</li><li>
Support for CMYK and Grayscale colorspace</li><li>
Read, Create and Modify Actions, Sticky Notes and Links</li><li>
Active JavaScript interpretation (for formatted and calculated fields)</li><li>
Adobe LiveCycle Designer compatibility (static forms only)</li><li>
Extract and search text</li><li>
Digitally signing and verification</li><li>
Native .NET 2.0 support, including 64-bit support</li></ul>

## What’s new in 5.0?

The following features have been added in 5.0:
&nbsp;<ul><li>
We added a WPF-only build without a reference to System.Drawing</li><li>
You can now save to a PDF/A-1b compliant document</li><li>
We added support for QR barcodes.</li><li>
The ImageShape now supports loading of JBIG2 images.</li><li>
.NET 1.1 is no longer supported</li></ul>

## Code Samples in this Guide

All non—trivial code samples are available as ready-to-run Visual Studio .NET projects. These samples are included in the installation package which can be <a href="http://www.tallcomponents.com/pdfkit/download" title="Optional alternate text">downloaded from our web site</a>.


Just unzip to any folder and open codesample.sln in either Visual Studio.NET 2003 or 2005 (will convert first). Each code sample is available as a separate project inside this solution. If the code sample is part of the solution, the name of the corresponding project is included in the caption line of the code sample.



## Upgrading from PDFKit.NET 4.0

PDFKit.NET 5.0 is fully backwards compatible. Simply replace the assembly and recompile.



## Please send us Feedback!

Please help us improve this document. If you have any questions, suggestions or find anything in this document is incorrect or missing then let us know at <a href="mailto:support@tallcomponents.com">support@tallcomponents.com</a>.



## Visit us Online

Visit our web site: <a href="http://www.tallcomponents.com" title="Optional alternate text">http://www.tallcomponents.com</a>.



## Naming Convention

We try to adhere as much as possible to Microsoft’s “Design Guidelines for Developing Class Libraries”. The guideline can be found here:


<a href="http://msdn2.microsoft.com/en-us/library/ms229042.aspx" title="Optional alternate text">http://msdn2.microsoft.com/en-us/library/ms229042.aspx</a>
