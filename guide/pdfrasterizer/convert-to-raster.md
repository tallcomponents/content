# Convert to Raster Formats

Although TIFF is also a raster format, we handle convering PDF documents to TIFF somewhat differently. This is described in [Convert to TIFF](convert-to-tiff).



## Render a single page to a bitmap

The following code renders the first page of a given document to a single bitmap (PNG) at 72 DPI:


```
using (FileStream file = new FileStream(
   @"document.pdf", FileMode.Open, FileAccess.Read))
{
   Document document = new Document(file);
   Page page = document.Pages[0];

   using (Bitmap bitmap = new Bitmap(
      (int)page.Width, (int)page.Height))
   {
      Graphics graphics = Graphics.FromImage(bitmap);
      graphics.SmoothingMode = SmoothingMode.AntiAlias;
      page.Draw(graphics);
      bitmap.Save(@"document_0.png", ImageFormat.Png);
   }
}
```


## Render multiple pages to a single bitmap

The following code renders all pages of a given document to a horizontal strip at 72 DPI:


```
using (FileStream file = new FileStream(
   @"document.pdf", FileMode.Open, FileAccess.Read))
{
   Document document = new Document(file);
   PageCollection pages = document.Pages;

   // calculate strip height and width
   int stripHeight=0, stripWidth=0;
   for (int i = 0; i  < pages.Count; i++) 
   {
      stripHeight = Math.Max(stripHeight, (int)pages[i].Height);
      stripWidth += (int)pages[i].Width;
   }

   using (Bitmap bitmap = new Bitmap(stripWidth, stripHeight))
   {
      Graphics graphics = Graphics.FromImage(bitmap);
      for (int i = 0; i  < pages.Count; i++) 
      {
         pages[i].Draw(graphics);
         // translate the graphics to the right so the next page
         // is drawn next to to the previous
         graphics.TranslateTransform((int)pages[i].Width, 0);
      }
      bitmap.Save(@"document_strip.png", ImageFormat.Png);
   }
}
```

Note the call to graphics.TranslateTransform. This call makes sure that the next page is drawn next to the previous page by translating the graphics object to the right by the amount of the page width.



## 4.3. Set the resolution

The resolution is expressed as the number of pixels per inch. The physical dimension of a PDF page is a given and returned by page.Width and page.Height in points (1/72 inch). The vertical resolution follows from the number of rows that a rendered page spans and the horizontal resolution follows from the number of columns that a rendered page spans.


If you do not explicitly transform the System.Drawing.Graphics object, then the resolution is 72 DPI. This means that each point corresponds to a single pixel because an inch equals 72 points.


The set the resolution to 300 DPI, you need 300/72 times more rows and 300/72 more columns. To achieve this you need to scale the Graphics object in both directions with the same factor.


This is demonstrated in the following code:


```
using (FileStream file = new FileStream(
   @"document.pdf", FileMode.Open, FileAccess.Read))
{
   Document document = new Document(file);
   Page page = document.Pages[0];

   // calculate the scale from the DPI
   int dpi = 300;
   float scale = (float)dpi / 72f;

   using (Bitmap bitmap = new Bitmap(
      (int)(page.Width*scale), (int)(page.Height*scale)))
   {
      Graphics graphics = Graphics.FromImage(bitmap);
      graphics.SmoothingMode = SmoothingMode.AntiAlias;
      // scale the graphics object
      graphics.ScaleTransform(scale, scale);
      page.Draw(graphics);
      bitmap.Save(@"document_0.png", ImageFormat.Png);
   }
}
```


## Select a raster output format

The raster format is selected when you write the System.Drawing.Bitmap to file using the following code:


```
bitmap.Save(@"document_0.png", ImageFormat.Png);
```

You can change the output format by passing another ImageFormat. E.g. the following line saves the bitmap to the lossy JPEG format:


```
bitmap.Save(@"document_0.png", ImageFormat.Jpeg);
```


## Code samples

The following code samples are included in the distribution and are relevant to converting PDF to ratser formats:
&nbsp;<ul><li>
ConvertToImage</li><li>
ViewPDF</li></ul>&nbsp;
