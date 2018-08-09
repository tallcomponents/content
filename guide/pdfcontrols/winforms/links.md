# Handling links

By default, the PagesViewer will navigate to link destinations for as long as they are located in the same document. It will not however automatically open URL’s or files (that would impose a security risk).


In order to deal with these, it is possible to install a custom ActionHandler. One can override the standard behavior of the PagesViewer by overriding one or more virtual methods. The following sample will pop up a message box when a link gets clicked on:


```
public class MyActionHandler : DefaultActionHandler
{
  public MyActionHandler( PagesViewer pagesViewer ) : base( pagesViewer )
  {
  }

  public override void GoToUri(
    string uri, bool isMap, 
    TallComponents.PDF.Actions.ActionContext context)
  {
    MessageBox.Show( string.Format( "Go to URI: {0}", uri ) );
    base.GoToUri(uri, isMap, context);
  }

  public override void GoToInternalDestination(
    TallComponents.PDF.Navigation.InternalDestination internalDestination, 
    TallComponents.PDF.Actions.ActionContext context)
  {
    MessageBox.Show(
      string.Format("Go to Page: {0}", internalDestination.Page.Index));
    base.GoToInternalDestination(internalDestination, context);
  }

  public override void Launch(
    string path, 
    TallComponents.PDF.Navigation.WindowBehavior windowBehavior,   
    TallComponents.PDF.Actions.ActionContext context)
  {
    MessageBox.Show(string.Format("Open file: {0}", path));
    base.Launch(path, windowBehavior, context);
  }
}
```

## Custom link actions.

For the standard edition, action handling is restricted to link handling. The professional edition contains more virtuals that can be overridden. These will be explained later.
