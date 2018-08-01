# Developer Guide

PDFRasterizer.NET is a 100% .NET (verifiable) solution for printing PDF documents unattended and converting PDF documents to a raster image, WPF graphics or an XPS document.

This developer guide will help you understand the PDFRasterizer.NET class library. It walks through the main concepts and illustrates them with code samples. 

## What is new

- Only medium or high trust required (was full trust in 2.1)
- Progressive drawing
- Cancellable drawing
- Asynchronous drawing
- Convert to (multi-page) color TIFF (was only Black&White in 2.1)
- LZW compression for TIFF output
- More advanced font substitution
- Convert to XPS
- Convert to WPF graphics
- Extract embedded files (also known as attachments)
- Layer support
- Diagnostics

## Licensing

PDFRasterizer.NET 3.0 runs either in licensed or unlicensed mode. It runs in licensed mode if a valid license key is registered.

In unlicensed mode, rendered pages are stamped with an evaluation notice. Extracted images have a red cross imposed. When converting to WPF, only the first page is converted normally. All other pages are rendered as a single raster image so no vector graphics are preserved.

## Restrictions

The following restrictions appy to PDFRasterizer.NET 3.0:

- Due to GDI+ and XPS restrictions, only the most basic form of transparency is supported
- Overprint and rendering intents are ignored
- Function-based shadings (type 1) are not supported
- Free-form Gouraud-shaded triangle meshes (type 4) are not supported
- Lattice-form Gouraud-shaded triangle meshes (type 5) are not supported
- Coons patch meshes (type 6) are not supported
- Tensor-product patch meshes (type 7) are not supported
- When printing, spool jobs may be very large due to conversion of glyphs to curves. This happens when there is no matching system font installed and no font substitution if configured. See also [Text and Fonts](text-and-fonts)
