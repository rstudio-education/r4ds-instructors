# *R for Data Science Instructor's Guide*

**DRAFT:** notes for people teaching [R4DS](http://r4ds.had.co.nz/)
with each chapter's learning objectives and key points.

This work is licensed under a Creative Commons Attribution 4.0 International License.

**References**

Hadley Wickham and Garrett Grolemund:
*R for Data Science: Import, Tidy, Transform, Visualize, and Model Data*.
1st ed., O'Reilly Media, 2017.

## Learner Personas

**Nethira**, 27, is wrapping up a PhD in nursing and trying to decide
whether to do a post-doc or take a data analyst position with an NGO
in Tamil Nadu.  She did two courses on statistics as an undergraduate,
both using Stata, and picked up a bit of R from a labmate in grad
school, but has never really come to grips with it as a tool.  Nethira
would like to improve her skills so that she can finish analyzing the
data she collected for her thesis and get a couple of papers out, and
to prepare herself for a possible change of career.  These lessons
will show her how to use the tidyverse in R to clean up, analyze,
visualize, and model her data without working long nights or weekends.

**Hannu**, 40, has worked as a traffic engineer for the Finnish
Ministry of Transportation for the past 12 years, during which time he
has become proficient with SQL and Python.  As part of an open data
initiative, his department has decided to build a traffic capacity
dashboard using Shiny, and Hannu wants to learn the basics of modern R
in a hurry so that he can join this project.  These lessons will
introduce him to the packages that make up the tidyverse, and prepare
him for a deeper dive into more advanced R programming.

Derived constraints:

- Learners know what variables are, how to index a list, how loops and
  conditionals work, and grasp the basics of programming language
  syntax, such as how to write a string or a list (both).
- Learners only have a shaky grasp of variable scope and the call
  stack, and will not understand closures or higher-order functions
  without detailed exposition (Nethira).
- Learners know very basic statistics (mean, standard deviation,
  linear regression), but do not understand what a *p*-value is
  or why an observation can only be used once during hypothesis
  confirmation (Hannu).
- Learners have 20-40 hours to work through this material. They
  may be able to ask more advanced friends or colleagues for help,
  but will primarily be learning on their own and by searching
  online (both).

## Introduction

### Objectives

- Describe the steps in the basic data analysis cycle.
- Explain the relative strengths and weaknesses of visualization and modeling.
- Explain when techniques beyond those described in these lessons may be needed.
- Explain the differences between hypothesis generation and hypothesis confirmation.
- Describe and install the prerequisites for these lessons.
- Explain where and how to get help.

### Key Points

- The basic data analysis cycle is import, tidy, repeatedly transform, visualize, and model, and then communicate.
- Visualizations provide novel insight, but don't scale well.
- Models scale well, but cannot provide unexpected insight.
- The techniques described in these lessons are good for tabular (rectangular) data up to about a gigabyte in size.
- Hypothesis generation is the ad hoc process of exploring data to find possible hypotheses.
  An observation can be used many times during hypothesis generation.
- Hypothesis confirmation is the rigorous application of mathematics to test falsifiable hypotheses.
  An observation can only be used once in hypothesis confirmation.
- These lessons require R, RStudio, and a set of R packages called the tidyverse.
- Help can be found by:
  - Typing `?name` at an interactive R prompt (where `name` identifies a package, function, or variable).
  - Copying and pasting the error message into a web search.
  - Searching on Stack Overflow.
- When asking for help, create a reproducible example that:
  - Loads packages.
  - Includes a small amount of data.
  - Has short, readable code.

## Data Visualization

### Objectives

- Explain what a data frame is and how to access the ones included with the tidyverse.
- Explore the properties of a data frame.
- Explain what geometries and mappings are.
- Create a basic visualization of a single data data frame with a single geometry and a single mapping.
- Create visualizations using the `x`, `y`, `color`, `size`, `alpha`, and `shape` properties.
- Explain ways in which continuous and discrete variables should and shouldn't be visualized.
- Explain what facets are and use them to display subsets of data in a single plot.
- Create scatterplots and continuous line charts.
- Create plots that represent data in two or more ways.
- Describe three places in which the visual aspects of a plot can be specified.
- Explain what a stat is in a plot, and how stats relate to geometries.
- Create stacked and side-by-side bar charts.
- Create a scatterplot to display data with many repeated values.
- Flip the XY axes in a plot.
- Use polar coordinates in a plot.
- Describe the seven parameters to a full ggplot2 visualization.

### Key Points

- A data frame is a rectangular set of observations (rows) of the same variables (columns).
- Example data frames can be loaded using `library(tidyverse)` and then referred to by name (e.g., `mpg`) or by fully-qualified name (e.g., `ggplot2::mpg`).
- To explore a data frame:
  - Type the name of the frame to see its shape, column titles, and first few rows.
  - Use `nrow(frame)` to get the number of rows.
  - Use `ncol(frame)` to get the number of columns.
  - Use `View(frame)` in RStudio to visualize the data frame as a table.
- A geometry is an object that a plot uses to represent data, such as a scatterplot or a line.
- A mapping describes how to connect features of a data frame to properties of a geometry, and is described by an aesthetic.
- A very simple visualization has the form `ggplot(data = <DATA>) + <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))`
- Continuous variables should be visualized using smoothly-varying properties such as size and color.
- Discrete variables should be visualized using properties such as shape and line type.
- A facet is a subplot that displays a subset of the overall data.
- Use `facet_wrap(<FORMULA>, nrow=<NUM>)` with a single discrete variable as a formula (such as `~COLUMN`)
  to display a single sub-plot for each value of the discrete variable.
- Use `facet_grid(<FORMULA>)` with a formula of two discrete variables (such as `FIRST ~ SECOND`)
  to display a single sub-plot for each unique combination of the two variables.
- Use `. ~ COLUMN` or `COLUMN ~ .` as a formula to display facets in rows or columns only.
- Use `geom_point` to create a scatterplot and `geom_smooth` to create a line chart.
- Add multiple geometries after the initial call to `ggplot` 
- The visual aspects of a plot can be specified as follows (each overrides the one(s) before):
  - Globally by specifying a value in the initial `ggplot` function.
  - For a particular geometry by specifying a value outside an aesthetic.
  - In a data-dependent way by setting a property of an aesthetic.
- A stat performs a data transformation, such as counting the number of elements in a subset of the data.
- Stats and geometries can often be used interchangeably since each stat has a default geometry and each geometry has a default stat.
- Map one variable to `x` and another to `fill` in the aesthetic for `geom_bar` to create a stacked bar chart.
- Set `position="dodge"` in `geom_bar` (outside the aesthetic) to create a side-by-side bar chart.
- Use `position="jitter"` in `geom_point` (outside the aesthetic) to add randomization to a scatterplot to show data with duplication.
- Add `coord_flip` to a visualization to flip the XY axes.
- Add `coord_polar` to a visualization to use polar coordinates instead of Cartesian coordinates.
- The seven parameters of a full ggplot2 visualization are:
  - data: the data frame to be plotted
  - geometry: how the data is to be displayed (e.g., scatterplot or line)
  - mapping: how the properties of the data map to the properties of the geometry (e.g., which columsn map to X and Y coordinates)
  - stat: the transformation to apply to the data (e.g., count the number of observations)
  - position: how to adjust the positions of displayed elements (e.g., jittering points in a scatterplot)
  - coordinate function: whether to use Cartesian coordinates or polar coordinates
  - facet: how to subset the data to create multiple subplots

```
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>, 
     position = <POSITION>
  ) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION>
```

## Workflow

### Objectives

- Assign values to variables.
- Call functions.
- Write readable code.

### Key Points

- Use `name <- value` to assign a value to a variable (do not use `=`).
- Use `function(value1, value2)` to call a function.
- Construct variable names out of words joined with underscores `like_this_example`.
