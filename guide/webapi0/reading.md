## Local and external documents

In the following `{document-path}` is used to reference a local or external document.

## Document

```
Accept : application/json

GET {document-path}
```

Response:

``` json
{
   "Version":"1.7",
   "Type":1,
   "PageCount":2,
   "HasFields":true,
   "HasBookmarks":false,
   "HasLayers":false,
   "Info":{
      "Author":"SE:W:CAR:MP",
      "Subject":"U.S. Individual Income Tax Return"
   },
   "Path":"/v0/documents/external/tallcomponentstest/0BzSuNhHw89yfRGcyYmFtX0tnU3c",
   "Id":"0BzSuNhHw89yfRGcyYmFtX0tnU3c",
   "Filename":"document-from-source.pdf",
   "ModifiedOnUtc":"Tue, 24 Jan 2017 15:03:11 GMT",
   "CreatedOnUtc":"Tue, 24 Jan 2017 15:03:11 GMT"
}
```

> `ModifiedOnUtc` is only available for documents in external storage

Valid values for documentType are `0` (classic), `1` (XfaStatic), `2` (XfaDynamic).
## Pages

### List pages

```
Accept : application/json

GET {document-path}/pages
```

Response:

``` json
[
   {
      "Rotate":0,
      "MediaBox":{
         "Left":0.0,
         "Bottom":0.0,
         "Width":612.0,
         "Height":792.0
      }
   },
   ...
]
```

In addition to `MediaBox `, a page may also have `CropBox`, `BleedBox`, `TrimBox` and `ArtBox`. All are represented by an array of 4 numbers.

Valid values of `Rotate` are `0`, `90`, `180` and `270`. The rotate property is only an instruction for the viewer whether the page should be displayed rotated clockwise.

## Fields

See PDFKit.NET [forms](https://www.tallcomponents.com/pdfkit/help/forms.htm/) and [field](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Forms_Fields_Field.htm).

```
Accept : application/json

GET {document-path}/fields
```

Response:

``` json
[
  {
    "fullName": "employee.first",
    "export": true,
    "mappingName": "exployeeFirst",
    "readOnly": false,
    "requiredMode": 1,
    "fieldType": "text",
    "toolTip": "Your first name. Optional."
  },
  {
    "fullName": "employee.last",
    "export": true,
    "mappingName": "exployeeLast",
    "readOnly": false,
    "requiredMode": 2,
    "fieldType": "text"    
  },

]
```

Depending on the value of fieldType, the following additional properties exist:

|Property|Description|Valid for types|
|--------|-----------|---------------|
|fieldType|Valid values are: `text`, `pushButton`, `checkBox`, `radioButton`, `dateTime`, `dropDownList`, `listBox`, `numeric`, `password` and `unknown`.|*all fields*|
|fullName||*all fields*|
|export|Boolean. This field will be exported when submitted.|*all fields*|
|mappingName||*all fields*|
|requiredMode|Valid values are: 0 (optional), 1 (recommended), 2 (required).|*all fields*|
|readOnly||*all fields*|
|multiline|Boolean|text|
|comb|Boolean|text|
|richText|Boolean|text|
|fileSelect|Boolean. Text field is used to enter a pathname of a file.|text|
|maxLength|Integer. Maximum number of charaters|text, password|
|passwordCharacter|The character (or string) used to hide the password|password|
|doNotScroll|Boolean. Whether long text must be scrolled.|text|
|value|A text string|*value field*|
|defaultValue|A text string|*value field*|
|formattedValue|The formatted value as a text string|*value field*|
|hasNeutralState|Boolean|checkBox|
|neutralValue|A text string|checkBox|
|onValue|A text string|checkBox|
|offValue|A text string|checkBox|
|radioButtonOptions|An array of string values|radioButton|
|canBeDeselected|Boolean|radioButton|
|dateTimeFormat|Valid values: 0 (date only), 1 (time only), 2 (both)|dateTimeField|
|allowTextEntry|Boolean|dropDownList|
|listOptions|An array of string tuples like so: `[{"display": "Netherlands", "export": "NL"}]`|dropDownList, listBox|
|listCommitMode|If true, the field value is updated at the moment that a user changes the selection. If false, the value is updated when the field looses focus.|dropDownList, listBox|
|multiSelect|Boolean. Multi-select allowed.|listBox|
|spellCheckAllowed|Boolean|dropDownList, text|
|numberFormat|0 (float), 1 (decimal), 2 (integer)|numeric|
|calculateAction|JavaScript text|*value field*|
|formatAction|JavaScript text|*value field*|
|keyStrokeAction|JavaScript text|*value field*|

These are *value fields*: checkBox, dateTime, dropDownList, listBox, numeric, password, radioButton, text. 

The type of the properties `value` and `defaultValue` depend on the `fieldType` property as follows:

|Field type|properties `value` and `defaultValue`|
|----------|-------------------------------------|
|`text`, `password`|A text string|
|`radioButton`|An index into the radioButtonOptions array|
|`checkBox`|Valid values: 0 (on), 1 (off), 2 (neutral), 3 (unknown)|
|`listBox`|An index into the listOptions array|
|`dropDownList`|A text string or an index into the listOptions array|
|`dateTime`|A datetime value|

  
Field types that are intentionally omitted for now are `image`, `signature` and `barcode`.

## Annotation

An annotation is a rectangular area on a PDF page that the user can interact with. Examples are *links*, *widgets*, *markup* and *notes*. In the PDFKit.NET API, the class Annotation is the abstract base class of all annotation classes. The figure below shows the class hierarchy:

![](https://www.tallcomponents.com/pdfkit/help/media/annotation-class-hierarchy.png)

The web API does not represent an Annotation explicitly. Instead it represents widgets, links, markups and popups as described in the next sections. Annotations are discussed here because of the properties that are shared by the specializations and thus are included in the JSON responses of the respecitve annotation types.

These properties are:

|Property|Description|
|--------|-----------|
|PageIndex|Integer. The index of the page|
|Left|The position of the left edge in points|
|Bottom|The position of the bottom edge in points|
|Width|The width in points|
|Height|The height in points|
|BorderWidth|Border width in points. 0 means no border|
|BorderColor|RGB values in the range 0..255|
|BorderStyle|Valid values are: 0 (solid), 1 (dashed), 2 (beveled), 3 (inset), 4 (underline)|
|Invisibe|The annotation is not visible on screen|
|Print|Boolean. The annotation is printed|
|Locked|Boolean. The annotation is locked in the viewer application|
|TabOrder|Integer. The order in which annotations receive input focus when the tab key is used|
 
## Widgets

A widget is an annotation and it is the visual representation of a field. Fields are defined at the document level. Widgets are defined at the page level. A field is associated with 0 or more widgets (but mostly with one). The following diagram illustrates the relations between document, page, field and widget:

![](https://www.tallcomponents.com/pdfkit/help/media/document-fields-pages-and-widgets.png)

In addition to the annotaton properties, a Widget has the following properties:

|Property|Description|
|--------|-----------|
|BackgroundColor|RGB values. null means transparent|
|Field|The full name of the associated field|
|Font|The name of the text font|
|FontSize|The size of the text font in points|
|TextColor|RGB values|
|HorizontalAlignment|The horizontal text alignment. Valid values are: 0 (left), 1 (center), 2 (right)|
|VerticalAlignment|The vertical text alignment. Valid values are: 0 (top), 1 (middle), 2 (bottom)|
|Rotate|Clockwise rotation in degrees. Valid values are: 0, 90, 180 and 270|

### List all widgets on a page 

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/widgets
```

Response:

```
[
 {
    "BackgroundColor": null,
    "Field": "topmostSubform[0].Page1[0].HeaderPg1[0].f1_01[0]",
    "Font": "HelveticaLTStd-Bold",
    "FontSize": 8.0,
    "TextColor": {
      "R": 0.0,
      "G": 0.0,
      "B": 128.01
    },
    "HorizontalAlignment": 0,
    "VerticalAlignment": 2,
    "Rotate": 0,
    "PageIndex": 0,
    "BorderWidth": 1.0,
    "BorderColor": null,
    "BorderStyle": 0,
    "Invisible": false,
    "Print": true,
    "Locked": false,
    "TabOrder": 1425,
    "Left": 225.687,
    "Bottom": 720.0,
    "Width": 91.113,
    "Height": 11.999000000000024
  },
  ...
]
```

### Get a widget on a page

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/widgets /{zero-based-index}
```

Response:

```
```

## Links

A link is an annotation that executes a sequence of actions when clicked. In addition to the annotation properties, a link has the following properties:

|Property|Description|
|--------|-----------|
|highlightStyle|The visual effect that is used when the mouse is pressed inside the annotation area. Valid values are: 0 (none), 1 (invert the background), 2 (invert the border), 3 (push down)|
|mouseupActions|An array of embedded action objects. See Actions|

### List all links on a page

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/links
```

Response:

```
```

### Get a link on a page

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/links/{zero-based-index}
```

Response:

```
```

## Markups

Markup annotations are used to mark-up a document. In a typical mark-up scenario, the author of a document asks a peer to review the document. The peer will add comments at specific location in the document, e.g. to mark-up a typo or to mark-up a figure that is not clear enough. Mark-ups can take several visual forms of which a simple textual note is the most common one.

In the PDFKit.NET API, Markup is the abstract base class of all markup annotations. It has the following properties that are common to all markup annotations. Note how property markupType implements the notion of special markup annotations.

|Property|Description|Valid for types|
|--------|-----------|---------------|
|markupType|Valid values are: `fileAttachment`, `freeText`, `line`, `note`, `square`, `stamp`, `textMarkup`, `unknown`|*all*|
|author||*all*|
|contents|The plain text that is displayed by the popup of this markup|*all*|
|created|datetime|*all*|
|inReplyTo|A relative URL to the markup that this markup is a reply to|*all*|
|opacity|Opacity specified in the range 0..255|*all*|
|popup|The embedded popup annotation associated with this markup|*all*|
|subject|String. Subject of this markup|*all*|
|text|The (rich) text that is displayed by the pop-up of this markup|*all*|
|iconName||fileAttachment, note|
|embddedFile|An embedded file object|fileAttachment|
|fontSize||freeText|
|textColor||freeText|
|interiorColor||freeText, line, square|
|orientation||freeText, stamp|
|lineEndStyle|See [list](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Annotations_Markups_LineEndStyle.htm)|line|
|lineStartStyle|See [list](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Annotations_Markups_LineEndStyle.htm)|line|
|lineHasCaption|Boolean. If true, text property is the caption of the line|line|
|leaderLine|Double|line|
|leaderLineExtension|Double|line|
|stateModel|The state model of this note. Possible values: `0` (none), `1` (marked), `2` (review), `3` (migration)|note|
|marked|Boolean. Only meaningful if stateModel is marked|note|
|migrationState|Possible values: `0` (none), `1` (confirmed), `2` (not confirmed). Only meaningful if stateModel is migration|note|
|reviewState|Possible values: `0` (none), `1` (accepted), `2` (rejected), `3` (cancelled), `4` (completed). Only meaningful if stateModel is review|note|
|stampAppearance|See [PDFKit.NET documentation](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Annotations_Markups_StampAppearance.htm) for stamp appearances|stamp|
|textAppearance|`0` (unknown), `1` (highlight), `2` (underline), `3` (squiggly), `4` (strikeout)|textMarkup|
|name|Name of unknown markup|unknown|


### List all markups on a page

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/markups
```

Response:

```
```

### Get a markup on a page

```
Accept : application/json

GET {document-path}/pages/{zero-based-index}/markups/{zero-based-index}
```

Response:

```
```

## Popups

A popup is a special annotation that displays the text of a markup as a pop-up message. The pop-up allows the user to edit the text. A pop-up is associated with a markup through its Markup property. A markup is associated with a popup through its Popup property. 

Popups are associated with markups, not with a page. Therefore it is not possible to list the popups on a page.

Note that the displayed text is a property of the markup, not of the pop-up.

![](https://www.tallcomponents.com/pdfkit/help/media/created-from-code-sample.png)

In addition to the Annotation properties, a Popup annotation has the following properties:

|Property|Description|
|--------|-----------|
|Open|Boolean. Pop-up should initially be displayed open|
|Markup|The Markup to which this pop-up belongs. *we need a way to reference*|


## Bookmarks

A PDF document has a bookmarks tree which is also known as the document outline or table of contents. Each bookmark item has an array of actions that are executed when selected. Typically, a bookmark item has a simple goto action pointing to a page inside the PDF document.

```
Accept : application/json

GET {document-path}/bookmarks
```

Response:

``` json
[
  {
    "title": "Introduction",
    "textColor": [255, 0, 0],
    "bold": true,
    "italic": false,
    "actions": 
    [
      {
        "actionType": "goto"
        "destination":
        {
          pageIndex: 0
        }
      }
    ],
    "children":
    [
      {
        "title": "Getting started"
      }
    ]
  }
]
```

## Destination

A Destination has the following properties:

|Property|Description|
|--------|-----------|
|path|Only defined for remote destination. The absense of this properties implies that the destination is inside the same document|
|pageIndex||
|pageDisplay|See [list](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Navigation_PageDisplay.htm) of possible values|
|windowBehavior|See [list](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Navigation_WindowBehavior.htm) of possible value. Only meaningful is path is defined|
|zoomFactor||
|left||
|bottom||
|right||
|top||

## Named destinations

Named destination are defined at the document level. A goto action may refer to one of these destinations using its name.

### List all named destination in a document

```
Accept : application/json

GET {document-path}/destinations
```

Response:

``` json
[
  {
    "name": "introduction",
    "destination": 
    {
      "pageIndex": 0    
    }
  },
  {
    "name": "appendixa",
    "destination": 
    {
      "pageIndex": 34
    }
  }
]
```

## Action

An action object has the following properties:

|Property|Description|Valid for types|
|--------|-----------|---------------|
|actionType|Possible values: `reset`, `submit`, `import`, `hide`, `goto`, `launch`, `javaScript`, `named`, `uri`, `unknown`|*all*|
|allFields|Boolean|reset, submit|
|fields|Array of full names of fields|reset, submit|
|convertDates|Boolean. If true, submit dates using the format 'D:yyyymmdd' (without the quotes)|submit|
|submitFormat|[Possible value](https://www.tallcomponents.com/pdfkit/help/T_TallComponents_PDF_Actions_SubmitFormat.htm)|submit|
|httpMethod|Only meaningful if submitFormat is `pdf` or `html`|submit|
|includeEmptyFields|Boolean. If true, submit fields with no value. Only meaningful if submitFormat is `pdf`.|submit|
|url|Text string. Submit to this URL or start an external website from this URL|submit, uri|
|isMap|Only meaningful is action is triggered by an annotation. Submit mouse position when opening URL|uri|
|destination|Go to this embedded destination. If a string, then this is a named destination|goto|
|path|The path of the FDF file to import or the application to launch|import, launch|
|javaScript|Excute this JavaScript|javaScript|
|hide|Hide (true) or show (false) annotations|hide|
|annotations|The annotations to hide or show. An array of relative URLs. **Implementation note: property HideAction.Fields should be resolved to the associated widgets**|hide|
|name|Action name|named|

### Document actions

```
Accept : application/json

GET {document-path}/actions
```

Response:

``` json
{
  "open":
  [
    {
      "actionType": "javaScript",
      "javaScript": "... javascript code ..."
    },
    {
      "actionType": "javaScript",
      "javaScript": "... javascript code ..."
    }
  ],
  "afterPrint": 
  {
    "actionType": "javaScript",
    "javaScript": "... javascript code ..."
  }, 
  "afterSave": 
  {
    "actionType": "javaScript",
    "javaScript": "... javascript code ..."
  }, 
  "beforeClose": 
  {
    "actionType": "javaScript",
    "javaScript": "... javascript code ..."
  }, 
  "beforePrint": 
  {
    "actionType": "javaScript",
    "javaScript": "... javascript code ..."
  }, 
  "beforeSave": 
  {
    "actionType": "javaScript",
    "javaScript": "... javascript code ..."
  },
  "calculationOrder": 
  [
    "order.vat",
    "order.subTotal",
    "order.grandTotal"
  ]  
}
```

## Layers

PDF layers are a means to selectively mark parts of the page graphics as visible or invisible. A typical example is an architectural drawing where layers are used to group related graphics such as measurements, details, comments, etc.

Layers are defined at document level as opposed to at page level. A document has 0 or more layers. A layer has a name and a boolean flag (visible).

```
Accept : application/json

GET {document-path}/layers
```

Response:

``` json
[
  {
    "name": "comments",
    "visible": true
  },
  {
    "name": "details",
    "visible": false
  }
]
```
