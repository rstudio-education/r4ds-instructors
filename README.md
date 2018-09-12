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

## 1. Introduction

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

## 2. Introduction

- See above.

## 3. Data Visualization

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
  - mapping: how the properties of the data map to the properties of the geometry (e.g., which columns map to X and Y coordinates)
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

## 4. Workflow: Basics

### Objectives

- Assign values to variables.
- Call functions.
- Write readable code.

### Key Points

- Use `name <- value` to assign a value to a variable (do not use `=`).
- Use `function(value1, value2)` to call a function.
- Construct variable names out of words joined with underscores `like_this_example`.

## 5. Data Transformation

### Objectives

- Describe the five basic data transformation operations in the tidyverse and explain their purpose.
- Choose records by value using comparisons and logical operators.
- Explain why filter conditions shouldn't use `==`, and correctly use `%in%` instead.
- Explain the purpose of `NA`, how it affects arithmetic and logical operations, and how to test for it.
- Explain how filtering treats `NA` and how to obtain different behavior.
- Reorder records in ascending or descending order according to the values of one or more variables.
- Select a subset of variables for all records by variable name.
- Select and rename a subset of variables for all records by variable name.
- Add new variables to a frame by deriving new values from existing ones.
- Combine the values in a data frame to create one new value, or one new value per group.
- Explain how summarization treats missing values (`NA`) and how to change this behavior.
- Combine multiple transformations in order using pipe notation.
- Explain why and how to include counts in summarization as a check on the validity of conclusions.
- Name and describe half a dozen common summarization functions.
- Explain the relationship between grouping and summarization.

### Key Points

- The five basic data transformation operations in the tidyverse are:
  - `filter`: choose records by value(s).
  - `arrange`: reorder records.
  - `select`: choose variables by name.
  - `mutate`: derive new variables from existing ones.
  - `summarize`: combine many values to create a single new value.
- `filter(frame, ...criteria...)` keeps records that pass all of the specified criteria
  - `name == value`: records must have the specified value for the named variable (note `==` rather than `=`).
  - `name > value`: the records' values must be greater than the given value (and similarly for `>=`, `!=`, `<`, and `<=`).
  - `min_rank(name)` to rank variables, giving the smallest values the smallest ranks.
  - Use `near(expression, value)` to compare floating-point numbers rather than `==`.
  - Use `&` (and) to require both conditions, `|` (or) to accept either condition, and `!` (not) to invert the sense of a condition.
- Use `name %in% (value1, value2, ...)` to accept any of a fixed set of values for a variable.
- `NA` (meaning "not available") represents unknown values.
- Most operations involving `NA` produce `NA`, because there's no way to know the output without knowing the input.
- Use `is.na(value)` to determine if a value is `NA`.
- `filter` discards records with `FALSE` and `NA` results in tests.
  - Use `is.na(value)` to include `NA`'s explicitly.
- Use `desc(name)` to order by descending value of the variable `name` instead of by ascending value.
- Use `select(frame, name1, name2, ...)` to select only the named variables from the given frame.
- Use `name1:name1` to select all variables from `name1` to `name2` (inclusive).
- Use `-(name1:name2)` to unselect all variables from `name1` to `name2` (inclusive).
- Use `rename(frame, new_name = old_name)` to select and rename variables from a frame.
- Use `everything()` to select every variable that hasn't otherwise been selected.
- Use `one_of(c("name1", "name2"))` to select all of the variables named in the given vector.
- Use `mutate(frame, name1=expression1, name2=expression2, ...)` to add new variables to the end of the given frame.
- Use `transmute(frame, name1=expression1, name2=expression2, ...)` create a new data frame whose values are derived from the values of an existing frame.
- Use `group_by(frame, name1, name2, ...)` to group the values of `frame` according to distinct combinations of the values of the named variables.
  - Use `ungroup` to restore the original data.
- Use `summarize(frame, name = function(...))` to aggregate the values in an entire data frame or by group within a data frame.
- By default `summarize` produces `NA` as output when there are `NA`s in the input.
- Use `na.rm = TRUE` to remove `NA`s from the data before summarization.
- Use `frame %>% operation1(...) %>% operation2(...) %>% ...` to produce a new data frame by applying each operation to an existing one in order.
- Use `n()` (for a simple count) or `sum(!is.na(name))` (to count the number of non-`NA`s) when summarizing values in order to see how many records contribute to an aggregated result.
- Common summarization functions include:
  - `mean` and `median`
  - `sd` for standard deviation
  - `min`, `quantile`, and `max` for extrema and intermediate values
  - `first`, `nth`, and `last` for positional extrema and intermediate values
  - `n_distinct` for the number of distinct values
  - `count` to calculate counts or weighted sums
- Each summarization peels off one layer of grouping.

## 6. Workflow: Scripts

### Objectives

- Use the RStudio editor to write, save, and run R scripts.
- Describe two things that should *not* be put in scripts.
- Explain how to spot and fix syntax errors in the RStudio editor.

### Key Points

- Use Cmd/Ctrl + Enter in the editor to run the current R expression in the console.
- Use Cmd/Ctrl + Shift + S to run the complete script in the console.
- Do not put `install.packages` or `setwd` in scripts, since they will affect other people's machines when run.
- The RStudio editor uses white-on-red X's and red squiggly underlining to highlight syntax errors.

## 7. Exploratory Data Analysis

### oObjectives

- Describe the steps in exploratory data analysis (EDA).
- Describe two types of questions that are useful to ask during EDA.
- Correctly define *variable*, *value*, *observation*, *variation*, and *tidy data*.
- Explain what a *categorical variable* is and how to best to store and visualize one.
- Explain what a *continuous variable* is and how best to store and visualize one.
- Explain why it is important to use a variety of bin widths when visualizing continuous variables as histograms.
- List three questions whose answers will help you understand your data.
- Describe and use a heuristic for identifying subgroups in data.
- Explain how to handle outliers or unusual values in data.
- Define *covariance* and describe how to visualize it for different combinations of two categorical and continuous variables.
- Explain how to make code clearer to experienced readers by omitting information.

### Key Points

- Exploratory data analysis consists of:
  - Generating questions about data.
  - Searching for answers by visualizing, transforming, and modeling data.
  - Using what is found to refine questions or generate new ones.
- Two questions that are always useful to ask during EDA are:
  - What type of variation occurs within my variables?
  - What type of covariation occurs between my variables?
- A *variable* is something that can be measured.
- A *value* is the state of a variable when measured.
- An *observation* is a set of measurements made under similar conditions, and may contain several values (each associated with a different variable).
- *Variation* is the tendency of values to differ from measurement to measurement.
- *Tidy data* is a set of values, each of which is associated with exactly one variable and observation.
  Tidy data is usually displayed in tabular form: each observation (or record) is a row, while each variable is a column with a name and a type.
- A *categorical variable* is one that takes on only one of a small set of values.
  - Categorical variables are best represented using factors or character strings.
  - The distribution of a categorical variable is best visualized using a bar chart (created using `geom_bar`).
  - `dplyr::count(name)` counts the number of occurrences of each value of a categorical variable.
- A *continuous variable* is one that takes on any of an infinite set of ordered values.
  - Categorical variables are best represented using numbers or date-times.
  - The distribution of a categorical variable is best visualized using a histogram.
  - `dplyr::count(ggplot2::cut_width(name, width))` divides occurrences into bins and counts the number of occurrences in each bin.
- Histograms with different bin widths can have very different visual appearances, so varying the bin width provides insight that no single bin width can.
  - Use `geom_histogram(mapping=..., binwidth=value)` to vary the width of histogram bins.
  - Or `geom_freqpoly` to display histograms using lines instead of bars.
- Three questions to ask of any dataset are:
  - Which values are most common (and why)?
  - Which values are rare (and why)?
  - What patterns are present in the data?
    - How can you describe the pattern?
    - How strong is it?
    - Is it a coincidence?
    - Does the pattern change if you examine subgroups of the data?
- Clusters of similar values suggest that data contains subgroups.  To characterize these subgroups, ask:
  - How are observations in each cluster similar?
  - How do observations in different clusters differ?
  - What might explain the existence of these clusters?
  - How might the appearance of these clusters be misleading (e.g., an artifact of the visualization used)?
- If outliers are present, repeat each analysis with and without them.
  - If there are only a few, and dropping them doesn't affect results, use `mutate` and `ifelse` to replace them with `NA`.
  - If there are many, or dropping them changes results, account for them in analysis and reporting.
- *Covariation* is the tendency for some variables to vary in related ways.
- When visualizing the relationship between continuous and categorical variables:
  - Displaying raw counts can be misleading if the number of items in different categories varies widely.
  - Displaying *densities* (i.e., counts standardized so that the area of each curve is the same) can be more informative.
  - Boxplots show less of the raw data, but are easier to interpret when there are many categories.
  - Reorder unordered categorical variables to make trends easier to see.
- When visualizing the relationship between two categorical variables:
  - Display the counts for each pairing of values (e.g., using `geom_count` or `geom_tile`)
  - In general, put the categorical variable with the greater number of categories or the longer labels on the Y axis.
- When visualizing the relationship between two continuous variables:
  - Use a scatterplot with jittering or transparency to handle datasets with up to hundreds of points.
  - Use `geom_bin2d` or `geom_hex` to bin values in two dimensions.
  - Bin one or both of the continuous variables so that visualizations for continuous variables can be used.
  - Use `cut_width` or `cut_number` to bin continuous values by value range or number of values respectively.
- Omitting argument names for commonly-used functions makes code easier for experienced programmers to understand.
  - The first two arguments to `ggplot` are the dataset and the mapping.

## 8. Workflow: Projects

### Objectives

- Explain why analysts should save scripts rather than environments.
- Explain what a *working directory* is and how to find what yours is.
- Explain why setting your working directory from within your script is a bad idea.
- Explain the difference between an *absolute path* and a *relative path* and the meaning of the symbol `~` in a path.
- Explain what an RStudio project is and how one is stored.

### Key Points

- Analysts should save scripts rather than environments because it is much easier to reconstruct an environment from a script than to reconstruct a script from an environment.
- The *working directory* is the directory where R looks for and saves files by default, and is displayed by calling `getwd()`.
- Setting the working directory from within a script with `setwd` makes reproducibility more difficult because that directory may not exist on some other (person's) machine.
- An *absolute path* specifies a single location starting from the top of the filesystem.
- A *relative path* specifies a location starting from the current directory, and may identify different locations depending on where it is used.
- The symbol `~` refers to the user's home directory on macOS and Linux, and to the user's `Documents` directory on Windows.
- An RStudio project is a directory that contains the scripts and other files involved in an analysis.
- Each RStudio project contains a `.Rproj` file with information about the project.
