---
title: "Getting started with Excel"
author: "John Ray"
date: "November 9, 2017"
layout: post

output: 
  md_document:
    variant: markdown_github
---

In this tutorial, we will walk through the following common uses of Microsoft Excel:

-   The relative reference system
-   The `$` operator, known as the 'lock' operator in Excel parlance
-   Using Excel to produce simple charts

I will be using an open-source alternative to Excel called LibreOffice, but the functionality and formulas are identical across both pieces of software. In this tutorial, I will present use cases of Excel's relative reference system, the lock operator, and its chart functionality. No prior knowledge of Excel is assumed, and no Visual Basic (the language for coding macros) is included. Readers are invited to follow along with their own data.

### The relative reference system

An important feature of how Excel organizes data is that each cell in a spreadsheet or workbook can be found using a two-coordinate system similar to that of a chess board: Each cell has a position in a column (ranging from A through Z, then AA through ZZ, and so on) and a position in a row ranging from 1 through roughly one million. For example, in the figure below, the cell referenced at `A1` contains the character string, "Hello, world!"

![A sample character string in cell A1.](figure/s1.png)

If I want to replicate the contents of that cell in another cell, I tell Excel that I want the contents of cell `A1`.

![](figure/s2a.png)

![](figure/s2b.png)

![](figure/s2c.png)

Note that I use the `=` operator to tell Excel that I want a reference to *cell* `A1` rather than, say, just writing the text string "A1" in a new cell somewhere. Writing `=A1` in a new cell would return the contents of cell `A1`, while simply writing `A1` in a new cell would create a cell that contains the phrase "A1" as a text string.

The `=` operator is a convenient feature of Excel because it allows us to work with our data without editing the data in place. For example, lets say I have two columns of numeric data and I want to create a new variable defined as the difference between each of these columns columns. Say I have the results of two theoretical elections for Democratic candidates and I want to compute the difference between the two. Here, note that the data is stored in column A, row 1 through column C, row 6. In Excel parlance we write this as \`A1:C6,' said out loud as "A1 through C6."

![](figure/s3.png)

To compute the difference between each column I'll click on the top cell of the next column, assign a new column name by entering a column name manually, and write the formula in the first cell in the row that contains data, which will be the second row of the spreadsheet.

![](figure/s4.png)

![](figure/s5.png)

Then, instead of re-entering similar code in every subsequent cell in the column, I can click on the small box in the lower-right-hand corner of my data and drag down to complete the new variable.

![](figure/s6.png)

![](figure/s7.png)

![](figure/s8.png)

And note that Excel knows to change the cell of reference from the cell we started at to the next cell *down*, to the next cell down from that, etc., because we dragged our formula *down.*

![](figure/s9.png)

I refer to Excel's click-and-drag approach to implementing formulas as the "relative reference" system using this system implies that the calculations you're running are relative to the cells to which your formulas refer. (Nobody cares where the term comes from, I just thought I'd say it)

But now lets consider a slightly different problem and sample data. Consider a case where I have some raw data, some of which shares a common denominator but not a common numerator. In the example to follow, the data in column `A` includes the names of US states, `B` contains raw votes for a theoretical Democratic candidate in a theoretical election, `C` contains raw votes for a theoretical Rebublican, and `D` contains raw votes for other parties. The data then includes a blank column, and then a column containing total votes cast in each election.

![](figure/s10.png)

Lets say that I want to know what *percent* of the vote each party earned in each state in this theoretical election rather than the raw vote totals. If I try to just click and drag as with the previous example, I successfully compute the Democratic percentage in the first column of the new data I compute, but the other two columns will break.

![](figure/s11a.png)

![](figure/s11b.png)

![](figure/s11c.png)

What's going on? When we click and drag *down* Excel computes our formula for the next cell down, but when we drag over to the right Excel also computes the formula for the next cell *over*. Look at the calculation Excel computes in the columns for `reppct` and `othpct`.

![](figure/s12a.png)

![](figure/s12b.png)

To remedy the automatic movement of the relative reference for either the row indicator, column indicator, or both, we will use the 'lock' operator, or `$`.

### The lock operator

In Excel, the `$` operator halts the movement of a relative reference either from row to row, column to column, or both. If I want to click and drag and have Excel follow me just from rows but not columns, I can force Excel to refer to the same column as I drag over by "locking" the column.

In our example, I want Excel to divide every cell in a 4x3 table, our party vote data, by every cell in a 4x1 table, our total vote data. As I move from column 1 of the party vote data to column 2 to column 3 of the party vote data, I want the denominator of the function to remain in column 1 of the 4x1 table of total votes. This is referred to as "locking" the total vote data that will go into the denominator.

To lock the denominator of the formula Excel will use to calculate votes as a percent of the state total, I use `$` to lock the column side of the relative reference. Instead of starting by writing `=F2` to reference the first cell in the total data, I write `=$F2`. Now, when I click and drag, the *row* portion of the formula is free to move, while the *column* portion of the formula is not.

![](figure/s13a.png)

![](figure/s13b.png)

![](figure/s13c.png)

One may consider that we didn't actually need that column of vote totals to make this calculation. If we did not have the vote total data, we could compute each party's vote percent in each state by dividing each party's individual counts by `dem + rep + oth`. The easiest way to do so would be to write the formula once, lock the columns using the `$` operator, then click and drag.

![](figure/s14a.png)

![](figure/s14b.png)

![](figure/s14c.png)

Then, if I want to place that data in a new column beside my original data, I will select all of the new data I have created, copy that data (`ctrl + c` on Windows and `apple + c` on), select the first cell I want to paste that data in, right click instead of press `enter`, select `Paste Special`, and paste in the *values* rather than the *formula.* We do this to deactivate relative referencing - copying-and-pasting and clicking-and dragging-both use relative referencing by default.

![](figure/s15a.png)

![](figure/s15b.png)

![](figure/s15c.png)

![](figure/s15d.png)

![](figure/s15e.png)

![](figure/s15f.png)

![](figure/s15g.png)

Likewise, if I wanted to find what percent of each party's total votes came from each state, I would lock the rows but not the columns.

![](figure/s17a.png)

![](figure/s17b.png)

![](figure/s17c.png)

And if I wanted to find what percent of *all* votes in that election went to each state in each party, I would lock both the rows and the columns.

![](figure/s18a.png)

![](figure/s18b.png)

![](figure/s18c.png)

Taking advantage of Excel's relative reference system is a great time saver. While migrating data from some resource to a spreadsheet where you can manipulate that data for your own purposes often requires some manual entry, relatively little manual coding is needed to use Excel for data management.

### Making charts

Making charts in Excel is straightforward with the exception of the way Excel deals with labels. The capacity of Excel to intelligently divine desired labels for variables, series, etc. for charts varies greatly from version to version of Excel, but in general getting the labels you want for a chart requires a little fidding. From our example, lets say I want to plot party vote percent by state. First, I would select my data and insert a chart into my spreadsheet.

![](figure/s19a.png)

![](figure/s19b.png)

![](figure/s19c.png)

![](figure/s19d.png)

![](figure/s19e.png)

Getting started with drawing plots using Excel is easy, but this looks nothing like what we want! The state labels are simply `1, 2, 3, 4`, my variable names are ugly and so is the color scheme, and the y-axis reports a decimal value rather than a percent.

Typically, I find it easiest to simply paste the data I wish to plot to a new section of the spreadsheet I am working in so that I can make a specially formatted version of the data that will look nice when plotted, without risking messing up my original raw data. Here, I simply copy-paste my data down to a new spot on the same sheet, and then manually change certain values of the data to suit my tastes.

![](figure/s20a.png)

![](figure/s20b.png)

![](figure/s20c.png)

![](figure/s20d.png)

![](figure/s20e.png)

![](figure/s20f.png)

![](figure/s20g.png)

![](figure/s20h.png)

![](figure/s20i.png)

![](figure/s20j.png)

![](figure/s20k.png)

![](figure/s20l.png)

![](figure/s20m.png)

I gloss over the coloring portion of the final two panes because there is great variation in the specifics of customizing colors, titles, etc., from Excel version to Excel version. I chose colors that correspond to those of the major parties, and gray for the third parties.
