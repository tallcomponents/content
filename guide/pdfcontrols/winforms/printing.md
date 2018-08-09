# Printing

PDFControls.NET supports printing of PDF documents. This can be done interactively (via a print dialog) or without any user interaction.

In order to print interactively, one can use PagesViewer.Print() without passing any additional arguments. This will pop up a print dialog that allows a user to modify a number of print settings. Printing will proceed automatically when the user presses the OK button.

![Using PagesViewer.Print](/guide/pdfcontrols/winforms/media/Using-PagesViewer-Print.png)

Next to these calls, PDFControls.NET contains a few control classes that allow one to customize interactive printing. These classes are:
&nbsp;<ul><li>
PrintDialog: this class implements the print dialog that get shown by PagesViewer.Print().</li><li>
PrinterListControl: this class implements a winforms control that allows one to select a printer.</li><li>
PaperSizeListControl: this class implements a winforms control that allows one to select the paper size.</li><li>
PrintPreviewControl: this class implements a winforms control that shows a print preview of a particular page.</li></ul>&nbsp;
We will have a closer look at these classes below.



## Printing Dialog

Instead of just calling PagesViewer.Print(), the PrintDialog class allows one to show the printing dialog separately, followed by a call to Document.Print() to perform the actual printing:


```
TallComponents.Interaction.WinForms.Controls.PrintDialog dialog =
  new TallComponents.Interaction.WinForms.Controls.PrintDialog(pagesViewer);

dialog.ShowDialog();

if (dialog.DialogResult == DialogResult.OK)
{
  dialog.PrintSettings.QueryPagePrintSettings += new  
    EventHandler<TallComponents.PDF.QueryPagePrintSettingsEventArgs>
      (PrintSettings_QueryPagePrintSettings);

  document.Print(dialog.PrintSettings);
}
```

Using the print dialog in combination with explicit printing.


This allows one for example to check and modify some settings before the actual printing occurs. It also gives more control over the printing process itself.


The professional version for example, allows one to subscribe to the QueryPagePrintSettings event of the PrintSettings instance of the dialog, so that the PagePrintSettings can be changed on the fly for every page that gets printed.



## Printdialog Controls

The main advantage of the PrintDialog class is that it is easy to use, and provides access to a number of common settings. In some cases however, one will want to extend or limit the settings that a user has access to. In other cases, one may want to show a print dialog in a different language.


These cases can be solved by creating your own custom printing dialog. In order to facilitate this, we have provided the PrinterListControl, the PaperSizeListControl and the PrintPreviewControl. All one needs to do, it to place these controls on a custom form, and have their PrintSettings property refer to the proper PrintSettings instance. If a user interacts with these controls the print settings will be updated accordingly. Conversely, if the print settings get changed, the controls will automatically update to reflect this.


The PrintDialog sample shows an example of this.



## PrintSettings

The PrintSettings class allows one to specify print job parameters. It provides the following options:
&nbsp;<ul><li>
Collate: Whether pages of multiple copies will be assembled in the proper order.</li><li>
Copies: The number of copies that will be printed.</li><li>
Duplex: Specifies whether duplex printing must be used.</li><li>
PagePrintSettings: page specific print settings. See below.</li><li>
PagesAsIndices: the selection of pages that will be printed, given as an array that enumerates the page indices. These indices are zero-based.</li><li>
PagesAsString: the selection of pages that will be printed, given as a human-readable string. For example “1, 2, 5 - 10”. Note that “1” means: the page with index 0. This property can conveniently be used in user interfaces.</li><li>
PrinterSettings: this is a reference to a System.Drawing.Printing.PrinterSettings instance. It allows selecting a printer by specifying a PrinterName. In addition, it provides detailed information about the selected printer. Please note that it is also possible to change some properties of the PrinterSettings. We do not recommend this, as the corresponding properties in the PrintSettings class (such as Copies) take precedence.</li><li>
PrintController: a reference to a System.Drawing.Printing.PrintController instance. To a certain extent, a print controller controls the behavior of the .NET printing system. Also see “silent printing” below. This property may be null.</li><li>
PrinterName: The name of the printer. The PrinterSettings (see below) reflect the settings of the printer that is specified here. If a different name is assigned, the PrinterSettings will be updated.</li><li>
PrinterSettings. Contains settings that specify the characteristics of the printer. These settings are fixed and unrelated to the job.</li></ul>

## PagePrintSettings

Page specific printing settings are specified by a PagePrintSettings instance. The settings have been separated from the PrintSettings as they can vary per page.
&nbsp;<ul><li>
AutoRotate: when set, pages will be rotated to fit the sheet of paper best.</li><li>
Color: specifies whether a page must be printed in color.</li><li>
HorizontalAlignment: whether pages should be aligned left, center or right.</li><li>
Landscape: Specifies whether landscape mode is used for the page orientation. If this is off, the .Net printing system will offer a coordinate system that has its origin at the top left of a sheet of paper. If this is on, the origin will be at the top left of a rotated (landscape) sheet of paper.</li><li>
PaperSize: the paper size to use for printing. This should be an instance of the paper sizes that are listed in the printer settings.</li><li>
PaperSource: the paper source to use for printing (i.e. which paper tray). This should be an instance of the sources that are listed in the printer settings.</li><li>
RenderSettings: the render settings that will be used for printing. By default, this instance has its RenderPurpose set to Print. This has consequences for the annotations that get rendered (see the Annotation.Print property).</li><li>
ScaleToFit: specifies whether a page must be scaled to fit on the printed sheet of paper. When set, pages may get scaled up or down, depending on whether they are too small, or too big.</li><li>
Transform: specifies a transformation to be applied to the page before printing. . This makes it possible to fine-tune the placement of a page on a sheet of paper.</li><li>
UsePdfPageSize: when set, the specified PaperSize property will be ignored. Instead, the page size of the PDF page determines the chosen paper size.</li><li>
UsePrintableArea. When set, care will be taken not to print in areas that the printer cannot print on, due to physical limitations. If the page is too big, it will be shrunk so it fits in the printable area of the printer. If this flag is cleared, graphics that are placed outside the printable area will be lost. In many cases however it is best to clear this flag, as the output tends to lose it “natural size”, and the physical margins of a printer are typically much smaller than the margins of a PDF page.</li><li>
VerticalAlignment: whether pages should be aligned top, center, or bottom.</li></ul>&nbsp;
These settings are in effect for all printed pages from the moment they are specified. The professional edition allows one to subscribe to the QueryPagePrintSettings event of the PrintSettings instance, and this allows one to change the PagePrintSettings on the fly for every page that gets printed from that point on until the next change.

## PrinterSettings

The PrinterSettings class specifies the (fixed) characteristics of a printer. These setting have largely been derived from the System.Drawing.Printing.PrinterSettings class (more about that below).

- CanDuplex: Specifies whether the printer supports duplex printing.
- IsDefaultPrinter: Specifies whether the printer is the default printer.
- IsPlotter: Specifies whether the printer is a plotter.
- MaximumCopies: Specifies the maximum number of copies that the printer can handle.
- PaperSizes: The paper sizes that are supported by this printer.
- PaperSources: The paper trays of this printer.
- PrinterName: The name of this printer.
- PrinterResolutions: The resolutions that are supported by this printer.
- SupportsColor: Specifies whether the printer supports color.
- SystemPrinterSettings: the underlying System.Drawing.Printing.PrinterSettings class. In many cases, access to this instance is of no concern, as common print settings can be controlled by the classes above. Access to this class can be useful however if printer-specific settings need to be accessed that are not covered above. See for example the PrintDialog sample. The implementation of the properties button needs access to this instance in order show a printer-specific properties window.

Please note the following about the SystemPrinterSettings property.

The members of the System.Drawing.Printing.PrinterSettings, do not only contain the printer settings above, but also settings like Copies or DefaultPageSettings, which in turn contains many settings found in PagePrintSettings above. Basically, there is no difference between these settings. Changing them in PrintSettings or PagePrintSettings will change the corresponding settings in System.Drawing.Printing.PrinterSettings and vice versa.

Nonetheless it is better to avoid manipulating the settings in SystemPrinterSettings directly. This is not illegal in itself, but our printing controls will not automatically notice any updates in these settings, so they may no longer show the correct values.</td></tr></table>

## Silent Printing

One can avoid showing a printing dialog by invoking the Print method of the Document class and passing it a PrintSettings instance. The document will then be printed immediately, using the specified settings.

By default, the PrintSettings class will use an instance of System.Drawing.Printing.StandardPrintController for its PrintController. This means that printing will proceed ‘silently’, i.e. no progress box will be shown. If you do want a progress box, please assign an instance of the System.Drawing.Printing.PrintControllerWithStatusDialog class to the PrintController property of the PrintSettings class.

Please note that the PrintController class raises a number of events that allow one to monitor progress, and execute custom code. See the relevant .NET documentation for more information.

## Font Substitution

By default, PDFControls.NET will send all output to the printer as low-level graphical GDI instructions, such as lines, fills, and images. This is also the case for text. This leads to graphically accurate output, but as a downside, print jobs may become large, and thus, slow.

If this is the case, one may consider substituting the PDF fonts by system fonts, provided that system fonts are available that (approximately) resemble the PDF fonts. In this way, text will not be rendered as a collection of lines and fills, but GDI will be told to render the text. This will lead to smaller print jobs. See the chapter on Fonts for details.