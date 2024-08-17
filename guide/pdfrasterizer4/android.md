# Render to Android ImageView widget

The following code renders a PDF to an Xamarin Android ImageView widget with resource ID imageView1:

``` csharp
private void RenderPage(Stream inputStream, int pageNum)
{
  if (screenWidth == 0 || screenHeight == 0)
  {
    screenHeight = Resources.DisplayMetrics.HeightPixels;
    screenWidth = Resources.DisplayMetrics.WidthPixels;
  }

  using (var outputStream = new MemoryStream())
  {
    AppCompatImageView imageView = FindViewById<AppCompatImageView>(Resource.Id.imageView1);

    Document document = new Document(inputStream);
    Page page = document.Pages[pageNum];

    float dpi = 72;

    double scale1 = Math.Max(screenWidth, screenHeight) / Math.Max(page.Width, page.Height);
    double scale2 = Math.Min(screenWidth, screenHeight) / Math.Min(page.Width, page.Height);

    dpi = (float)Math.Max(scale1, scale2) * 72;

    page.SaveAsBitmap(outputStream, ImageEncoding.Png, dpi);

    Bitmap bmp = BitmapFactory.DecodeByteArray(outputStream.GetBuffer(), 0, (int)outputStream.Length);

    imageView.SetImageBitmap(bmp);
  }
}
```
