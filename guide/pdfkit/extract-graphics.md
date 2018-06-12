# Extract Graphics

As of version 3.0, it is possible to extract the existing graphics on a page as a collection of ContentShape objects. This collection and its items can be navigated, inspected, modified and persisted. This allows you to actually change the PDF content.



## Page.CreateShapes

The central method to extracting graphics is Page.CreateShapes. The following code copies all content except the images from an existing document to a new document:


```
static void Main(string[] args)
{
   using (FileStream fileIn = new FileStream(
      "in.pdf", FileMode.Open, FileAccess.Read))
   {
      Document pdfIn = new Document(fileIn);
      Document pdfOut = new Document();

      foreach ( Page page in pdfIn.Pages)
         pdfOut.Pages.Add(copy(page));

      using (FileStream fileOut = new FileStream(
         "out.pdf", FileMode.Create, FileAccess.Write))
      {
         pdfOut.Write(fileOut);
      }
   }
}

static Page copy(Page page)
{
   Page newPage = new Page(page.Width, page.Height);

   ShapeCollection shapes = page.CreateShapes();
   purge(shapes);
   newPage.Overlay.Add(shapes);

   return newPage;
}

static void purge(ShapeCollection shapes)
{
   for(int i=0; i<shapes.Count; i++)
   {
      Shape shape = shapes[i];
      if (shape is ImageShape) { shapes.RemoveAt(i); i--; }
      if (shape is ShapeCollection) purge(shape as ShapeCollection);
   }
}
```

Code sample: Copy content except images from an existing document to a new document



## Shape types

Only the following shape types are returned:
&nbsp;<ul><li>
Structure shapes: ShapeCollection, ClipShape and LayerShape</li><li>
Path shapes: FreeHandShape and LineShape</li><li>
Graphics shapes: TextShape and ImageShape</li></ul>&nbsp;
Note that multiline text is not converted to a MultilineTextShape object and that paths are not converted to EllipseShape or RectangleShape objects.



## Transformations

The Transform property of each ContentShape is set to an object of type TransformCollection. The items inside this collection are always of type MatrixTransform. Each transformation inside a PDF document is preserved as a single MatrixTransform so they are not collapsed.



## Restrictions

The following features are not preserved while extracting graphics since they are not yet supported by the Shape object model:
&nbsp;<ul><li>
Complex transparencies will be reduced to a single opacity value of type double (ContentShape.Opacity).</li><li>
When modifying the TextShape.Text property, you need to be aware that if it uses a subsetted font, it may not be able to display the new text. This is the case if characters are used for which no representation exists in the subsetted font.</li><li>
Shading types FunctionBasedShading (Type1), FreeFormGouraudShaded (Type4), LatticeFormGouraudShaded (type5), CoonsPatchMeshes (type6) and TensorProductPatchMeshes (type7) shadings are not supported. These are mapped to no fill.</li><li>
The functions of shading types Axial Shading (type2) and RadialShading(type3) are converted to an array of color stops to achieve a similar visual effect.</li><li>
Not all PDF color spaces are supported. Especially CIE based color spaces, DeviceN, Indexed and Pattern. These are converted to RGB to achieve a similar visual effect.</li></ul>&nbsp;
