# Fonts

The appearance of text on a PDF page is defined by the associated font. The PDF format allows embedding the entire or partial font program so that the appearance can always be reproduced independently of what fonts are installed on the client machine. It is however also possible to not embed the font program. It is then up to the PDF viewer to decide how to render the text. In general this is done by selecting a font on the machine from the name and parameters of the reference font. This process is called font substitution. PDFRasterizer.NET has a default substitution scheme, but it also allows you to decide what font to substitute.

Central to controlling the rendering of text is the `TextRenderSettings` class which can be accessed through the `RenderSettings.TextSettings` property.

## Font search path

The TextRenderSettings has a property FontSearchPath which is a semi-colon separated list of folders to search for font files that are included in the substitution map and in the ResolveFont event. The folders are searched from left to right until the font file is found. The default value is `.;%FONTDIR%`. This means that we will first look in the current folder and then in the Windows fonts folder.

## Font substitution map

One way to control font substitution for non-embedded fonts is to add entries to the `FontSubstitutionMap` property of the `TextRenderSettings` class. Note that the map is only used for fonts that are not embedded. This is demonstrated by the following code:

``` csharp
var settings = new RenderSettings();
settings.TextSettings.FontSubstitutionMap.Add(
  "TallComponents Demo Font", "ariali.ttf");
settings.TextSettings.FontSubstitutionMap.Add(
  "TallComponents Demo Font Bold", "impact.ttf");  
```

### Default substitution font

If a font is not embedded and no entry is found in the map, then the property FontSubstitutionMap. DefaultSubstitutionFont indicates what font to use. The value of this property is again the file name of a font program that is found with the help of the SearchPath property. The default value is “times.ttf”.

## ResolveFont event

An alternative to adding entries to the FontSubstitutionMap is to handle the ResolveFont event. In contrast to the font substitution map, this event will be raised for all fonts, including those that are embedded.

``` csharp
var settings = new RenderSettings();
settings.TextSettings.ResolveFont += new ResolveFontEventHandler(ResolveFont);

static void ResolveFont(TextRenderSettings sender, ResolveFontEventArgs args)
{
  // map all non-system fonts to the system font courier
  if (args.FontLocation != FontLocation.System)
  {
    if (args.PdfFontName.EndsWith("Bold"))
      args.Bold = true;
    args.SystemFontName = "courier";
  }
}
```

## CMaps

CMaps are used when displaying some fonts, mostly Chinese, Japanese or Korean (also known as CJK fonts). The CMaps are part of the zip achive that can be downloaded from our website. You should copy the folder \Support\CMaps to your deployment environment and tell PDFRasterizer.NET 3.0 where they can be found. You can do this by assigning the path of folder CMaps to the static property TextRenderSettings.CMapFolder or by handling the event TextRenderSettings.ResolveCMap. The event allows you to provide each individual CMap as a System.IO.Stream so that you may pull the CMap from e.g. a database or load a resource from an assembly.

## Font styles

Windows has the concept of font styles. So a font may be regular, bold, italic or bold-italic. The PDF format does not share this concept. A PDF document may ofcourse use bold or italic fonts but if the font description inside the PDF document does not explicitly contain this information other than that the name bold or italic is part of the PDF font name by convention.