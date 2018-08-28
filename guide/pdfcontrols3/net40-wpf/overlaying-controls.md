# Overlaying controls

When writing your custom PDF viewer/editor, you may want to enhance the PagesViewer control with your own controls. For this you have two options: you can do this by overlaying controls (discussed below) or by providing [interactors](interactors).

## Update position and size

If you control has to stick with an area in page space (such as the extent of a PDF widget) then you will have to update the position, size and visibility of your control when the viewport changes. You do this as follows:

- handle the [`PagesViewer.ViewportChanged`]() event
- calculate the new position and size in viewer space
- update your controls's position and size

Here is the code:

``` c#
pagesViewer.ViewportChanged += 

void OnViewportChanged()
{
}
```


