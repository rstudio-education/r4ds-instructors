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
  conditionals work, and grasp the basics of programming language syntax, such
  as how to write a string or a list (both).
- Learners only have a shaky grasp of variable scope and the call stack, and
  will not understand closures or higher-order functions without detailed
  exposition (Nethira).
- Learners know very basic statistics (mean, standard deviation, linear
  regression), but do not understand what a *p*-value is or why an observation
  can only be used once during hypothesis confirmation (Hannu).
- Learners have 20-40 hours to work through this material. They may be able to
  ask more advanced friends or colleagues for help, but will primarily be
  learning on their own and by searching online (both).

Note: definitions of terms are marked with `_single underscores_`, while other
form of emphasis uses `*single asterisks*`.  This makes it easy to extract
definitions for glossary construction.

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

### Objectives

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
- A _variable_ is something that can be measured.
- A _value_ is the state of a variable when measured.
- An _observation_ is a set of measurements made under similar conditions, and may contain several values (each associated with a different variable).
- _Variation_ is the tendency of values to differ from measurement to measurement.
- _Tidy data_ is a set of values, each of which is associated with exactly one variable and observation.
  Tidy data is usually displayed in tabular form: each observation (or record) is a row, while each variable is a column with a name and a type.
- A _categorical variable_ is one that takes on only one of a small set of values.
  - Categorical variables are best represented using factors or character strings.
  - The distribution of a categorical variable is best visualized using a bar chart (created using `geom_bar`).
  - `dplyr::count(name)` counts the number of occurrences of each value of a categorical variable.
- A _continuous variable_ is one that takes on any of an infinite set of ordered values.
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
- _Covariation_ is the tendency for some variables to vary in related ways.
- When visualizing the relationship between continuous and categorical variables:
  - Displaying raw counts can be misleading if the number of items in different categories varies widely.
  - Displaying _densities_ (i.e., counts standardized so that the area of each curve is the same) can be more informative.
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
- The _working directory_ is the directory where R looks for and saves files by default, and is displayed by calling `getwd()`.
- Setting the working directory from within a script with `setwd` makes reproducibility more difficult because that directory may not exist on some other (person's) machine.
- An _absolute path_ specifies a single location starting from the top of the filesystem.
- A _relative path_ specifies a location starting from the current directory, and may identify different locations depending on where it is used.
- The symbol `~` refers to the user's home directory on macOS and Linux, and to the user's `Documents` directory on Windows.
- An RStudio project is a directory that contains the scripts and other files involved in an analysis.
- Each RStudio project contains a `.Rproj` file with information about the project.

## 10. Tibbles

### Objectives

- Explain the relationship between a tibble and a `data.frame` and the main ways in which tibbles differ from `data.frame`s.
- Create tibbles from `data.frame`s and from scratch.
- Explain what a *non-syntactic name* is and how to create tibble columns with non-syntactic names.
- FIXME: explain how to use `tribble` (which requires an understanding of `~`).
- Display an arbitrary number of rows and columns of a tibble.
- Subset tibbles using `[[...]]`.
- Subset tibbles using `$`.
- FIXME: explain use of `[...]` (single bracket).

### Key Points

- A tibble is a `data.frame` whose behaviors have been modified to work better with the tidyverse.
  - Tibbles never change their inputs' types.
  - Tibbles never adjust the names of variables.
  - Tibbles evaluate their constructor arguments lazily and sequentially, so that later variables can use the values of earlier variables.
  - Tibbles do not create row names.
  - Tibbles only recycle inputs of length 1, because recycling longer inputs has been a frequent source of bugs.
- Tibbles can be created from `data.frame`s using `as_tibble` or from scratch using `tibble`.
  - Use `is_tibble` to determine if something is a tibble or not.
  - Use `class` to determine the classes of something.
- A _non-syntactic name_ is one which is not a valid R variable name.
- To create a non-syntactic column name, enclose the name in back-quotes.
- Use `print` with `n` to set the number of rows and `width` to set the number of character columns.
- Use `name[["variable"]]` or `name$variable` to extract the column named `variable` from a tibble.
- Use `name[[N]]` to extract column `N` (integer) from a tibble.

## 11. Data Import

### Objectives

- Name six functions for reading tabular data and explain their use.
- Read CSV data files with multiple header lines, comments, missing headers, and/or markers for missing data.
- Explain how data reader functions determine whether they have extra and missing values, and how they handle them.
- Name four functions used to parse individual values and explain their use.
- Explain how to obtain a summary of parsing problems encountered by data reading functions.
- Define *locale* and explain its purpose and use.
- Define *encoding* and explain its purpose and use.
- Explain how `readr` functions determine column types.
- Set the data types of columns explicitly while reading data.
- Explain how to write well-formatted tabular data.
- Describe what information is lost when writing tibbles to delimited files and what formats can be used instead.

### Key Points

- Use the following functions to read tabular data in common formats:
  - `read_csv`: comma-delimited files.
  - `read_csv2`: semicolon-delimited files.
  - `read_tsv`: tab-delimited files.
  - `read_delim`: files using an arbitrary delimiter.
  - `read_fwf`: files with fixed-width fields.
  - `read_table`: read common fixed-width tabular formats with whitespace separators.
- Use `skip=n` to skip the first N lines of a file.
- Use  `comment="#"` (or something similar) to ignore lines starting with `#`.
- Use `col_names=FALSE` to stop `read_csv` from interpreting the first row as column headers.
- Use `col_names=c("first", "second", "third")` to specify column names by hand.
- Use `na="."` (or something similar) to specify the value(s) used to mark missing data.
- Data reader functions use the number of values in the first row to determine the number of columns.
  - Extra values in subsequent rows are omitted.
  - Missing values in subsequent rows are set to `NA`.
- Use `parse_integer`, `parse_number`, `parse_logical`, and `parse_date` to parse strings containing integers, general numbers, Booleans, and dates.
  - Use `na="."` (or similar) to specify the value(s) that should be interpreted as missing data.
- Use `problems(name)` to access the `problems` attribute of the output of data reading functions.
- A _locale_ is a collection of linguistic and/or regional settings for information formats, such as Canadian English or Brazilian Portuguese.
  - Use `locale(...)` to specify such things as the separator character used in long numbers.
- An _encoding_ is a specification of how characters are represented digitally, such as ASCII or UTF-8.
  - Specify `encoding="name"` when parsing data to interpret the character data correctly.
  - UTF-8 is now the most commonly-used character encoding scheme.
- Data reading functions read the first 1000 rows of the dataset and use the heuristics embodied in `guess_parser` to guess the type of the column.
- Use `col_types=cols(...)` to manually specify the types of the columns of a data file.
  - Use `name = col_double()` to set the column's name to `name` and the type to `double`.
  - Use `name = col_date()` to set the name to `name` and the type to `date`.
- Use `write_csv` to write data in comma-separated format and `write_tsv` to write it in tab-separated format.
  - Use `write_excel_csv` to write CSV with extra information so that it can immediately be loaded by Microsoft Excel.
  - Use `na="marker"` to specify how `NA` should be shown in the output.
- Delimited file formats only store column names, not column types, so the latter have to be re-guessed when the file is re-read.
  - (Old) Saving data in R's custom binary format RDS will save type information.
  - (New) Saving data in the cross-language Feather format will also save type information, and this data can be read in multiple languages.

## 12. Tidy Data

### Objectives

- Describe the three rules tabular data must obey to be considered "tidy", and the advantages of storing data this way.
- Explain what *gathering* data means and use gather operations to tidy datasets.
- Explain what *spreading* data means and use spread operations to tidy datasets.
- Explain what *separating* data means and use separate operations to tidy datasets.
- Explain what *uniting* data means and use unite operations to tidy datasets.
- Describe two ways in which values can be missing from a dataset.
- Explain how to *complete* a dataset and use completion operations to tidy datasets.
- Explain why it can be useful to carry values forward and use this to tidy datasets.

### Key Points

- Tidy data obeys three rules:
  - Each variable has its own column.
  - Each observation has its own row.
  - Each value has its own cell.
- Tidy data is easier to process because:
  - No subsidiary processing is required (e.g., to split names into personal and family names).
  - Each column can be processed independently (e.g., there's no need to choose the type of processing based on a "type" field in another column).
- To _gather_ data means to take N columns whose names are actually values and transform them into 2 columns where the first column holds the former column names and the second holds the values.
  - Use `gather(name, name, ..., key="key_name", value="value_name")` to transform the named columns into two columns with names `key_name` and `value_name`.
- To _spread_ data means to take two columns, the N values in the first of which identify the meanings of the values in the second, and create N+1 columns, one for each of the distinct values in the first column.
  - Use `spread(key=first, value=second) to spread the values in `second` according to the keys in `first`.
- To _separate_ data means to split one column into multiple values.
  - Use `separate(name, into=c("first", "second", ...))` to separate the values in one column to create multiple new columns.
- To _unite_ data means to combine the values of two or more columns into a single column.
  - Use `unite(new_name, first, second, ...)` to combine the named columns to create a column named `new_name`.
  - Values will be combined with `_` unless `sep="#"` (or similar) is used (with `sep=""` to unite without a separator).
- Use `convert=TRUE` with these functions to (try to) convert data types.
- Values can be explicitly missing (the presence of an absence) or their entries can be missing entirely (the absence of a presence).
- To _complete_ a dataset means to fill in missing combinations of values.
  - Use `complete(first, second, ...)` to fill in missing combinations of the values from the named columns.
- Missing values sometimes indicate that the most recent value should be carried forward.
  - Use `fill(first, second, ...)` to carry the most recent observation(s) forward in the named column(s).

## 13. Relational Data

### Objectives

- Define *relational data* and explain what *keys* are and how they are used when processing it.
- Explain the difference between a *primary key* and a *foreign key*, and explain how to determine whether a key is actually a primary key.
- Explain what a *surrogate key* is and why surrogate keys are sometimes needed.
- Explain how relations are represented in relational data and describe three types of relations.
- Define a *mutating join* and use mutating joins to combine information from two tables.
- Define four kinds of joins and use each to combine information from two tables.
- Explain what joins do if some keys are duplicated, and when this might occur.
- Describe and use some common criteria for joins.
- Define a *filtering join*, describe two types of filtering joins, and use them to combine information from two tables.
- Describe the difference between how mutating joins and filtering joins behave in the presence of duplicated keys.
- Describe three steps for identifying keys in tables that can be used in joins.
- Describe and use three set operations on records.

### Key Points

- _Relational data_ is made up of sets of tables that are related in some way.
- A _key_ is a variable or set of variables whose values uniquely identify observations in a table.
  - Keys are used to connect observations in one table to observations in another.
- A _primary key_ uniquely identifies an observation in its own table.
  - Use `count(name)` and `filter(n > 1)` to identify multiple occurrences of what is supposed to be a primary key.
- A _foreign key_ uniquely identifies an observation in some other table, and is used to connect information between those tables.
- A _surrogate key_ is an arbitrary identifier associated with an observation (such as a row number) that has no real-world meaning.
  - Surrogate keys are sometimes added to data when the data itself has no valid primary keys.
- Relations are represented by matching primary keys in one table to foreign keys in another.  Relations can be:
  - _One-to-one_ (or 1-1), meaning there is exactly one matching value in each table.
  - _One-to-many_ (or 1-N), meaning that each value in one table may have any number of matching values in another.
  - _Many-to-many_ (or N-N), meaning that there may be many matching values in each table.
- A _mutating join_ updates one table with corresponding information from another table.
- An _inner join_ combines observations from two tables when their keys are equal, discarding any unmatched rows.
  - Use `inner_join(left, right, by="name")` to join tables `left` and `right` on equal values of the column `name`.
- A _left outer join_ (or simply _left join_) combines observations when keys are equal, keeping rows from the left table even if there are no corresponding values from the right table.
  - Missing values from the right table are assigned `NA` in the result.
  - Use `left_join` with arguments as above.
- A _right outer join_ (or simply _right join_) does the same, but keeps rows from the right table even when rows from the left are missing.
  - Use `right_join` with arguments as above.
- A _full outer join_ (or simply _full join_) keeps all rows from both table, filling in for gaps in either.
  - Use `full_join` with arguments as above.
- If a key is duplicated in one or both tables, a join will produce all combinations of records with that key.
  - This often arises when a key is a primary key in one table and a foreign key in another.
  - If keys are duplicated in both tables, it may be a sign that the data is corrupt or that the supposed key actually isn't one.
- A _natural join_ combines tables using equal values for all columns with identical names.
  - Use `by=NULL` in a join function to force a natural join.
- Use `by=c("name1", "name2", ...)` to join on equal values of named columns.
- Use `by=c("a" = "b", "c" = "d", ...)` to join on columns with different names.
- Use `suffix=("name", "name")` to override the default `.x`, `.y` suffixes used for name collisions.
- A _filtering join_ is one that keeps (or discards) observations from one table based on whether they match (or do not match) observations in a second table.
  - Use `semi_join(left, right)` to keep rows in `left` that have matches in `right`.
  - Use `anti_join(left, right)` to keep rows in `left` that do *not* have matches in `right`.
- Because they only keep or discard rows, filtering joins never create duplicate entries, while mutating joins can if keys are duplicated.
- Three steps for identifying keys in tables that can be used in joins are:
  - Identify the variable or variables that form the primary key for each table based on an understanding of the data.
  - Check that each table's primary key has no missing values.
  - Check that possible foreign keys match primary keys in other tables (e.g., by using `anti_join` to look for missing matches).
- Three set operations that work on entire records are:
  - `union(left, right)`: returns unique observations from either or both table.
  - `intersect(left, right)`: returns unique observations that are in both tables.
  - `setdiff(left, right)`: returns observations that are in one of the tables but not both.

## 14. Strings

### Objectives

- Write character strings in R, including ones that contain special characters.
- Write multiple strings to the terminal, respecting escaped characters.
- Use functions from the `stringr` package to perform basic operations on strings.
- Explain what a *regular expression* is and what kinds of patterns they can match.
- Describe two functions that implement regular expressions and use them to match simple patterns against text.
- Describe nine patterns provided by regular expressions.
- Capture subsections of matched text in regular expressions and re-match captured text within a pattern.
- Detect and extract matches between a pattern and the strings in a vector.
- Replace substrings that match regular expressions.
- Split strings based on regular expression matches.
- Locate substrings that match regular expressions.
- Control matching options in regular expressions.
- Find objects in the global environment whose names match a regular expression.
- Find files and directories whose names match a regular expression.

### Key Points

- Character strings in R are enclosed in matching single or double quotes.
- Use backslash to escape special characters such as `\"`, `\n`, and `\\`.
- Use `writeLines` to display a string or a vector of strings with special characters interpreted.
- Use `str_length` to get a string's length.
- Use `str_c` to concatenate strings.
- Use `str_sub` to extract or replace substrings.
- Use `str_to_lower`, `str_to_upper`, and `str_to_title` to change the case of strings.
- Use `str_sort` to sort a vector of strings and `str_order` to get the 
- Use `str_order` to get the ordered indices of the strings in a vector.
- Use `str_pad` to pad a string to fit a specified width and `str_trim` to trim it to fit that width.
- A _regular expression_ is a pattern that matches text.
  - Regular expressions are written as text using punctuation and other characters to express choice, repetition, and other operations.
  - Regular expressions can express patterns that have fixed nesting, but not patterns that have unlimited nesting (such as nested parenthesization).
- Use `str_view(text, pattern)` to find the first match of `pattern` to `text` and `str_view_all` to view all matches.
- Nine patterns used in regular expressions are:
  - `.` matches any single character.
  - `\` escapes the character that follows it.
  - `^` and `$` match the beginning and end of the string respectively (without consuming any characters).
  - Use `\d` to match digits and `\s` to match whitespace.
  - Use `[abc]` to match any single character in a set and `[^abc]` to match any character *not* in a set.
  - Use `left|right` to match either of two patterns.
  - Use `{M,N}` to repeat a pattern M to N times.
  - Use `?` to signal that a pattern is optional (i.e., repeated zero or one times), `*` to repeat a pattern zero or more times, and `+` to repeat a pattern at least once.
  - Use parentheses `(...)` for grouping, just as in mathematics.
- Every set of parentheses in a regular expression creates a numbered _capture group_.
  - Use `\1`, `\2`, etc. to refer to capture groups within a pattern in order to match the same actual text two or more times.
- Use `str_detect(strings, pattern)` to create a logical vector showing where a pattern does or doesn't match.
- Use `str_subset(strings, pattern)` to select the subset of strings that match a pattern and `str_count` to count the number of matches.
- Use `str_extract(strings, pattern)` to extract the first match for the pattern in each string.
- Use `str_extract_all(strings, pattern)` to extract all matches for the pattern in each string.
- Use `str_match(string, pattern)` to extract parenthesized sub-matches for a pattern.
- Use `tidyr::extract` to extract parenthesized sub-matches from a tibble into new columns.
- Use `str_replace` or `str_replace_all` to replace substrings that match regular expressions.
- Use `str_split` to split a string based on regular expression matches.
- Use `str_locate` and `str_locate_all` to find the starting and ending positions of substrings that match regular expressions.
- Use `regex` explicitly to construct a regular expression and control options such as multi-line matches and embedded comments.
- Use `apropos` to find objects in the global environment whose names match a regular expression.
- Use `dir` to find objects in the filesystem whose names match a regular expression.

## 15. Factors

### Objectives

- Define *factor* and explain the purpose of factors in R.
- Create and (re-)order factors.
- Determine the valid levels of a factor.
- Rename the levels of a factor.

### Key Points

- A _factor_ is a variable that can take on one of a fixed set of values.
  - Factors are ordered, but the order is not necessarily alphabetical.
- Use `factor(values, levels)` to create a vector of factors by matching strings in `values` to level names in `levels`.
  - Values that don't match level names are converted to `NA`.
- The idiom `factor(values, unique(values))` orders the factors according to their first appearance in the data.
  - Use `fct_reorder(factor, values)` to reorder a factor according to a set of numeric values.
- Use `levels(factor)` to recover the valid levels of a factor.
- Use `fct_relevel(factors, "levels")` to move the named levels to the front of the list of factors (e.g., for display purposes).
- Use `fct_infreq` to reorder factors by frequency.
- Use `fct_rev` to reverse the order of factors.
- Use `fct_recode(factor, "new_name_1" = "old_name_1", "new_name_2" = "old_name_2", ...)` to rename some or all factors.
  - Assigning several old levels to a single new level combines entries.
  - Use `fct_collapse(factors, new_name = c("old_name_1", "old_name_2"), ...)` to collapse many levels at once.
- Use `fct_lump(factor, n=N)` to combine the smallest factors, leaving `N` groups.

## 16. Dates and Times

### Objectives

- Describe three types of data that refer to an instant in time.
- Get the current date and time.
- Describe and use three ways to create a date-time.
- Convert dates to date-times and date-times to dates.
- Describe and use eight accessor functions to extract components of dates and date-times.
- Describe and use three functions for rounding dates.
- Explain how to modify components of dates and date-times.
- Explain an idiom for exploring patterns in the lower-order components of date-times.
- Explain how the difference between two moments in time is represented in base R and when using `lubridate`.
- Explain the difference between a difftime, a *period*, and an *interval*.
- Determine your current timezone.

### Key Points

- Instants in time are described by _date_, _time_, and _date-time_.
- Use `today` to get the current date and `now` to get the current date-time.
- A date-time can be created from a string, from individual date and time components, or from an existing date-time.
  - Use `lubridate` functions such as `ymd` or `dmy` to parse year-month-day dates.
  - Use functions such as `ymd_hms` to parse full date-times.
  - Supplying a timezone with `tz="XYZ"` forces the creation of a date-time instead of just a date.
  - Use `make_date` or `make_datetime` to construct a date or date-time from numeric components.
- Use `as_datetime` to convert a date to a date-time and `as_date` to convert a date-time to a date.
- Use the following accessor functions to extract components from dates and date-times:
  - `year` and `month`
  - `yday` (day of the year), `mday` (day of the month), and `wday` (day of the week)
  - `hour`, `minute`, and `second`
- Use `floor_date`, `round_date`, and `ceiling_date` to round dates down, to nearest, or up to a specified unit.
- Use an accessor function on the left side of assignment to modify a portion of a date or date-time in place.
  - E.g., use `year(x) <- 2018` to set the year of a date or date-time to 2018.
- Use `update(existing, name=value, ...)` to create a new date-time with modified values.
- Use `update` to set the higher-order components of date-times to a constant in order to explore the variation in the lower-order components.
- A _difftime_ represents the absolute difference between two moments in time (in seconds).
  - Use `as.duration(difftime)` to convert to a `lubridate` `duration`, which always uses seconds to represent differences in times.
  - Use `dyears`, `dseconds`, etc. to construct differences explicitly.
- A _period_ represents the difference between two times taking human factors into account (such as daylight savings time).
- An _interval_ is a duration with a starting point, which makes it precise enough that its exact length can be determined.
- Use `Sys.timezone()` to determine your current timezone.

## 18. Pipes

### Objectives

- Describe the pros and cons of four ways to write successive operations on data.
- Explain the use of `%T>%`, `%$%`, and `%<>%`.

### Key Points

- Four ways to write successive operations on data are:
  - Save each intermediate step as a new object: a lot of typing with many opportunities for transposition mistakes.
  - Overwrite the original object many times: loss of originals makes debugging difficult, and repetition of a single makes reading difficult.
  - Compose functions: unnatural reading order and parameters widely separated from function names.
  - Use the pipe `%>%`: simple to read *if* the transformations are sequential and applied to a single main stream of data.
- `%T>%` ("tee") returns its left side rather than its right.
- `%$%` unpacks the variables in a `data.frame` (which is useful when calling functions in base R that don't rely on `data.frame`s).
- `%<>%` assigns the result back to the starting variable.

## 19. Functions (and Control Flow)

### Objectives

- Explain the benefits of creating functions.
- Describe three steps in the creation of a function.
- Define functions of zero or more arguments.
- Describe three rules that function names should follow.
- Describe the difference between data and details in function arguments.
- Define *conditional statement* and write conditional statements with multiple branches and a default branch.
- Explain what a *short-circuit operator* is and write conditions using these operators.
- Define *precondition* and implement preconditions in functions.
- Write functions that take (and pass on) a varying number of arguments.
- Describe and use two ways to return values from functions.
- Implement pipeable functions that perform transformations or have side effects.

### Key Point

- Create functions to elminate duplicated code, make programs more readable, and simplify maintenance and evolution.
- When creating a function, select a name, decide on its arguments, and write its body.
- Function names should:
  1. Prefer verbs (actions) to nouns (things).
  2. Use full words and consistent typography.
  3. Be consistent with other functions in the same package or application.
- Arguments to functions are (broadly speaking) either:
  - Data to be operated on (come first).
  - Details controlling how the function operates (come last, and should have default values).
  - When overriding the value of a default, use the full name of the argument.
- A _conditional statement_ may or may not execute code depending on whether a condition is true or false.
  - Each conditional statement must have one `if`, zero or more `else if`, and zero or one `else` in that order.
  - Each branch except the `else` must have a logical condition that determines whether it is selected.
  - Branch conditions are tested in order, and only the code associated with the first branch whose condition is true is executed.
  - If no condition is true, and an `else` is present, the code in the `else` branch is executed.
- Conditions must be `TRUE` or `FALSE`, not vectors or `NA`s.
- A _short-circuit operator_ stops evaluating terms as soon as it knows whether the overall value is `TRUE` or `FALSE`.
  - "and", written `&&`, stops as soon as a term is `FALSE`.
  - "or", written `||`, stops as soon as a term is `TRUE`.
  - Use the functions `any`, `all`, and `identical` to collapse vectors into single values for testing.
- Always indent the bodies of conditionals and functions (preferably by two spaces) and obey style rules for placement of curly braces.
- A _precondition_ is something that must be true of a function's inputs in order for the function to work correctly.
  - Use `if` and `stop` to check that inputs are sensible before processing it, and generate a meaningful error message when it's not.
  - Or use `stopifnot` to check that one or more conditions are true (without generating a custom error message).
- Use `...` (three dots) as a placeholder for zero or more arguments, which can then be passed into other functions.
  - Use `list(...)` to convert the actual arguments to a list for processing.
- A function in R returns either:
  - An explicit value when `return(value)` is called.
  - The value of the last expression evaluated if no explicit `return` was executed.
- To make a function pipeable:
  - For a transformation, take the data to be transformed as the first argument and return a modified object.
  - For a side effect, perform the operation (e.g., save to a file) and use `invisible(value)` to return the value without printing it.

## 20. Vectors

### Objectives

- Define *atomic vector* and *list*, explain the differences between them, and give examples of each.
- Explain what `NULL` is used for and how it differs from `NA`.
- Determine the type and length of an arbitrary value.
- Describe the values that logical vectors can contain and how they are usually constructed.
- Describe the values that integer and double vectors can contain and the special values that each type can contain.
- Describe the values that character vectors can contain.
- Explain the difference between *explicit coercion* and *implicit coercion* and use the former to convert values from one type to another.
- Explain the rule used to determine the type of a vector explicitly constructed out of values of different types.
- Define *recycling* and correctly identify and interpret uses of it.
- Recycle values explicitly.
- Give vector elements names and explain when and why this is useful.
- Describe six ways to subset a vector.
- Explain the difference between single `[...]` and double `[[...]]`
- Create and inspect lists.
- Subset lists.
- Define *attribute* and *augmented vector*.
- Describe three ways vector attributes are used in R.
- Explain how factors are implemented using augmented vectors.
- Explain how tibbles are implemented using augmented vectors.

### Key Points

- An _atomic vector_ is a homogeneous structure that holds logical, integer, double, character, complex, or raw data.
- A _list_ (sometimes called a _recursive vector_) is a vector that can hold heterogeneous data, including other vectors.
- The special value `NULL` represents the absence of a vector, or a vector of length zero, while `NA` represents the absence of a value.
- Use `typeof(thing)` to obtain the name of the type of `thing`.
  - Use `is_logical`, `is_integer`, and similarly-named functions to test the types of values.
  - Use `is_scalar_integer` and similarly-named functions to test the type of a value and whether it is scalar or vector.
- Use `length(thing)` to obtain the (integer) length of the type of `thing`.
- Logical vectors can contain `TRUE`, `FALSE`, and `NA`, and are often constructed using Boolean expressions such as comparisons.
- Integer vectors contain integer values, which should be used for counting.
  - To force a value to be stored as an integer, write it without a decimal portion and put `L` after it (for "long").
  - Integer vectors can contain the special value `NA`.
  - Use `is.na` to check for this special value.
- Double vectors contain floating-point numbers, which should be used for measurement.
  - Double vectors can contain the special values `NA`, `NaN` (not a number), `Inf` (infinity), and `-Inf` (negative infinity).
  - Use `is.finite`, `is.infinite`, and `is.nan` to check for these special values.
- Character vectors can contain character strings, each of which can be arbitrarily long.
- _Explicit coercion_ is the use of a function to convert values from one type to another.
  - Use `as.logical`, `as.character`, `as.integer`, or `as.double` to create a new vector containing the converted values from an original.
- _Implicit coercion_ occurs when a value or vector of one type is used where another type is expected.
- The function `c(value1, value2, ...)` creates a vector whose type is the most complex of the types of the provided values.
  - In order of increasing complexity, types are logical - integer - double - character.
- To _recycle_ values is to re-use those from the shorter vector involved in an operation to match the length of the longer vector.
  - A "scalar" in R is actually a vector of length 1, and most recycling involves replicating a scalar to have the same length as a vector.
  - Base R produces a warning if the length of the longer is not an integer multiple of the length of the shorter.
  - Tidyverse functions throw errors in this case to forestall unexpected results.
- Use `rep(values, times)` to recycle (or repeat) values explicitly.
- Some or all vector elements can be given names when the vector is constructed using `c(name1=value1, ...)`.
  - Use `purrr:set_names(vector, names)` to set the names of a vector's values after that fact.
- A vector can be subsetted using `[...]` in four ways:
  - Subsetting with a vector of positive integers selects those elements in order (possibly with repeats).
  - Subsetting with a vector of negative integers selects all elements *except* those identified.
  - Subsetting with zero creates an empty vector.
  - Subsetting with a logical vector keeps values corresponding to `TRUE` elements of the logical vector.
  - Subsetting with a character vector keeps only those values with the given names (possibly with repeats).
  - Using an empty subscript `[]` returns the entire vector.
- Create lists using `list(value1, value2, ...)` and inspect their structure with `str(name)`.
- Subsetting a list with `[...]` always returns a list.
- Subsetting a list with `[[...]]` returns a single component (i.e., has one less level of nesting than the original).
- `list$name` does the same thing as `[[...]]` for a named element of a list.
- An _augmented vector_ is one that has extra named _attributes_ attached to it.
- Get the value of a vector attribute using `attr(vector, name)`.
- Set the value of a vector attribute using `attr(vector, name) <- value`.
- Use `attributes(name)` to display all of the attributes of a vector.
- Vector attributes are used to:
  - Name the elements of a vector (`names`).
  - Store dimensions to make a vector behave like a matrix.
  - Store a class name to implement classes in the S3 object-oriented system.
- A factor is an integer vector that has the class `factor` and a `levels` attribute with the factors' names.
- A tibble is a list with three classes and `names` and `row.names` attributes.
  - All elements of a tibble must be vectors having identical lengths.

## 21. Iteration

### Objectives

- Describe the parts of a simple `for` loop.
- Create empty vectors of a given type and length.
- Explain why it is safer to use `seq_along(x)` than `1:length(x)`.
- Write loops that iterate over the columns of a tibble using either indices or names.
- Describe and use an efficient way to write loops when the size of the eventual output cannot be known in advance.
- Explain how to write a loop when the number of required iterations is not known in advance.
- Describe what happens when looping over the names of a vector that has some unnamed elements.
- Explain what *higher-order functions* are, explain why they're useful, and write higher-order functions.
- Describe the `map` family of functions and their purpose, and rewrite simple `for` loops to use `map` functions.
- Describe the purpose and use of the `safely`, `possibly`, and `quietly` function.
- Describe and use `map2` and `pmap`.
- Describe and use `walk`, `walk2`, and `pwalk`.
- Define *predicate function* and describe higher-order functions that work with predicate functions.
- Define *reduction* and use `reduce` to implement it.
- Define *accumulation* and use `accumulate` to implement it.

### Key Points

- A `for` loop usually has:
  - A variable whose value changes for each iteration of the loop.
  - A set of values being iterated over (such as the indices of a vector).
  - A body that is executed once for each iteration.
  - An output variable where results are stored (whose space is usually preallocated for efficiency).
- Use `vector("type", length)` to generate a vector of the specified type and length (usually to be filled in later).
- `1:length(x)` is non-empty when `x` is empty; `seq_along(x)` is empty when `x` is empty, and so is better to use in loop controls.
- To loop over the columns a tibble:
  - Use `for (variable in seq_along(tibble))` to loop over the numeric indices of the columns.
  - Use `for (variable in names(tibble))` to loop over the names of the columns.
- When the size of a loop's eventual output cannot be known in advance, use a list to collect partial results and then `unlist` or `purrr:flatten_dbl` to combine them into a vector.
  - If values are tables, collect them in a list and use `bind_rows` to combine them all after the loop.
- If the number of required iterations is not known in advance, use a `while` loop instead of a `for` loop.
  - Make sure that the condition of the `while` loop can be changed by the loop body so that the loop does not run forever.
- If none of the elements of a vector have names, `names(vector)` returns `NULL`, so a `for` loop doesn't execute any iterations.
- If some of the elements of a vector have names and some don't, `names(vector)` returns empty strings for unnamed elements.
  - This means that a `for` loop will execute, but that attempts to access unnamed vector elements by name will fail.
- A _higher-order function_ is one that takes other functions as arguments.
  - Higher-order functions allow programmers to write control flow once and re-use it with different operations.
- `map(object, function)` applies `function` to each `object` and returns a list of results.
  - The specialized functions `map_lgl`, `map_int`, etc., operate on and return vectors of specific types (logical, integer, etc.)
  - These functions preserve names and pass extra arguments through to the function provided.
- FIXME: go through "21.5.1 Shortcuts" after learning about formulas.
- `safely(func)` creates a new function that never throws an error, but instead always returns a list of two values:
  - `result` is either the original result (if the original function ran without an error) or `NULL` (if there was an error).
  - `error` is either `NULL` (if the original function ran without an error) or the error object (if there was an error).
  - `map(data, safely(func))` will therefore return a list of pairs.
  - And `transpose(map(data, safely(func)))` will return a pair of lists.
- `possibly(func)` creates a new function that returns a user-supplied default value instead of throwing an error.
- `quietly(func)` works like `safely` but captures printed output, messages, and warnings.
- `map2(vec1, vec2, function)` applies `function` to corresponding elements from `vec1` and `vec2`.
- `pmap(list_of_lists, function)` applies `function` to the values in each of the sub-lists.
  - It is safest to give the sub-lists names that match the names of the function's parameters rather than relying on positional matching.
- The `walk` family of functions execute functions without collecting and returning their results.
- A _predicate function_ is one that returns a single logical value.
- `keep` and `discard` keep elements of the input where a predicate function returns `TRUE` or `FALSE` respectively.
- `some` and `every` determine whether a predicate us true for any or all elements of the input data.
- `detect` returns the first element for which a predicate is true.
- `detect_index` returns the index of the first element for which a predicate is true.
- `head_while` and `tail_while` collect runs of values from the start or end of a structure for which a predicate is true.
- _Reduction_ combines many values using a binary (two-argument) function to create a single resulting value.
  - Use `reduce(data, function)` to do this.
  - `reduce` throws an error of `data` is empty unless an initial value `init` is provided.
- _Accumulation_ performs the same operation as reduction, but keeps the intermediate results (i.e., calculates a running sum).
