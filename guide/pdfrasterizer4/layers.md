# Layers

The PDF specification allows you to group graphics objects together in layers and turn visiblity selectively on and off in any combination. 
This is typically used in CAD drawing to show/hide levels of details. 
The following snapshot from the Adobe PDF Reader shows how the user can configure visibility of layers.

![Visibility of layers can be turned on and off](/guide/pdfrasterizer/media/visibility-of-layers-can-be-turned-on-and-off.png)

## Render all layers

PDFRasterizer.NET 4.0 lets you control which layers to render. The following code shows how:

``` csharp
using (var pdfFile = new FileStream(pdfPath, FileMode.Open, FileAccess.Read))
{
   var pdf = new Document(pdfFile);
   var page = pdf.Pages[0];

   var layers = pdf.Layers;
   
   // render all layers      
   foreach (var layer in layers)
   {
      layer.Visible = true;
   }

   using (var pngFile = new FileStream(pngPath, FileMode.Create))
   {
     page.SaveAsBitmap(pngFile, ImageEncoding.Png, 150f);
   }
}
```
## Clone Layers

The above code sample uses document.Layers to switch rendering of each layer on. 
This means that in principal the settings may change afterwards if a second thread modifies the document.Layers collection. 
You can avoid this by using RenderSettings.LayerSettings and cloning the layers as follows:

``` csharp
var settings = new RenderSettings();
settings.LayerSettings.Layers = pdf.Layers.Clone();
```

The following code sample shows how `RenderSettings` is used.

## Render each layer to a separate image

``` csharp
using (var pdfFile = new FileStream(pdfPath, FileMode.Open, FileAccess.Read))
{
  var pdf = new Document(pdfFile);
  var page = document.Pages[0];

  var settings = new RenderSettings();
  settings.LayerSettings.Layers = pdf.Layers.Clone();
   
  foreach (var l1 in pdf.Layers)
  {
    foreach (var l2 in settings.LayerSettings.Layers)
      l2.Visible = l2.Name == l1.Name;
  
    using (var pngFile = new FileStream($"{layer.Name}.png", FileMode.Create))
    {
      page.SaveAsBitmap(pngFile, ImageEncoding.Png, 150f, settings); 
    }
  }
}
```

## Default behavior

If LayerSettings.Layers equals null, the layer visibility settings as stored in the document will be used. In other words, the document will be rendered equal to how e.g. the Adobe PDF Reader will show the document initially.


