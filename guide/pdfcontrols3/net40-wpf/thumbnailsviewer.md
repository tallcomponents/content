# Thumbnails viewer

See [API reference](/pdfcontrols3/help/classes/TallComponents.PDF.Controls.WPF.ThumbnailsViewer?build=net40-wpf).

The ThumbnailsViewer shows the thubmnails of the PDF document of the PagesViewer control to which it is connected. 

Here is the XAML of class MainWindows:

``` xml
<Window x:Class="MyApp.MainWindow"
   ...    
   <DockPanel LastChildFill="True">
      <Menu DockPanel.Dock="Top">
         ...
      </Menu>
      <ToolBarTray DockPanel.Dock="Top">
         ...
      <ToolBar>
      <StatusBar Name="statusbar" DockPanel.Dock="Bottom">
         ...
      </StatusBar>
      <tc:ThumbnailsViewer Name="thumbnailsViewer" Width="200" DockPanel.Dock="Left">
	     ...
      </tc:ThumbnailsViewer>
      <tc:PagesViewer Name="pagesViewer">
	     ...
      </tc:PagesViewer>
    </DockPanel>
</Window>
```

Here is the codebehind of class MainWindow:

``` c#
public MainWindow()
{
   InitializeComponent();

   thumbnailsViewer.PagesViewer = pagesViewer;
   ...
}
```

And here is what the application looks like after opening the PDF Reference 1.7:

![bookmarks viewer](/guide/pdfcontrols3/net40-wpf/media/thumbnailsviewer.png)

Clicking items in the thumbnails viewer will make the PagesViewer control navigate to the given page and vice versa.
