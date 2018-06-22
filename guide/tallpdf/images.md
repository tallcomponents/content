# Images

The Image class is a specialization of paragraph. So all members of paragraph are members of image as well. Like any paragraph, an image can be added to a section, table cell, area, header and footer. Image is a very simple class. It allows you to add a raster images like a BMP or JPEG image to the document.



## Size

You can set the Width and Height properties to control the size. Furthermore, if the KeepAspectRatio property is set (default), the height property is calculated from the width and vice versa. If you do not explicitly set the size, the maximum width is used. If an image does not fit in the available/given space, by default an exception is generated. Set the property FitPolicy to FitPolicy.Shrink to force resizing the image in this case.


If both the height and width are set, the image will be stretched to fit in the given area. The KeepAspectRatio flag is honored when resizing, and the image will be best-fit inside the specified area.



## Image Source

An Image object can be constructed from a file path, a URL, a System.IO.Stream and a System.Drawing.Bitmap.



#### Path

You set the path of the image either through the constructor or through the 'Path' property. The path can either be a file on disk or a URL. If the path points to a file on disk, it can be either virtual (ASP.NET), absolute or relative.


The following code samples demonstrate different scenarios.


```
Image image = new Image("tc-logo.gif");
```

Code sample: Construct an image from a relative file path (C#)


```
Image image = new Image(@"c:\images\tc-logo.gif");
```

Code sample: Construct an image from an absolute file path (C#)


```
Image image = new Image("http://www.tallcomponents.com/img/tc-logo.gif");
```

Code sample: Construct an image from a URL (C#)



#### System.Drawing.Bitmap

Sometimes you want to use a bitmap that is created in memory using the GDI+ image classes. In this case you can construct an Image object from a System.Drawing.Bitmap object.


Depending on what constructor overload is used, it is the responsibility of either the caller or callee to dispose the bitmap object. If the caller disposes, then the bitmap should not be disposed before Document.Write returns.


The following code samples demonstrate different scenarios.


```
using (Bitmap bitmap = new Bitmap("tc-logo.gif"))
{
   Image image = new Image(bitmap);
   section.Paragraphs.Add(image);

   using (FileStream file = new FileStream("out.pdf", FileMode.Create))
   {
      document.Write(file);
   }
}
```

Code sample: Construct an image from a Bitmap – caller disposes (C#)


```
System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap("tc-logo.gif");
Image image = new Image(bitmap, true);
section.Paragraphs.Add(image);

using (FileStream file = new FileStream("out.pdf", FileMode.Create))
{
   document.Write(file);
}
```

Code sample: Construct an image from a Bitmap – callee disposes (C#)



#### System.IO.Stream

In some cases you want to construct the image from a System.IO.Stream instance. A typical scenario is when the image is stored as a binary blob in a database. In this case you get the data from the database, wrap this in a MemoryStream and construct the Image from this stream.


Depending on what constructor overload is used, it is the responsibility of either the caller or callee to close the stream object. If the caller is responsible then the stream should not be closed before Document.Write returns.


The following code sample demonstrates a typical scenarios.


```
Document document = new Document();
Section section = document.Sections.Add();

using ( SqlConnection connection = new SqlConnection( "..." ) )
{
   connection.Open();

   string query = "SELECT data FROM images WHERE id=3";
   SqlCommand cmd = new SqlCommand( query, connection );
   byte[] blob = (byte[]) cmd.ExecuteScalar();

   Image image = new Image( new MemoryStream(blob) );
   section.Paragraphs.Add(image);
}

using (FileStream file = new FileStream("out.pdf", FileMode.Create))
{
   document.Write(file);
}
```

Code sample: Construct an image from a memory stream (C#)



## Multi-frame images

For all constructor overloads that were discussed above, an additional overload exists that takes an extra integer argument. This is used as the frame index in case the image has multiple frames. This is typically the case in multi-page TIFF images. There is also a FrameIndex property to select the frame after construction. Finally there is a FrameCount property that returns the number of frames in the image.


