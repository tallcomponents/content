# Bookmarks viewer

See [API reference](/pdfcontrols3/help/classes/TallComponents.PDF.Controls.WPF.BookmarksViewer?build=net40-wpf).

The BookmarksViewer shows the bookmarks tree (aka outline) of the PDF document of the PagesViewer control to which it is connected. 

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
      <tc:BookmarksViewer Name="bookmarksViewer" Width="200" DockPanel.Dock="Left">
	     ...
      </tc:BookmarksViewer>
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

   bookmarksViewer.PagesViewer = pagesViewer;
   ...
}
```

And here is what the application looks like after opening the PDF Reference 1.7:

![bookmarks viewer](/guide/pdfcontrols3/net40-wpf/media/bookmarksviewer.png)

Clicking items in the treeview will make the PagesViewer control navigate to the associated destination.
