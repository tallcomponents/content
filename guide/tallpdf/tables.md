# Tables

A table is a specialization of Paragraph. Therefore, all members of paragraph are members of table as well. So you can align it, apply text flow constraints and add it to a section, header, footer and table cell.


A table consists of one or more rows. A rows consists of one or more cells. A cell may span more than one column. A cell consists of one or more paragraphs. Because a table is a paragraph, it is possible to nest tables.

![paragraph-table-rows-en-cells](/guide/tallpdf/media/paragraph-table-rows-en-cells.png)

The following code snippet shows how to create a table, including rows, cells and paragraphs.

```
Document doc = new Document();
Section section = new Section();
doc.Sections.Add(section);

// add a table
Table table = new Table();
section.Paragraphs.Add(table);

// add a row
Row row = table.Rows.Add();

// add a cell
Cell cell = row.Cells.Add();

// add text to the cell
TextParagraph text = new TextParagraph();
cell.Paragraphs.Add(text);
```

Code sample: Build a table in C#

## Borders and Background

You can apply both a background and a border to each table, row and cell. A background is represented by a brush. Therefore a backgound can be a solid color, a hatched pattern, etc. A border is represented by four pens that correspond to the four respective edges. The background or border(edge) can be null, meaning it does not exist.

#### Padding, Margins and Border Width

The extra space outside a border is set by a left, right, top and bottom margin. The extra space inside a border is set by a left, right, top and bottom padding. These attributes are part of the table, row or cell. The following figure makes this clear.

![table-border-padding-and-margin](/guide/tallpdf/media/table-border-padding-and-margin.png)

## Flow Constraints

In addition to the flow constraints that can be applied to any paragraph, there are some extra flow contraints that can be applied to a table. These are Table.RepeatFirstRow and Row.DoNotBreak. If RepeatFirstRow is set, then the first row will be repeated on each page if the table spans multiple pages. If Row.DoNotBreak is set, the row is kept together and not broken across pages.

## How to Size Cells

There are a number of properties that control the width of the table columns. Although table columns have no explicit representation in the DOM, they are a natural consequence of the fact that all cells that have the same position in a row have an equal width. Or stated otherwise, the vertical edges of these cells are aligned. Cells that span multiple columns are special in this regard but they do fit into this view.

The column widths are established in a multi-step process. This process and the rules that govern it, will now be discussed in detail. The following properties control this process:
&nbsp;<ul><li>
Cell.FitToContent - an attempt is made to not break paragraphs</li><li>
Cell.Fixed - the cell has a fixed width</li><li>
Cell.PreferredWidth - the preferred width; ignored if fit-to-content</li><li>
Table.ForceWidth - if true, the preferrences of the cells are overruled</li><li>
Table.PreferredWidth - if ForceWidth is true, the table has this width, otherwise ignored</li></ul>

#### Step 1: Minimum Cell Width

In the first step, the minimum width of each cell is determined. Cell width includes border, margin and padding (see Borders and Background).

The minimum cell width has a lower bound equal to the minimum width of the included paragraphs. This width depends on the type of paragraph. The minimum width of a text paragraph is the width of the largest word. The width of both a drawing and image equals their Width property. The minimum width of a table follows from the sizing process that is described in this section.

The following cases exist:

- a
- b

&nbsp;<ul><li>
_Cell.Fixed property is set_


The minimum width equals the preferred width of the cell. If the minimum width of the included paragraphs is larger than the preferred value, the minimum width is set to this value.</li><li>
_Otherwise_


The minimum width equals the the minumum width of the included paragraphs.</li></ul>

#### Step 2: Preferred Cell Width

In the next step, the preferred width of each cell is determined. The following cases exist:
&nbsp;<ul><li>
_Cell.FitToContent property is set_

The preferred width is so that each paragraph in the Cell.Paragraphs collection fits on a single line. So if the collection contains 3 paragraphs, the preferred width is so that 3 unbroken lines result.</li><li>
_Otherwise_

The preferred width equals the PreferredWidth property, unless this is smaller than the minimum width as determined in the previous step.</li></ul>

#### Step 3: Minimum and Preferred Column Width

In the previous two steps the minimum and preferred width of each individual cell are determined. The largest minimum width of all cells in a column becomes the minimum width of that column. Similarly, the largest preferred width of all cells in a column becomes the preferred width of that column.

#### Step 4: Actual Column Width

Now that each column has a minimum and preferred width, the actual column widths can be determined. This is done as follows.

In most cases the preferred width of each column also becomes the actual width. However, in a number of cases the preferred width can or will not be respected. These are:
&nbsp;<ul><li>
_Table width is forced_

If the ForceWidth property of a table is set, then the table overrules the cells. Depending on whether the forced table width is either larger or smaller than the preferred width, the columns need to grow or shrink. These cases are described next in 'Grow Columns' and 'Shrink Columns', respectively.</li><li>
_Total preferred width is too large_

If the page width does not allow the preferred column widths, then the columns shrink to the maximum available space as described next in 'Shrink Columns'.</li></ul>

#### either Step 5: Grow Columns ...

The table has a forced width that is larger than the preferred width as determined by the collection of cells. Therefore, some cells need to grow relative to their preferred width. In a first attempt, only the none-fit-to-content cells grow. If the first attempt is not sufficient, then also the fit-to-content cells grow. Growth is distributed evenly.

#### ... or Step 5: Shrink Columns

The table has a forced width that is smaller than the preferred width as determined by the collection of cells or the latter is larger than the available space. In this case, all cells shrink in an equally distributed manner. A cell never shrinks beyond its minimum width. Note that for a fixed cell, its preferred width is also its mimimum width, so it will not shrink below its preferred width.

#### Cells that Span Multiple Columns

The previous steps describe the process of establishing the size of each column. This involves minimum and preferred widths of cells inside these columns. How do cells that span multiple columns factor into this?

Steps 1, 2 and 3 as described above are really carried out from left to right. So these steps are first performed for the first column. Then they are performed for the next column, etc. Then, steps 4 and 5 follow for all columns.

In the first 3 steps, a multiple-column cell only contributes to a column if that column is the last of all spanned columns. In that case, its minimum and preferred width are reduced by the sum of the preferred and minimum width of the previous columns.

#### Table Exceeds Page Width

In a special case even the minimum width of all cells exceeds the page width. In this case the table will simply cross the right margin and possibly even the right edge of the paper.


