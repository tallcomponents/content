# Event-Driven Generation

It is possible to add content in an event-driven manner. While the PDF document is being generated, events are fired. In the event handlers you provide content to be rendered. Using this model, TallPDF.NET will flash out pages while generating. Consequently the memory consumption is limited to the resources of a single page. This make TallPDF.NET the ideal component for generating large documents in a heavy-load environment.


By default, TallPDF.NET does not generate in event-driven mode. To generate in event-driven mde, you should call the Document.Write overload that takes an extra boolean as follows:


```
Document document = ...;
System.Web.HttpResponse httpResponse = ...;
FileStream file = ...;

// NOT event-driven
document.Write( file );
document.Write( file, false );
document.Write( httpResponse );
document.Write( httpResponse, false );

// event-driven
document.Write( file, true );
document.Write( httpResponse, true );
```

Code sample: Generate in event-driven mode by selecting the Document.Write overload (C#)



## Restrictions

If you choose to generate the PDF document in an event-driven manner, you should be aware of a number of restrictions. These restrictions are a consequence of the forward-only, non-cached nature of the generation process. They should be considered as a trade-off between functionality and speed/memory footprint.
&nbsp;<ul><li>
Cell.ColSpan is ignored. If a cell should span multiple columns, set its PreferredWidth property to a larger value so that is spans multiple columns.</li><li>
Cell.FitToContent is ignored. You must set the PreferredWidth property to a predefined value.</li><li>
Each section starts on a new page. The Section.StartOnNewPage property is ignored. If you want to switch to a different page layout, you can do this by handling the QueryPageSettings event which is fired for each page. If you want to switch to different headers/footers, you can do this by handling the StartPage and EndPage events which are fired for each page.</li><li>
You can not cross-reference paragraphs from fragments.</li><li>
Context fields are not resolved.</li><li>
You cannot use the CrossreferenceSection class to generate a table of contents or a list of figures/tables.</li></ul>

## Section Events

The following events are fired by the Section class.



### Events fired by Section
&nbsp;<table><tr><th>
Event</th><th>
Fired when</th><th>
Do this in your handler</th></tr><tr><td>
QueryPageSettings</td><td>
Just before a new page will be added</td><td>
Provide the page settings such as the size of the different page boxes (media box, bleed box, etc.).</td></tr><tr><td>
StartPage</td><td>
Just after a new page has been added</td><td>
Define areas on this page.</td></tr><tr><td>
EndPage</td><td>
Just before a new page will be added.</td><td>
Add some final content.</td></tr></table>

## ParagraphCollection Events

The following events are fired by the ParagraphCollection class.



### Events fired by ParagraphCollection
&nbsp;<table><tr><th>
Event</th><th>
Fired when</th><th>
Do this in your handler</th></tr><tr><td>
PrintFirstParagraph</td><td>
Just before the first paragraph on the current page is added.</td><td>
Provide a paragraph that should be the first on the current page.</td></tr><tr><td>
PrintParagraph</td><td>
New content is required.</td><td>
Provide a new paragraph.</td></tr></table>

## Paragraph Events

The following events are fired by the Paragraph class.



### Events fired by Paragraph
&nbsp;<table><tr><th>
Event</th><th>
Fired when</th><th>
Do this in your handler</th></tr><tr><td>
BreakParagraph</td><td>
Before a paragraph is broken across pages.</td><td>
Application specific.</td></tr><tr><td>
ContinueParagraph</td><td>
Before a paragraph continues on the next page.</td><td>
Application specific.</td></tr><tr><td>
EndParagraph</td><td>
After a paragraph has been fully rendered.</td><td>
Application specific.</td></tr><tr><td>
RollbackParagraph</td><td>
After a paragraph has been rolled back due to a text flow contraint such as keep-with-next.</td><td>
Application specific.</td></tr><tr><td>
TransformParagraph</td><td>
After a paragraph has been tranformed. E.g. due to a vertical alignment setting of a table cell.</td><td>
Application specific.</td></tr></table>

## TextParagraph Events

The following events are fired by the TextParagraph class.



### Events fired by TextParagraph
&nbsp;<table><tr><th>
Event</th><th>
Fired when</th><th>
Do this in your handler</th></tr><tr><td>
LineBreak</td><td>
When a text line needs to be broken.</td><td>
Specifiy where to break the line. If you do not handle this event, lines are broken at whitespace occurrence which is perfectly reasonable.</td></tr></table>

## RowCollection Events

The following events are fired by the RowCollection class.



### Events fired by RowCollection
&nbsp;<table><tr><th>
Event</th><th>
Fired when</th><th>
Do this in your handler</th></tr><tr><td>
PrintFirstRow</td><td>
Just before the first row on the current page is added to the current table. Note that this even is fired n times if the table spans n pages.</td><td>
Provide a row that should be the first on the current page.</td></tr><tr><td>
PrintRow</td><td>
A new row is required.</td><td>
Provide a new row.</td></tr></table>

## Example: Firehose Generation from SQL

Generating a PDF document from data stored inside a SQL database can very well be done in an event-driven manner. This way you can fully profit from the performance benefits of the forward-only SqlDataReader.



#### Create a SqlDataReader

A SqlDataReader provides a forward-only, read-only pointer over data retrieved from a SQL database. The following code sample shows you how to create a SqlDataReader instance. (The pubs database is installed together with the .NET Framework SDK.)


```
SqlConnection connection = new SqlConnection(
   @"server=(local)\NetSDK;database=pubs;Trusted_Connection=yes");
SqlCommand command = new SqlCommand(@"select * from Authors", connection);
connection.Open();
SqlDataReader reader = command.ExecuteReader();
```

Code sample: Create a SqlDataReader in C#



#### Prepare Table.Rows

The next step is to setup the Table.Rows class for generating rows that correspond to the records in the database. First pass the reader to the Data member of the RowCollection class. The Data member allows you to associate any data with the RowCollection collection. This data will be available in the event handler that will provide the rows. Next, the event handlers are connected to the RowCollection class.


```
Table table = new Table();
table.Rows.Data = reader;
table.Rows.PrintRow += new PrintRowEventHandler(PrintRow);
table.Rows.PrintFirstRow += new PrintRowEventHandler(PrintFirstRow);
```

Code sample: Prepare Table.Rows in C#



#### Handle PrintRow Event

When the Document.Write method is called with the pull argument set to true, the event handler PrintRow will be called somewhere during the generation process. The following shows how to pull the content from the SqlDataReader and pass it on to the generation process.


```
bool PrintRow( object sender, PrintRowEventArgs args )
{
  SqlDataReader reader = args.Data as SqlDataReader;
  if ( null != reader && reader.Read() )
  {
    args.Row = new Row();
    addCell(
      args.Row, 
      reader.GetValue(2).ToString() + " " + reader.GetValue(1).ToString() );
    addCell( args.Row, reader.GetValue(4).ToString() );
    addCell( args.Row, reader.GetValue(5).ToString() );

    args.MoreContent = true; // we have more content
  }
  args.MoreContent = false; // no more content
}
```

Code sample: Handle PrintRow Event in C#


addCell is a helper method that adds a cell to the row instance and is rather trivial.



#### Handle PrintFirstRow Event

The PrintFirstRow event is fired to allow the handler to add one or more header rows. This event is fired for each page that the table spans. The following shows how to provide a single header row.


```
void PrintFirstRow(object sender, PrintRowEventArgs args)
{
   args.Row = new Row();

   addCell(args.Row, "Author");
   addCell(args.Row, "Address");
   addCell(args.Row, "City");

   // Only one line, no more content
   args.MoreContent = false;
}
```

Code sample: Handle PrintFirstRow Event in C#



#### Finalize

After generation, the connection to the database is closed:


```
try
{
   Document document = new Document();
   //...
   //...
   document.Write( reader, true );
}
finally
{
   //...
   if ( null != connection ) connection.Close();
}
```

Code sample: After generation, close the SQL connection


