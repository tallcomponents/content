# Generate PDF from XML

The central idea of TallPDF.NET is that you can build an instance of a TallPDF Document and that you can save that instance to PDF. Instead of building a Document instance programmatically, you can also load it entirely or in parts from XML. There is no difference between a document instance loaded from XML and one that has been constructed programmatically. So, in addition to building PDF programmatically, you can also generate PDF from XML. For this we have defined a proprietary XML format that reflects the programmatic object model in a very obvious way.


**NOTE**: The evaluation download includes the TallPDF.NET XML schema as an xsd document. Section _Visual Studio .NET IntelliSense (XML)_ describes how to use this XSD to enable IntelliSense in Visual Studio .NET while editing TallPDF.NET XML.


The following is a simple XML document that conforms to the TallPDF.NET format:


```
<document xmlns="http://www.tallcomponents.com/schemas/tallpdf/v3">
   <section>
      <paragraph type="textparagraph">
         <fragment>Hello World</fragment>
      </paragraph>
   </section>
</document>
```

Code sample: Hello World TallPDF.NET XML


You invoke the conversion of this XML to PDF as follows:


```
FileStream file = new FileStream("out.pdf", FileMode.Create);

XmlDocument xml = new XmlDocument();
xml.Load("helloworld.xml");

Document document = new Document();
document.Write(new XmlNodeReader(xml.DocumentElement), file);
```

Code sample: Invoke the conversion of XML to PDF in C#


TallPDF.NET does not only read from an XmlElement; it also reads from any XmlReader implementation. The XmlReader is the no-caching, forward-only alternative. See also Section _Working with XSL_.


Note that you can not only read the top-level class 'Document' from XML, you can read each TallPDF.NET layout class from XML. This allows you to mix a programmatic and XML approach. Because our XML reader do not expect an XmlDocument you can start reading at any location in a given XML document.



## XML Format Specification

The TallPDF.NET XML format can be derived from the definitions of the TallPDF.NET layout classes using a number of rules. This is not by coincidence; all TallPDF.NET classes inherit from a common class, TallComponents.PDF.Layout.Object, where the Read method is implemented. This method transforms XML to TallPDF.NET objects using the reflection mechanisms of .NET. Note that this guarantees a 100% correspondence between the programmatic and XML approach. This section discusses the rules that map the TallPDF.NET classes on XML. This section assumes that you understand the TallPDF.NET document object model and have a basic understanding of XML. The mapping is case-insensitive.


We distinguish the following XML node types:
&nbsp;<ul><li>
Top Element</li><li>
Child Elements</li><li>
Attributes</li><li>
Text Nodes</li></ul>

#### Top Element

The top element is the element that is passed to the read method of the TallPDF.NET object. The top element is not necessarily the document element, but it may be. The name of the top element can be anything.



#### Child Element

Child elements correspond to public, writeable properties of the class that corresponds to the parent element. We should distinguish between value (e.g. Drawing.Width), object (e.g. Document.OddFooter) and collection properties (e.g. Table.Rows). These case are discussed separately.



#### Value Type Properties

Properties that have a value type such as are Fragment.FontSize (double) or Section.StartOnNewPage (bool) are specified by an attribute on the corresponding element. The name of the property matches the name of the attribute in XML. Example:


```
<fragment fontsize="10"/>
```

Code sample: Assign a value property


Note that TallPDF.NET XML is case insensitive when it comes to element names and attribute names.


Properties of type double (widths, heights, margins, padding etc.) can be specified in any unit supported by the Unit class. Internally, the value will be converted to points and assigned to the property. See Unit.Parse in the Type Reference for a complete list of all supported units. Example:


```
<!-- A4 Landscape --><pagesize width="297mm" height="210mm"/>
```

Code sample: Assign a double value using a unit specifier


Objects like Font that have named static properties or fields of the same type can be assigned by entering the name of the static property or field.value. Example:


```
<fragment textcolor="AquaBlue">
```


#### Reference Type Properties

Properties that have a reference type such as Cell.Border are usually represented by a child element as opposed to an attribute. The name of the element matches the the name of the property. Example:


```
<cell>
   <border>
      <top width="1" color="Black"/>
   </border>
</cell>
```

Code sample: Properties of reference type are specified as child elements


An exception is a property of reference type that has static properties or static fields of the same type. In that case, you can specify the property as an attribute. Example:


```
<fragment font="Helvetica"/>
```

Code sample: Fragment.Font specified as an XML attribute



#### Special Attribute "type"

Some properties can be assigned values of different types. For example, Border.Background is of the type Brush. Brush itself is an abstract that has specializations such as SolidBrush. In that case, you must explicitly specify the type of object you wish to assign.Example:


```
<border>
   <background type="solidbrush" color="red"/>
</border>
```

Code sample: Assign an instance of SolidBrush to a property of type Brush


In this case, a new instance of SolidBrush is assigned to Border.Background and its Color property is set to Red.


For TallPDF.NET you can simply use the class name as opposed to the fully qualified name. If you are using your own classes that derive from TallPDF.NET classes, then you will need to specify the fully qualified name. Note that fully qualified class names are case sensitive in TallPDF.NET XML.



#### Collection Properties

The TallPDF.NET document object model has several collection properties such as Section.Paragraphs, Table.Rows and TextParagraph.Fragments. It is not required to include these collections as elements in the XML. TallPDF.NET will automatically look for the collection property that can contain the child element as an item. The name of the child element matches the singular form of the name of the collection property. Example:


```
<table>
   <!-- no need to include rows -->
   <row>
      <!-- no need to include cells -->
      <cell>
         <!-- paragraphs go here... -->
      </cell>
   </row>
   <row>
      <!-- no need to include cells -->
      <cell>
         <!-- paragraphs go here... -->
      </cell>
   </row>
</table>
```

Code sample: Collection properties are implicit in XML


In the previous example, the property name is equal to the name of the type. If this is not the case, you must specify the type explicitly as in the next example.


```
<section>
   <!-- add a new instance of type TextParagraph to Section.Paragraphs -->
   <paragraph type="textparagraph">
      <!-- more -->
   </paragraph>
   <!-- add a new instance of type Table to Section.Paragraphs -->
   <paragraph type="table">
      <!-- more -->
   </paragraph>
</section>
```

Code sample: Some collection properties require an explicit type attribute


The name of the element indicates the collection that the object will be added to. Heading for example has 2 collection properties, Fragments and NumberFragments, both of type Fragment. Example:


```
<paragraph type="heading" level="0" label="Chapter" caption="XML">
   <!-- add new Fragment instance to Heading.NumberFragments -->
   <numberfragment text="#l #0"/>
   <!-- add new Fragment instance to Heading.Fragments -->
   <fragment text="#c"/>
</paragraph>
```

Code sample: Some collection properties require an explicit type attribute


The name of the element indicates the collection that the object will be added to. Heading for example has 2 collection properties, Fragments and NumberFragments, both of type Fragment. Example:


```
<paragraph type="heading" level="0" label="Chapter" caption="XML">
   <!-- add new Fragment instance to Heading.NumberFragments -->
   <numberfragment text="#l #0"/>
   <!-- add new Fragment instance to Heading.Fragments -->
   <fragment text="#c"/>
</paragraph>
```

Code sample: Multiple collection properties of the same type



#### Enum Property

If the property is of type enumeration, then the attribute value should match one of the enum fields. For example:


```
<pen linecapstyle="butt"/>
```

Code sample: Specify an enum property as an XML attribute



#### Text Properties

Properties of type string that are called Text are treated somewhat differently. In addition to specifying a string property simply as an attribute, a string property can also be specified as a text node. This allows you to span multiple lines and to use the CDATA construct in order to include characters that have a special meaning in XML. Example:


```
 <fragment>
   <![CDATA[
Properties of type string named Text are treated specially. 
In addition to using an attribute named text, you can specify 
it as a text node. This is a natural choice since this property 
is typically set to a large piece of text, spanning multiple lines. 
Furthermore, it makes it possible to use the CDATA construct. 
This allows us to use characters reserved by XML like> 
and & without escaping them. This is especially relevant when 
the text is read from an external source.
]]>
</fragment>
```

Code sample: Properties of type System.String can be specified as text nodes



## Working with XSL

TallPDF.NET defines its own proprietary format. In general you will have XML data in some other forma , e.g. as generated by the FOR XML statement of SQL. In that case you will write an XSL(T) style sheet that transforms your XML to XML that is understood by TallPDF.NET. Normally, you write such a transformation only once per XML schema.


The following snippets show, respectively, a custom XML document, an XSL document that translates it to TallPDF.NET XML and finally a C# code that invokes the transformation. Note that this document does not discuss writing XSL documents since this is a general technique independent of our component.


```
<?xml version="1.0" encoding="utf-8"?>
<Customers>
   <Customer id="1">
      <Name>Chris Sharp</Name>
   </Customer>
   <Customer id="2">
      <Name>Mike Jones</Name>
   </Customer>
</Customers>
```

Code sample: Application specific XML


The following XSL transforms the above XML to TallPDF.NET XML.


```
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0"
   xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
   xmlns="http://www.tallcomponents.com/schemas/tallpdf/v3">
   <xsl:output method="xml" encoding="utf-8"/>
   <xsl:template match="/">
      <!-- the document is declared at root level -->
      <document>
         <section>
            <!-- Static content: this will appear in every document
                 regardless of the data -->
            <paragraph type="textparagraph">
               <fragment font="helveticabold" fontsize="14">
                  Simple XSL Transformation
               </fragment>
            </paragraph>
            <!-- Dynamic content: go through the data and render content 
                from that -->
            <xsl:apply-templates select="/Customers/Customer"/>
         </section>
      </document>
   </xsl:template>
   <xsl:template match="/Customers/Customer">
      <!-- Dynamic content: render a paragraph for each customer -->
      <paragraph type="textparagraph">
         <fragment font="helveticabold" fontsize="10">
            <xsl:value-of select="@id"/>.
         </fragment>
         <fragment font="helvetica" fontsize="10">
            <xsl:value-of select="Name"/>
         </fragment>
      </paragraph>
   </xsl:template>
</xsl:stylesheet>
```

Code sample: XSL transforming XML to TallPDF.NET XML


The following code invokes the transformation in order to generate PDF.


```
#if DOTNET2
Stream converted = new MemoryStream();

// Load the XML
XmlReader xml = new XmlTextReader("data.xml");

// Load XSL
XslCompiledTransform xsl = new XslCompiledTransform();
xsl.Load("transform.xsl");

// Do the transformation
xsl.Transform(xml, null, converted);

// Set back the position pointer in the stream to the beginning
converted.Position = 0;

XmlReader reader = new XmlTextReader(converted);
#else
// Load the XML
XmlDocument xml = new XmlDocument();
xml.Load("data.xml");

// Load XSL
XslTransform xsl = new XslTransform();
xsl.Load("transform.xsl");

// Do the transformation
XmlReader reader = xsl.Transform(xml, null);
#endif

// Construct the PDF document from the transformed XML
Document doc = new Document();
doc.Read(reader);

// Write the PDF to disk
using (FileStream file = new FileStream(
   "out.pdf", FileMode.Create, FileAccess.ReadWrite))
{
   doc.Write(file);
}
```

Code sample: Invoke the XSL transformation in C#



## Special Characters

A number of characters like the ampersand(&), less-than (<) and greater-than (>) are reserved in XML. If you want to include these in your generated PDF, you have the following options depending on where the special character occurs.


The following is invalid:


```
<paragraph type="heading" bookmark="Chapter #0 <#c>"/>
```

It should be rewritten as:


```
<paragraph type="heading" bookmark="Chapter #0 <#c>"/>
```

It should be rewritten as:


```
<paragraph type="heading" bookmark="Chapter #0 &lt;#c&gt;"/>
```

Properties of type string can be specified as a text node. Because of this you can use the CDATA construct in addition to character-escaping.


The following is invalid:


```
<fragment>
   it is true that 10 < 12
</fragment>
```




```
<fragment>
   <![CDATA[
   it is true that 10 < 12
]]>
</fragment>
```


## XML Parse Exceptions

During conversion of XML to PDF, a number of exceptions can be raised due to errors in the XML file or due to non-conformance of extended classes (see Code-Behind XML Elements). This sections lists all those exceptions and shows examples of typical mistakes. Note that these exceptions exist in addition to the Microsoft .NET Framework exceptions.



#### AttibuteNotFoundException

This exception is raised when an XML element has an attribute that does not correspond to a property.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section color="Red">
      <paragraph type="textparagraph">
         <fragment text="Hello World"/>
      </paragraph>
   </section>
</document>
```</td><td>
```
Type TallComponents.PDF.Layout.Section does not have a public property called color.
```</td></tr></table>&nbsp;
Code sample: AttibuteNotFoundException



#### ElementNotFoundException

This exception is raised when an XML element has a child element that does not correspond to a property.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section>
      <bindingsetting/>
      <paragraph type="textparagraph">
         <fragment text="Hello World">
         </fragment>
      </paragraph>
   </section>
</document>
```</td><td>
```
Type TallComponents.PDF.Layout.Section does not have a public property called bindingsetting.
```</td></tr></table>&nbsp;
Code sample: ElementNotFoundException



#### EnumConstantNotFoundException

This exception is raised when an attribute corresponds to an enumeration property but the value of the attribute does not correspond to any of the enumeration constants.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section>
      <paragraph 
         type="textparagraph"
         horizontalalignment="top">
         <fragment text="Hello World"/>
      </paragraph>
   </section>
</document>
```</td><td>
```
Enumeration TallComponents.PDF.Layout.Paragraph+HorizontalAlignment does not have a constant called top.
```</td></tr></table>&nbsp;
Code sample: EnumConstantNotFoundException



#### PropertyCannotParseException

This exception is raised when an attribute corresponds to a property but the value of the attribute cannot be parsed into a valid property value.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section oddfooter="pagenumber">
      <paragraph type="textparagraph">
         <fragment text="Hello World"/>
      </paragraph>
   </section>
</document>
```</td><td>
```
Type TallComponents.PDF.Layout.Footer does not have a Parse method, nor does it have a public static (shared) property called pagenumber.
```</td></tr></table>&nbsp;
Code sample: PropertyCannotParseException



#### MissingReadMethodException

This exception is raised when a child element cannot be read from XML because it does not have a public Read method that takes XmlReader as an argument.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section>
      <paragraph type="textparagraph">
         <fragment text="Hello World">
            <textcolor>
            </textcolor>
         </fragment>
      </paragraph>
   </section>
</document>
```</td><td>
```
Type System.Drawing.Color does not have a public method called Read that takes an XmlReader as an argument.
```</td></tr></table>&nbsp;
Code sample: MissingReadMethodException



#### MissingAddMethodException

This exception is raised when a child element can not be added to a collection because the latter does not have an Add method that takes the element as an argument.
&nbsp;<table><tr><th>
Invalid XML</th><th>
Exception.Message</th></tr><tr><td>
```
<document>
   <section>
      <paragraph type="fragment">
         <fragment text="Hello World"/>
      </paragraph>
   </section>
</document>
```</td><td>
```
Type TallComponents.PDF.Layout.Paragraphs does not have a public method called Add that takes a TallComponents.PDF.Layout.Fragment as an argument.
```</td></tr></table>&nbsp;
Code sample: MissingAddMethodException


