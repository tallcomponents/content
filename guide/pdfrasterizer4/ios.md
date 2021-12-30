Given the following Xamarin iOS MainPage.xaml (parts omitted):

``` xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="RenderPDF.MainPage">
            
  <!-- parts omitted -->

  <Image x:Name="image"
         Source="Default.png"
         Aspect="AspectFill"
         VerticalOptions="EndAndExpand" />

</ContentPage>
```

The following MainPage partial class (code omitted), renders the first page of embedded resource tiger.pdf to the Image above:

``` csharp
public partial class MainPage : ContentPage
{
  private Stream imageStream;
  private Stream pdfStream;
  private Document document;

  public MainPage()
  {
    InitializeComponent();
    
    LoadPDF();
  }

  public void LoadPDF()
  {
    if (pdfStream == null)
    {
      var assembly = typeof(MainPage).Assembly;
      using (Stream resourceStream = assembly.GetManifestResourceStream("RenderPDF.tiger.pdf"))
      {
        if (resourceStream != null)
        {
          pdfStream = new MemoryStream();
          resourceStream.CopyTo(pdfStream);
          
          RenderFirstPage();
        }
      }
    }
  }

  public async void RenderFirstPage()
  {
    Task<ImageSource> result = Task<ImageSource>.Factory.StartNew(() => RenderPage(0));
    image.Source = await result;
  }

  public ImageSource RenderPage(int pageNum)
  {
    if (pdfStream == null)
      return null;

    if (document == null)
      document = new Document(pdfStream);

    var page = document.Pages[pageNum];
    imageStream = new MemoryStream();
    page.SaveAsBitmap(imageStream, ImageEncoding.Png, 72);
    imageStream.Position = 0;
    return ImageSource.FromStream(() => imageStream);
  }
}
```
