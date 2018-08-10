# JavaScript

The professional edition of PDFControls.NET supports executing JavaScript. This will work out-of-the-box for many simple scripts, but some JavaScript objects need to be customized by the application that they run in, in order to provide the correct functionality.

The way that this works, is basically by registering a custom JavaScript factory that creates custom JavaScript objects. The code below for example registers ‘MyJavaScriptFactory’ and has it create a custom App object.

``` cs
JavaScriptFactory.Register(new MyJavaScriptFactory());

…

private class MyJavaScriptFactory : JavaScriptFactory
{
  protected override App CreateApp()
  {
    return new MyApp();
  }
}
```

The following sections will discuss the objects that we allow to be customized (such as App). For details about these objects please see the Acrobat JavaScript reference, which can be found at Adobe.

## Global

The Global object allows data to be shared between documents and have data persisted across “sessions”.

In order to provide this functionality, the application will need to provide a mechanism for persisting data. PDFControls.NET does not implement this by default, as this would require write access to some storage medium.

The TallComponents.PDF.Javascript.Scripting.Global object allows the following methods to be overridden:

- GetPersistedNames(): returns the collection of variable names that have been persisted during all previous sessions. By default, this method returns an empty collection.
- GetPersistedValue(name): returns the value of a previously persisted variable. By default, this method returns null for all passed names.
- Initialize(): initializes global storage. This method will be called once, before executing any JavaScript. Override this method to add custom initialization code. By default this method defines a set of standard JavaScript methods and objects. Make sure to call base in your implementation.
- SetPersistedValue(name, value): persists the given variable name with the given value. The type of the variable must be Boolean, Number or String. The latter is a requirement of the Acrobat JavaScript reference. By default this method has no effect.

## App

The App object has the same lifetime as the application, and amongst other things, it defines a number of GUI-related functions. The TallComponents.PDF.Javascript.Scripting.App class allows the following methods to be overridden:

- Alert(message, icon, buttons, title, document, checkbox): this method implements the App.alert functionality. It should display an alert dialog on the screen. See the Acrobat JavaScript reference for details. By default this method does nothing, and returns AlertResult.OK.
- LaunchURL(url, newFrame): this method implements the App.launchURL functionality. It should launch a browser window with the given URL. See the Acrobat JavaScript reference for details. By default this method does nothing.

## Doc

A Doc object will be created for each document, and it has the same lifetime. It offers the interface between JavaScript and the document. The TallComponents.PDF.Javascript.Scripting.Doc class allows the following methods to be overridden:

- ExportAsFDF(path, formData): this method implements the doc.exportAsFDF functionality. See the Acrobat JavaScript reference for details. Except for the path, all other JavaScript parameters have been accounted for in the formData object, for as far as supported (see the PDFControls.NET reference for details). This method should write the provided form data to disk. By default, this method does nothing, as this would require write access to the system.
- ImportAnFDF(path): this method implements the doc.importAnFDF functionality. See the Acrobat JavaScript reference for details. This method should create an FdfFormData instance from the given path and return it as its result. The default implementation will try to do this. It will return null if this fails. In order to provide an implementation that adheres to the PDF specification, one should override this method and invoke base.ImportAnFDF(path) first. If this returns null, one should allow the user to select a file.
- ImportAnXFDF(path): this method implements the doc.importAnXFDF functionality. See the Acrobat JavaScript reference for details. This method should create an XfdfFormData instance from the given path and return it as its result. The default implementation will try to do this. It will return null if this fails. In order to provide an implementation that adheres to the PDF specification, one should override this method and invoke base.ImportAnXFDF(path) first. If this returns null, one should allow the user to select a file.
- ImportTextData(path): this method implements the doc.importTextData functionality. See the Acrobat JavaScript reference for details. This method should populate the fields of the document as indicated in the file. By default this method does nothing, and returns ImportTextDataResult.CannotOpenFile.
- SubmitForm(options): this method implements the doc.submitForm functionality. See the Acrobat JavaScript reference for details. The parameters of this method are passed in a SubmitFormOptions instance, for as far as supported. See the PDFControls.NET reference for details. This method has a default implementation for posting FDF to the URL that is specified in the options. Other variants have no default implementation yet, but this may be provided in future updates.

Note that SubmitForm has a default implementation. This means that form data will be submitted to the specified server by default. This may be a privacy concern. If so, please override this method and provide a custom implementation.

## Console

The console object gives access to a console window. This allows debug information to be printed for example. PDFControls.NET provides no default implementation. The TallComponents.PDF.Javascript.Scripting.Console class allows the following methods to be overridden:

- Clear(): implements the console.clear functionality. See the Acrobat JavaScript reference for details. This method has no default implementation.
- Hide(): implements the console.hide functionality. See the Acrobat JavaScript reference for details. This method has no default implementation.
- PrintLn(): implements the console.printLn functionality. See the Acrobat JavaScript reference for details. This method has no default implementation.
- Show(): implements the console.show functionality. See the Acrobat JavaScript reference for details. This method has no default implementation.

## Messages

This object is not present in the Acrobat JavaScript object model. PDFControls.NET defines it, so that it becomes possible to provide localized message strings. The TallComponents.PDF.Javascript.Scripting.Messages class allows the following methods to be overridden:

- ClockAm: Override this string to provide a localized version of the string "am".
- ClockPm: Override this string to provide a localized version of the string "pm".
- DoesNotMatchFieldFormat: Indicates that a field value does not match its format. Override this string to provide a localized version of the string "The value entered does not match the format of the field".
- InvalidDateForField: Indicates that an invalid date is being used for a particular field. Override this string to provide a localized version of the string "Invalid date/time: please ensure that the date/time exists. Field ". Please note that the field name will be printed after this string.
- InvalidMonth: Indicates that an invalid month index is being used. Override this string to provide a localized version of the string "** Invalid **"
- MonthsFull: Override this string array to provide an array of localized full names for all months, e.g. ["January", "February", ...]. The array must have size 12.
- MonthsShort: Override this string array to provide an array of localized short names for all months, e.g. ["Jan", "Feb", ...]. The array must have size 12.
- MustBeBetweenOrEqual: Indicates that a value falls outside particular bounds. Override this string to provide a localized version of the string "Invalid value: must be greater than or equal to %s and less than or equal to %s.". Please note that the bounds are provided as an argument to this string and will be inserted at the locations of ‘%s”.
- MustBeGreaterThanOrEqual: Indicates that a value is too small. Override this string to provide a localized version of the string "Invalid value: must be greater than or equal to %s.". Please note that the lower bound is provided as an argument to this string and will be inserted at the location of ‘%s”.
- MustBeLessThanOrEqual: Indicates that a value is too big. Override this string to provide a localized version of the string "Invalid value: must be less than or equal to %s.". Please note that the upper bound is provided as an argument to this string and will be inserted at the location of ‘%s”.
- MustMatchFieldFormat: Indicates that a field value does not match a particular format. Override this string to provide a localized version of the string “should match format ". Please note that the required format will be printed directly after this string.

Please note that these are JavaScript strings. Some of these strings have escape codes that must be present in order for the message text to be constructed correctly.
