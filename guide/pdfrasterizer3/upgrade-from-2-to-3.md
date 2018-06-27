# Upgrade from 2.1

PDFRasterizer.NET 3.0 introduces a few breaking changes. This means that you may need to rewrite your client code. The following paragraphs discuss all the changes.



## LicenseAttribute.AddLicense

Method LicenseAttribute.AddLicense() has been removed. You should now use LicenseCollection.Add() instead.



## RenderSettings.CMapFolder

Static property RenderSettings.CMapFolder has been moved to TextRenderSettings.CMapFolder.



## 3ColorRenderSettings.ColorProfile

Property ColorRenderSettings.ColorProfile has been renamed into ColorRenderSettings.OutputColorProfile.



## FontSubstitutionMap.SearchPath

Property FontSubstitutionMap.SearchPath has been moved and renamed into TextRenderSettings.FontSearchPath. Notice that the Save and Load from the FontSubsitutionMap will now persist / restore that property.



## DocumentInfo

Properties Document.Author, CreationDate, Creator, Keywords, Producer, Subject and Title have been moved into the new DocumentInfo class.



## Summary

Page.Draw(), now returns null (previously a Summary object). Overloads have been introduced that accept a Summary argument.


Summary class is now located in namespace TallComponents.PDF.Rasterizer.Diagnostics.



## Page.Rotate

Property Page.Rotate (int) has been replaced by Page.Orientation (enum).



## PageCollection

Class Pages has been renamed to PageCollection.



## Rectangle constructor

Rectangle constructor removed, this was accidentially added.



## ConvertToImageOptions

Class ConvertToImageOptions has been removed. Property ConvertToImageOptions.Resolution has been moved to the ConvertToTiffOptions class.


