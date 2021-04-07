# Convert XHTML to PDF

As of version 4.0 TallPDF.NET has extensive XHTML + CSS2 support. This is available through the XhtmlParagraph class. The XhtmParagraph class fully supports XHTML 1.0 Strict, XHTML 1.1 and CSS 2.1 including page breaks, forms and links.


XhtmlParagraph is a specialization of paragraph. Therefore, all members of paragraph are members of XhtmlParagraph as well. You can add it to a section, table cell, header or footer.


The following code snippets show how to use the XhtmlParagraph.


```
Document document = new Document();
Section section = document.Sections.Add();

// create an XhtmlParagraph with static content
XhtmlParagraph xhtml1 = new XhtmlParagraph();
xhtml1.Text = "<html><body><b><i>Hello world</i></b></body></html>";
section.Paragraphs.Add(xhtml1);

s// create an XhtmlParagraph from a URL
XhtmlParagraph xhtml2 = new XhtmlParagraph();
xhtml2.Path = @"http://www.yourserver.com/xhtml/path/file.htm";
section.Paragraphs.Add(xhtml2);

// convert to PDF and save
using (FileStream fs = new FileStream("out.pdf", FileMode.Create))
{
   document.Write(fs);
}
```

Code sample: Import XHTML in C#



## Disclaimer

XhtmlPargraph tries its best to convert any XHTML to PDF. The output may not be identical to that of a major browser. It is recommended to use XhtmlParagraph in a controlled environment where the features used by your XHTML documents can be tested before being used.


## XHTML vs HTML

The XhtmlParagraph expects XHTML as opposed to HTML. While XHTML is well-formed XML, HTML generally is not. Common differences are:

- All elements must be closed. Use either `<b>...</b>` or `<img ... />`
- Opening and closing tags must be correctly balanced: `<b>bold<i>bolditalic</b>italic</i>` should be: `<b>bold<i> bolditalic</i></b><i>italic</i>`
- All attribute values have to be enclosed in quotes: `<table border=3 width=300>` should be `<table border="3" width="300">`

## Conversion Settings

You can specify conversion settings that control different aspects of the XHTML conversion. These settings are specified in the XhtmlParagraph.Settings propery. The following code shows how to specify conversion settings using the ConversionSettings class:


```
XhtmlParagraph xhtmlParagraph = new XhtmlParagraph();
xhtmlParagraph.Settings = new ConversionSettings();
```

You can use the same ConversionSettings instance and assign it to multiple XhtmlParagraph instances. If you do not set the Settings property of an XhtmlParagraph, it will use default settings. The following paragraphs discuss the difference settings that are exposed by the ConversionSettings class.



#### Style Sheets

The CSS 2.1 specification states the following about the precedence of styles depending on their origin (in ascending order of precedence):
&nbsp;<ol><li>
user agent declarations</li><li>
user normal declarations</li><li>
author normal declarations</li><li>
author important declarations</li><li>
user important declarations</li></ol>&nbsp;
**User Agent Declarations**


The user agent declaration are incorporated in the converter itself. These are described in <a href="https://www.w3.org/TR/CSS21/sample.html">Appendix D, “Default style sheet for HTML 4”</a> of the CSS 2.1 Specification. E.g. one such style definition is:


```
strong { font-weight: bolder }
```

This defines the default meaning of the strong element.


**Author Declarations**


The _author normal declarations_ and _author important declarations_ are embedded or referenced in the XHTML document itself. Because the precedence of these declarations are higher than the user agent declarations, you can override the semantics of the strong element as follows:


```
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
<head>
<style type="text/css" id="internalStyle">
  strong { 
    color: green;
    font-family: monospace;
    font-weight: bold;
  }
</style>
</head>
<body>
<p>
  This is text that uses our 
  <strong>internal stylesheet</strong>.
</p>
</body>
</html>
```

A declaration is said to be _important_ if the delimiter token "!" and keyword "important" follow the declaration as follows:


```
<style type="text/css" id="Style1">
    p { color:green!important }
</style>
<p style="color:red">This text will be green.</p>
```

Otherwise, the declaration is _normal_.


**User Declarations**


The third origin of style sheets is user. TallPdf.NET allows you to specify user style sheets through the ConversionSettings.StyleSheets property. The following code shows how to add a user style sheet:


```
XhtmlParagraph xhtmlParagraph = new XhtmlParagraph();
xhtmlParagraph.Settings = new ConversionSettings();

CssStyleSheet styleSheet = new CssStyleSheet();
styleSheet.Text = "strong { color:red!important }";
xhtmlParagraph.Settings.StyleSheets.Add(styleSheet);
```

Just like the author declarations, user declarations can be _normal_ or _important_. This is determined by the use of the !important syntax. Note that the code sample above is a _user important declaration_ and has the highest precedence of all declarations.



#### UseDtd

If you set UseDtd to false, then the DTD that is referenced by the XHTML document will be ignored. The default value is true. If you set this property to false, then all XHTML 1.1 entities will be resolved by TallPdf.NET itself. Any entities in the XHTML document that are not part of the XHTML 1.1 standard will result in an exception. So you should only set UseDtd to false if you are absolutely certain that all used entities are covered by the XHTML 1.1 standard. Setting UseDtd to false will increase the speed of the converter.



#### FontPath

By default, the converter will search the system fonts folder for fonts that are referenced in the XHTML document. When running in a context such as ASP.NET it might not be allowed to access the Windows Fonts folder due to security restrictions. In that case you can set the FontPath-property to point to an accessible folder containing True-type fonts.


Property ConversionSettings.FontPath allows you to control this. If you set this property, then the converter will first look inside ConversionSettings.FontPath, next it will look inside the system fonts folder. The default value of FontPath is null.


As opposed to the rest of TallPDF.NET, fonts have to be specified in the XhtmlParagraph using the font family name.



#### BasePath

During conversion, the base path will be searched for loadable objects such as images and style sheets. This base path will be determined once before the conversion starts and then used throughout the conversion. It is determined as follows:
&nbsp;<ol><li>
If property ConversionSettings.BasePath has been set, then this path is used as the base path.</li><li>
If the above property is not set, then property Document.Path or Area.Path (depending on the context) will be used as the base path. (If you use a FileStream as the source of the XHTML document, then we will use the underlying path as the base path.)</li><li>
If the above property is not set (because you use a stream or verbatim text), then the current folder of the application will be used as the base path.</li></ol>

#### PDF Settings

Three properties of the ConversionSettings class control the PDF output. These are EnableForms, EnableLinks and EnableTooltips. All these properties default to true.


EnableForms converts HTML forms and fields into PDF fields.


EnableLinks converts HTML links (A element with an href attribute) into PDF links.


EnableTooltips converts HTML tooltips into PDF tooltips.



#### Credentials

Sometimes, a password is required to access a URL. You can specify such a password using the Credentials property as follows:


```
XhtmlParagraph xhtmlParagraph = new XhtmlParagraph();
xhtmlParagraph.Settings = new ConversionSettings();

NetworkCredential credential = new NetworkCredential("jim","1234");
xhtmlParagraph.Settings.Credentials = credential;
```


#### ContentFitBehavior

The ContentFitBehavior property lets you specify how the HTML content scales to the PDF page. These are the different values:


_DoNotScale_: The HTML will not be scaled. 96 pixels correspond to 72 points (96 DPI). Note that the content may run over the right edge of the page.


_SpecifyWidth_: The property ConversionSettings.FixedWidth specifies the number of pixels that the width of the XhtmlParagraph corresponds with.


_ScaleToWidth_: The HTML content is scaled to fit the width of the section.


_ScaleToSinglePage_: The HTML content is scaled to fit both the width and the height of the page. The XhtmlParagraph will never be larger than a single page.



#### CustomResourceLoader

This property lets you specify a resource loader that has the first opportunity to load a resource such as an image as specified by the <img> tag. You should create a class that implements the IResourceLoader interface and then assign an instance of this class to this property. Its method LoadResource will be called for each resource. Return null if you want the engine to handle the request.


The custom resource loader can be implemented for resources that require authentication.



## Restrictions

- Only a single form per document is supported
- Javascript and Dynamic content (HTML generated through JavaScript, events etc.) are not supported
- Aural features are not supported
- Direction and Unicode-bidi are not supported
