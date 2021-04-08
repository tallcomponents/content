# Extend TallPDF.NET

TallPDF.NET exposes its functionality through a set of cohesive .NET classes,You can extend all these classes by inheriting from them. These specialization can then be used both programmatically and from XML.



## Inherit from a TallPDF.NET Class

The following class definition shows how you can create a special Heading called H1 that encapsulates all settings that are specific to level-1 headings.


```
using TallComponents.PDF.Layout.Paragraphs;

namespace MyCompany.TallPDF
{
  public class H1 : Heading
  {
    public H1() : base(0)
    {
      Label = "Chapter";
      Bookmark = "#l #0 #c";
      Fragments.Add(new Fragment("#l #0 #c",Font.HelveticaBold, 14));
    }

    public string Text
    {
      get { return Caption; }
      set { Caption = value; }
    }
  }
}
```

Code sample: Create a specialized Heading


The following client code shows how H1 would typically be used:


```
Document document = new Document();
Section section = document.Sections.Add();
H1 h1 = new H1();
h1.Text = "Extend TallPDF.NET";
section.Add(h1);
```

Code sample: Use the specialized Heading



## Use Specialized Class in XML

Given the specialized class H1, you can now use it in XML as follows:


```
<document>
   <section>
      <paragraph type="MyCompany.TallPDF.H1">
         Hello Extended World
      </paragraph>
   </section>
</document>
```

Code sample: Use specialized class in XML


Note that the content of the text node is assigned to the Text property of H1. See also Section _Text Properties_ in Chapter [Generate PDF from XML](generate-pdf-from-xml).


**NOTE**: It is necessary to force the CLR to load the assembly that defines H1 before you do the conversion. Otherwise, the CLR will not be able to create a new instance of your custom type through the .NET reflection mechanisms. You can do this by referring a type inside this assembly or by loading the assembly explicitly by calling Assembly.LoadFrom("path to my assembly.dll" ).


