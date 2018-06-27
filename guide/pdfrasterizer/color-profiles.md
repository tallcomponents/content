# Color Profiles

PDFRasterizer.NET 3.0 allows you to specify an output color profile that defines how colors are transformed to RGB. Currently, RGB is the only supported output color space.



## Color transformation

Conceptually, colors are rendered to RGB in two steps. First the color is transformed to an absolute color, next the absolute color is transformed to the output RGB color. This process is controlled by properties ColorRenderSettings.TransformationMode and ColorRenderSettings.OutputColorProfile.


Property ColorRenderSettings.OutputColorProfile has a default color profile assigned. It can be overridden with your own profile using the following code:


```
  using (FileStream file = new FileStream("sRGB.icm", FileMode.Open, FileAccess.Read))
{
   ColorProfile profile = new ColorProfile(file);
   RenderSettings renderSettings = new RenderSettings();
   renderSettings.ColorSettings.ColorProfile = profile;
}
```

Code sample: Load custom output device color profile



## PDF color spaces

In PDF, each color is defined by a combination of the current color space and the current _color operands_. The number of operands depends on the current color space. E.g. if DeviceRGB is the current color space, then the color is defined by 3 operands that each specifiy the intensity of the R, G and B color channels, respectively.


There are three groups of color spaces: Device color spaces, CIE-based color spaces and Special color spaces.



#### CIE-based color spaces

The CIE-based color spaces are ICCBased, Lab, CalGray and CalRGB. A color specified in one of these color spaces is already an _absolute color_. The color space includes parameters that fully define how to transform the color operands in the CIE-based color space to the absolute color.


If the TransformationMode is set to HighQuality, the absolute color is transformed to the output RGB color as defined by the output color profile. Otherwise, an internal fast algorithm is used.



#### Device color spaces

The Device color spaces are DeviceRGB, DeviceGray and DeviceCMYK. These color spaces are named device color spaces because they correspond to the color space of a typical output device.


Color operands in the DeviceRGB color space are simply copied to the output RGB color operands so no transformation is applied. Propeties OutputColorProfile and TransformationMode are ignored.


The single operand (gray value) in the DeviceGray color space is copied to all three RGB color operands. Propeties OutputColorProfile and TransformationMode are ignored.


If the TransformationMode is set to HighQuality, then first the CMYK color is transformed to an absolute color using an internal CMKY color profile. Next the absolute color is transformed to the output RGB color as defined by the output color profile. If the TransformationMode is set to HighSpeed, then an internal fast algorithm is used.



#### Special color spaces

The Special color spaces are Indexed, Pattern, Separation and DeviceN. These color spaces have an underlying color space that is either a Device color space or a CIE-based color space. The transformation from the color operands in the Special color space to the equivalent operands in the underlying color space are fully defined by the parameters that define the Special color space. E.g. If the color space is Indexed, a single operand selects a set of color operands from a palette. These operands specify a color in the underlying color space. Next, the transformation of the underlying color space is applied.


