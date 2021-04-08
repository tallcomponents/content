# Document Structure

The top-level structure of a PDF document is shown below.

![document-structure](/guide/pdfkit/media/document-structure.png)

You can either open an existing document or create a new document as shown in the following code sample:

```
using System;
using System.IO;
using TallComponents.PDF;

// create a new empty document
Document document1 = new Document();

// open an existing document
using ( FileStream file = new FileStream(
    "report.pdf",
       FileMode.Open, FileAccess.Read ) )
{            
Document document2 = new Document( file );
}
```

Code sample: Create a new document or open an existing document

## Pages

The property Pages of class Document allows you to enumerate, remove and insert pages. Chapter [Actions](actions) discusses pages in more detail.

## Fields

The property Fields of class Document allows you to enumerate, remove and insert form fields. Fields are discussed in more detail in [Forms](forms).

## Bookmarks

A PDF document has a bookmarks tree which is also known as the document outline or table of contents. The top-level bookmarks can be accessed through the Document.Bookmarks property. From here you can enumerate the entire tree. You can also add, insert and remove bookmarks and modify bookmark properties. Bookmarks are discussed in more detail in [Navigation](navigation).

## Viewer Preferences

The viewer preferences define how a PDF reader should display a PDF document initially. Viewer preferences are discussed in more detail in [Navigation](navigation).

## JavaScript

Just like you can embed JavaScript in an HTML page for things like input validation and dynamically changing element properties, you can embed JavaScript in a PDF document for similar purposes. In fact, if you set the input mask on a text field, some JavaScript will be inserted on the fly to make this happen.

## Document Info

The document info consists of simple properties such as Author, Creator and Subject. The following snippet dumps part of the document info of a given document to the Console:

```
// open file 
using ( FileStream file = new FileStream(
  "my.pdf", FileMode.Open, FileAccess.Read ) )
{
  // open PDF document
  Document document = new Document( file );
  // dump info to console
  Console.WriteLine("author: {0}", document.DocumentInfo.Author );
  Console.WriteLine("subject: {0}", document.DocumentInfo.Subject );
  Console.WriteLine("title: {0}", document.DocumentInfo.Title );
}
```

Code sample: Dump the PDF document info to the console (DumpInfo)

## Metadata

Metadata schemas are embedded as XML documents. This data is often inserted by PDF producers to preserve metadata such as high-level layout information. A metadata schema is a collection of name-value pairs. A value is of type MetadataValue which is an abstract base class. The only concrete specializations are MetadataString and UnsupportedMetadataValue. We will add support for other types than string in future updates.

## Security

The Document.Security property gives you access to the security settings of the document. The property is of type Security which is an abstract base class. The only concrete derived class is PasswordSecurity. We anticipate to provide more and allow you to write custom security handlers – hence this approach. You can assign securty settings like this:

```
// open file 
using ( FileStream fileIn = new FileStream( 
  "guide.pdf",
  FileMode.Open, FileAccess.Read ) )
{
  // open source PDF
  Document document = new Document( fileIn );

  // assign owner and user passwords
  PasswordSecurity security = new PasswordSecurity();
  security.OwnerPassword = "1234";
  security.UserPassword = "GoodNews";
  document.Security = security;

  // save as a new PDF document
  using ( FileStream fileOut = new FileStream( 
    "guide_protected.pdf", 
    FileMode.Create, FileAccess.Write ) )
  {
    document.Write( fileOut );
  }
}
```

Code sample: Save PDF document with passwords (AssignPassword)


