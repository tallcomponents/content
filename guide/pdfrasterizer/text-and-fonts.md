# Text and Fonts

The appearance of a fragment of text in a PDF document is defined by its font. The PDF format allows embedding the entire or partial font program so that the appearance can always be reproduced on any machine independent of what fonts are installed on the client machine. It is however also possible to omit the font program. In this case, it is up to the PDF consumer to decide how to render the text. In general this is done by selecting its own font that matches the parameters (most importantly the name) of the referenced font. This process is called font substitution. PDFRasterizer.NET 3.1 has its own default substitution scheme but it also allows you to hook into this process.


Central to controlling the rendering of text is the `TextRenderSettings` class which can be accessed through the `RenderSettings.TextSettings` property.



## Font search path

The TextRenderSettings has a property FontSearchPath which is a semi-colon separated list of folders to search for font files that are included in the substitution map and in the ResolveFont event. The folders are searched from left to right until the font file is found. The default value is “.;%FONTDIR%”. This means that we will first look in the current folder and then in the Windows fonts folder.



## Font substitution map

One way to control font substitution for non-embedded fonts is to add entries to the FontSubstitutionMap property of the TextRenderSettings class. This is demonstrated in the following code:


```
using (FileStream file = new FileStream(
   @"FontDemo.pdf", FileMode.Open, FileAccess.Read))
{
   Document document = new Document(file);
   Page page = document.Pages[0];

   using (Bitmap bitmap = new Bitmap((int)page.Width, (int)page.Height))
   {
      RenderSettings renderSettings = new RenderSettings();
      renderSettings.TextSettings.FontSubstitutionMap.Add( 
         "TallComponents Demo Font", "ariali.ttf" );
      renderSettings.TextSettings.FontSubstitutionMap.Add( 
         "TallComponents Demo Font Bold", "arialbi.ttf" );

      Graphics graphics = Graphics.FromImage(bitmap);
      page.Draw(graphics, renderSettings);
      bitmap.Save(@"FontDemo.png", ImageFormat.Png);
   }
}
```

Code sample: Implement font substitution through the FontSubstitutionMap


If you open the PDF document using the Adobe PDF Reader and hit CTRL+D and then go to the Fonts tab, you will see the following fonts listed:

![Example](/guide/pdfrasterizer/media/Example.png)

The first argument to the FontSubstitutionMap.Add method call is the name of the font in the PDF document. The second argument is the file name of the font program that should be used to render the font.

#### Default substitution font

If a font is not embedded and no entry is found in the map, then the property FontSubstitutionMap. DefaultSubstitutionFont indicates what font to use. The value of this property is again the file name of a font program that is found with the help of the SearchPath property. The default value is “times.ttf”.



## ResolveFont event

An alternative to adding entries to the FontSubstitutionMap is to handle the ResolveFont event. In contrast to the font substitution map, which only plays a role for non-embedded fonts, this event will be raised for all fonts.


The first time that a particular PDF font is encountered, the system will try to resolve the font, and then raise this event to inform the client which device font it has found. The Location property of the ResolveFontEventArgs object specifies whether a font definition has been found, and if so where it has been found.


The following code sample does not change anything but dumps the name of each font and the value of the Location property:


```
static void Main(string[] args)
{
   using (FileStream file = new FileStream(
      @"FontDemo.pdf", FileMode.Open, FileAccess.Read))
   {
      Document document = new Document(file);
      Page page = document.Pages[0];

      using (Bitmap bitmap = new Bitmap((int)page.Width, (int)page.Height))
      {
         RenderSettings renderSettings = new RenderSettings();
         renderSettings.TextSettings.ResolveFont += 
            new ResolveFontEventHandler(resolveFont);

         Graphics graphics = Graphics.FromImage(bitmap);
         page.Draw(graphics, renderSettings);
      }
   }
}

static void resolveFont(object sender, ResolveFontEventArgs args)
{
   Console.WriteLine("{0} ({1})", args.PdfFontName, args.Location);
}
```

Code sample: Use the ResolveFont event to dump the name and location of each font


If the event handler does not change any properties of this event, the system will use the definition as indicated by the Location propery. If the font has been resolved this is often the right thing to do. If the font is unresolved the system will use the default font as specified in the FontSubstitutionMap.


By changing one or more event properties, the event handler can change this behavior. Not only is it possible to map unresolved fonts to different fonts than the default, but it is also possible to map resolved fonts to different font definitions.


The following code does the exact same thing as Code sample Implement font substitution through the FontSubstitutionMap that uses the FontSubstitutionMap to map fonts. For brevity, only the event handler is listed.


```
static void resolveFont(object sender, ResolveFontEventArgs args)
{
   if (args.Location == FontLocation.Unresolved)
   {
      switch (args.PdfFontName)
      {
         case "TallComponents Demo Font":
            args.FontPath = "ariali.ttf";
            break;
         case "TallComponents Demo Font Bold":
            args.FontPath = "arialbi.ttf";
            break;
      }
   }
}
```

Implement font substitution through the ResolveFont event


Changing the font handling for fonts that have been resolved may make sense in some cases. Fonts that have their FontRenderMode set to RenderAsCurves increase the size of print jobs and the size of WPF output. Therefore, one may choose to have these fonts rendered by an appropriate system font. This is merely a matter of setting the SystemFontName of the event to the appropriate font name. This is demonstrated in the following code sample:


```
void resolveFont(object sender, ResolveFontEventArgs args)
{
   if (args.FontRenderMode == FontRenderMode.RenderAsCurves)
   {
      switch (args.PdfFontName)
      {
         case "Times-Roman":
            args.SystemFontName = "Times New Roman";
            break;
         case "Times-Bold":
            args.SystemFontName = "Times New Roman";
            args.Bold = true;
            break;
         case "Times-Italic":
            args.SystemFontName = "Times New Roman";
            args.Italic = true;
            break;
      }
   }
}
```

Code sample: Render well-known fonts as system fonts instead of as curves


If the system is able to resolve the font, based on the information provided by the event handler, it will simply use that font. If for some reason the font cannot be resolved, or if it cannot be used as requested, a new ResolveFont event will be raised.



## CMaps

CMaps are used when displaying some fonts, mostly Chinese, Japanese or Korean (also known as CJK fonts). The CMaps are part of the zip achive that can be downloaded from our website. You should copy the folder \Support\CMaps to your deployment environment and tell PDFRasterizer.NET 3.0 where they can be found. You can do this by assigning the path of folder CMaps to the static property TextRenderSettings.CMapFolder or by handling the event TextRenderSettings.ResolveCMap. The event allows you to provide each individual CMap as a System.IO.Stream so that you may pull the CMap from e.g. a database or load a resource from an assembly.



## Font styles

Windows has the concept of font styles. So a font may be regular, bold, italic or bold-italic. The PDF format does not share this concept. A PDF document may ofcourse use bold or italic fonts but if the font description inside the PDF document does not explicitly contain this information other than that the name bold or italic is part of the PDF font name by convention.



## Code samples

The following code samples asre included in the distribution and are relevant to font handling:
&nbsp;<ul><li>
FontMapping</li><li>
UseOSFont</li></ul>&nbsp;
