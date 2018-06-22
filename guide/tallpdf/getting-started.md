# Getting started


## Development environment

TallPDF.NET has been compiled and tested with Microsoft Visual Studio.NET 2010 and Microsoft .NET Framework 2.0 and 4.0. TallPDF.NET is a 100% managed .NET component and consists of just a single assembly: TallComponents.PDF.Layout.dll. TallPDF.NET can be used with any .NET application, including console applications, WinForms applications, ASP.NET web applications, ASP.NET web services and Windows services.



#### NET Framework 2.0 and 4.0

TallPDF.NET 4.0 has been compiled against version 2.0 and 4.0 of the .NET framework. For .Net 4.0 we provide a “normal” version as well as a Client Profile build and a WPF build.



#### 64-bit

All builds of TallPDF.NET 4.0 can be used on 64-bit machines.



#### Dependencies

TallPDF.NET does not depend on any third-party product other than the Microsoft .NET platform. It consists of just a single assembly.


The Microsoft .NET framework may be redistributed. It can be included in the installer for any product. For more information please read "<a href="http://msdn2.microsoft.com/en-us/library/xak0tsbd(en-US,VS.80).aspx">Redistributing the Microsoft .NET Framework</a>" at the <a href="https://msdn.microsoft.com/en-us/default.aspx">Microsoft MSDN website</a>.



#### Deployment

TallPDF.NET is fully compatible with the <a href="https://support.microsoft.com/en-us/kb/326355">XCopy deployment</a> principle of .NET. This means that installing TallPDF.NET is as simple as copying the single assembly to the installation folder of your application. For web applications this would be the /bin folder inside the virtual folder of your web site.


Although it is possible, there is no need to install TallPDF.NET in the <a href="https://msdn.microsoft.com/library?url=/library/en-us/cpguide/html/cpconglobalassemblycache.asp">GAC (Global Assembly Cache)</a>.



#### Classic ASP and COM

TallPDF.NET does not support the legacy technologies COM and ASP out of the box. However, the .NET Framework allows you to call .NET assemblies from a COM component. See "<a href="https://msdn.microsoft.com/library?url=/library/en-us/dndotnet/html/callnetfrcom.asp">Calling a .NET Component from a COM Component</a>" for more information.



#### Visual Studio .NET IntelliSense (C#/VB.NET)

The TallPDF.NET 4.0 installation includes the XML document file TallComponents.PDF.Layout.xml which is generated from our C# comments. If this file is in the same folder as the TallComponents.PDF.Layout.dll that is referenced from your project, TallPDF.NET documentation is added to Visual Studio .NET IntelliSense as shown in the following figure.

![visual-studio-.NET-code-intellisense](guide/tallpdf/media/visual-studio-NET-code-intellisense.png)

#### Visual Studio .NET IntelliSense (XML)

The TallPDF.NET 4.0 installation includes the TallPDF.NET 4.0 XML schema file TallComponents.PDF.Layout.xsd. Copy this file to the following location:
&nbsp;<ul><li>
<program files (x86)>\Microsoft Visual Studio <version>\Xml\Schemas</li></ul>&nbsp;
Now you can select it as the targetSchema in the Properties view of Visual Studio .NET after which IntelliSense support is added.

![select-the-TallPDF.NET-4.0-XML-schema-file](guide/tallpdf/media/select-the-TallPDF-NET-40-XML-schema-file.png)

![visual-studio-.NET-XML-intellisense](guide/tallpdf/media/visual-studio-NET-XML-intellisense.png)

## Hello World Console Application – C#

To create a Console Application in C# that generates a PDF document, do the following:
&nbsp;<ol><li>
Start Visual Studio .NET.</li><li>
Select menu item File > New > Project.</li><li>
From the New project dialog, select project type ‘Visual C#’ and project template ‘Console application’.</li><li>
Enter a name for the project and the folder to store the project, then click Ok.</li><li>
A default project is created.</li><li>
Select menu item Project > Add Reference…</li><li>
Select the Browse… tab, select TallComponents.PDF.Layout.dll and click Ok.</li><li>
Open Program.cs or main.cs from the solution explorer.</li><li>
Replace the code in this file with the code below.</li><li>
Save, compile and run the application.</li><li>
Open the newly generated PDF document.</li></ol>&nbsp;
```
using System;
using System.IO;

using TallComponents.PDF.Layout;
using TallComponents.PDF.Layout.Paragraphs;

class Hello
{
   static void Main( string[] args )
   {
      // create a document
      Document doc = new Document();

      // add a section
      Section section = new Section();
      doc.Sections.Add( section );

      // add a text paragraph
      TextParagraph textParagraph = new TextParagraph();
      section.Paragraphs.Add( textParagraph );

      // add a text fragment
      Fragment fragment = new Fragment();
      fragment.Text = "Hello world!";
      textParagraph.Fragments.Add( fragment );

      // save document as PDF to disk
      using ( FileStream fs = new FileStream(
         “hello.pdf”, FileMode.Create ) )
      {
         doc.Write( fs );
      }
   }
}
```

Code sample: Hello World Console Application in C#



## Hello World Console Application – VB.NET

To create a Console Application in VB.NET that generates a PDF document, do the following:
&nbsp;<ol><li>
Start Visual Studio .NET.</li><li>
Select menu item File > New > Project.</li><li>
From the New project dialog, select project type ‘Visual Basic’ and project template ‘Console application’.</li><li>
Enter a name for the project and the folder to store the project, then click Ok.</li><li>
A default project is created.</li><li>
Select menu item Project > Add Reference…</li><li>
Select the Browse… tab, select TallComponents.PDF.Layout.dll and click Ok.</li><li>
Open Module1.vb from the solution explorer.</li><li>
Replace the code in this file with the code below.</li><li>
Save, compile and run the application.</li><li>
Open the newly generated PDF document.</li></ol>&nbsp;
```
Imports System.IO

Imports TallComponents.PDF.Layout
Imports TallComponents.PDF.Layout.Paragraphs

Module HelloWorld
   Sub Main()
      ' create a document
      Dim doc As New Document

      ' add a section
      Dim section As New Section
      doc.Sections.Add(section)

      ' add a text paragraph
      Dim textParagraph As New TextParagraph
      section.Paragraphs.Add(textParagraph)

      ' add a text fragment
      Dim fragment As New Fragment
      fragment.Text = "Hello world!"
      textParagraph.Fragments.Add(fragment)

      ' save document as PDF to disk
      Using fs As FileStream = New FileStream("hello.pdf", FileMode.Create)
         doc.Write(fs)
      End Using
   End Sub
End Module
```

Code sample: Hello World Console Application in VB.NET



## Hello World ASP.NET Application – C#

To create an ASP.NET Application in C# that generates a PDF document, do the following:
&nbsp;<ol><li>
Start Visual Studio .NET.</li><li>
Select menu item File > New > Project.</li><li>
From the New project dialog, select project type ‘Visual C#’ and project template ‘ASP.NET Web Application’.</li><li>
Enter a name for the project and the folder to store the project, then click Ok.</li><li>
A default project is created.</li><li>
Select menu item Project > Add Reference…</li><li>
Select the Browse… tab, select TallComponents.PDF.Layout.dll and click Ok.</li><li>
Open Default.aspx and delete all code except the first line: the @Page directive.</li><li>
Open Default.aspx.cs and replace all code with the code below.</li><li>
Save, compile and run the application.</li><li>
The PDF opens in the web browser.</li></ol>&nbsp;
```
using System;

using TallComponents.PDF.Layout;
using TallComponents.PDF.Layout.Paragraphs;

namespace WebApplication1
{
   public class MyPage : System.Web.UI.Page
   {
      protected void Page_Load(object sender, EventArgs e)
      {
         // create document
         Document doc = new Document();

         // add a section
         Section section = new Section();
         doc.Sections.Add(section);

         // add a text paragraph
         TextParagraph textParagraph = new TextParagraph();
         section.Paragraphs.Add(textParagraph);

         // add a text fragment
         Fragment fragment = new Fragment();
         fragment.Text = "Hello world!";
         textParagraph.Fragments.Add(fragment);

         // save the PDF directly to the browser
         doc.Write(Response);
      }
   }
}
```

Code sample: ASP.NET Web Application in C#



## Hello World ASP.NET Application – VB.NET

To create an ASP.NET Application in C# that generates a PDF document, do the following:
&nbsp;<ol><li>
Start Visual Studio .NET.</li><li>
Select menu item File > New > Project.</li><li>
From the New project dialog, select project type ‘Visual Basic’ and project template ‘ASP.NET Web Application’.</li><li>
Enter a name for the project and the folder to store the project, then click Ok.</li><li>
A default project is created.</li><li>
Select menu item Project > Add Reference…</li><li>
Select the Browse… tab, select TallComponents.PDF.Layout.dll and click Ok.</li><li>
Open Default.aspx and delete all code except the first line: the @Page directive.</li><li>
Open Default.aspx.vb and replace all code with the code below.</li><li>
Save, compile and run the application.</li><li>
The PDF opens in the web browser.</li></ol>&nbsp;
```
Imports TallComponents.PDF.Layout
Imports TallComponents.PDF.Layout.Paragraphs

Public Class MyPage
   Inherits System.Web.UI.Page

   Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) _ 
      Handles Me.Load

      ' create a document
      Dim doc As New Document

      ' add a section
      Dim section As New Section
      doc.Sections.Add(section)

      ' add a text paragraph
      Dim textParagraph As New TextParagraph
      section.Paragraphs.Add(textParagraph)

      ' add a text fragment
      Dim fragment As New Fragment
      fragment.Text = "Hello world!"
      textParagraph.Fragments.Add(fragment)

      ' save the PDF directly to the browser
      doc.Write(Response)
   End Sub
End Class
```

Code sample: ASP.NET Web Application in VB.NET



## Further reading
&nbsp;<ul><li>
Topic "<a href="http://msdn2.microsoft.com/en-us/library/xak0tsbd(en-US,VS.80).aspx">Redistributing the Microsoft .NET Framework</a>" at the <a href="https://msdn.microsoft.com/en-us/default.aspx">Microsoft MSDN site</a>.</li><li>
Topic "<a href="https://support.microsoft.com/en-us/kb/326355">How to deploy an ASP.NET Web application using Xcopy deployment</a>" at the <a href="https://support.microsoft.com/en-us">Microsoft Support site</a>.</li><li>
Topic "<a href="https://msdn.microsoft.com/library?url=/library/en-us/cpguide/html/cpconglobalassemblycache.asp">Global Assembly Cache</a>" at the <a href="https://msdn.microsoft.com/en-us/default.aspx">Microsoft MSDN site</a>.</li><li>
Topic "<a href="https://msdn.microsoft.com/library?url=/library/en-us/dndotnet/html/callnetfrcom.asp">Calling a .NET Component from a COM Component</a>" at the Microsoft MSDN site.</li></ul>&nbsp;
