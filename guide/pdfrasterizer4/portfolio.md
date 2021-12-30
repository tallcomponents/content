# PDF portfolio

A PDF package or portfolio is a collection of multiple documents packaged together into one PDF file. 
The documents can be of different file formats, including PDF itself. 
PDF portfolios allows you to keep multiple files together for distribution or archiving.

The following code sample shows how to enumerate embedded files (if any) and recognize PDF documents. 
Once a PDF document is extracted, it can be rendered as a regular PDF document as discussed elsewhere in this guide: 
- [Convert to TIFF](render-to-tiff)
- [Save as bitmap](render-to-bitmap)
- [Render to Skia surface](skia-surface)
- [Render to Xamarin Android widget](android)
- [Render to Xamarin iOS view](ios)

``` csharp
using (var stream = new Document(new FileStream(path, FileMode.Open, FileAccess.Read))
{
  var portfolio = new Document(stream);
  foreach (var file in portfolio.EmbeddedFiles)
  {
    if (file.MimeType == "application/pdf" || 
        file.MimeType == null && ".pdf" == Path.GetExtension(file.FileName))
    {
      using (var ms = new MemoryStream())
      {
        file.Write(ms);
        var embedded = new Document(ms);
        // ... render from here
      }
    }
  }
}
```
