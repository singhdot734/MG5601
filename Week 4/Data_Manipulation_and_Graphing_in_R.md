# Introduction

R is a language used to manipulate and analyze data.
By default R uses a command line, like unix, to do everything.
However most people work with R in R studio, which gives a graphical interface and makes things like writing scripts for R much easier.

R studio has 3-4 panels that you can change and resize.
On the top right is the environment panel that will show the objects and tables you have loaded.
On the bottom right is a multi-use panel that can be used to show your working directory, plots you generate, packages installed, and help pages for commands.
On the bottom left is the console, which functions like the unix console we've been using, but with R syntax instead.
The top left is where scripts will be displayed when you have one open. 

## R Scripts

Most of the time when you're working in R you'll be working on an R script. 
Using a script allows you to write multiple lines of code at once and then execute them all at once. 
A script also lets make comments on your code (using the # symbol) to explain what each command is doing. 
R scripts can also be saved and passed on to other people, allowing them to repeat what you've done and replicate your findings.

## R syntax

Because R is a different coding language to unix, commands in R have a different format. 
All commands in R have parentheses after them, and the options go inside the parenthetical. 
The commands are not the exact same either. 
For example, to get the path to your working directory in unix you would use `pwd` while in R you would use `getwd()`.
Within the parenteticals, options are separated by a comma, and imput to the option is indicated by an = sign.

R studio has a number of features that help you type commands in R syntax.
Whenever you type the left parenthesis, it will automatically create the right one.
It also does this with quotation marks.
If you highlight some text and press the left parenthesis or quotation mark, it will put the entire highlightedd section in the parenthetical.
if you are ever unsure what a command does or what arguments it requires you can type ?commandName in the console and the help pane will have a description and example.

R also has tab completion, both for commands and for objects.
When typing the first few letters of a command--as long as the package the command is a part of is loaded--R studio should pop up a small menu with the command.
Scroll to the command with the arrow keys (if it's not already the first option) and press tab, and the full command will be inserted for you. 
It also does this if you are trying to refer to an object stored in the environment.

# Setting up the environment

The first thing you need to do at the begining of any R script is set up both the document and your workspace. 
The start of the script should indclude the general purpose of the script, who wrote it, and when.

1. Start a new R script by clicking on the page icon with an green plus in the toolbar, or by `ctrl+shift+N`
2. At the top create a header with your name, the date, and the class number. Make sure each line of the header starts with a # so that the computer doesn't read it as code. 
3. At the start of every document you should set your working directory. By default it starts with where R is installed, which on OSC will be your home directory. 
Check the default location by using `getwd()`. 
Note that typing the command into the script and hitting enter doesn't run the line, it just moves you to the new line of the script.
To run the line you can highlight the line and press the run button on the top right of the script pane, or you can click on the line and press `ctrl+enter` (or `cmd+enter` on a mac).
You can also run the command in the console pane, it just won't be saved for later. 
5. In the OSC filebrowser make a new directory in your scratch folder called R_tutorial.
Set your R working directory to that new folder with the command `setwd("/fs/scratch/PAS2698/YourDir/R_tutorial")`.
Make sure to change YourDir to your named folder. 
6. Check this worked by using the same command to get the working directory as before. 
7. The next thing you should do is load all of the packages you need in the script.
Packages can be thought of as options for R that give you access to new commands.
It's useful to have a chunk of code at the top of the script with all the packages, so that you don't have to search through the entire script to find the one you want to load. 
Packages are loaded in R using the command `library(packageName)`.
Try loading a package called tidyverse.
8. You most likely got an error, because you don't have tidyverse installed by default.
Packages must be installed, usually from a central repository called CRAN, before they can be loaded and used.
Install tidyverse with the command `install.packages("tidyverse")`.
tidyverse is a suite of packages used to manipulate and manage data tables.
Installing it on a fresh version of R also installs all other packages that it depends on, so it can take a few minutes.
9. After the instillation is done, try loading the tidyverse package again.
10. Any other packages you load in this script, make sure to add them to this setup region
11. Take a screenshot of the header to your script. 
12. You can find a short 2 page cheat sheet for most of the packages in the tidyverse in R studio. 
In the menu at the top, go to help->Cheat Sheets and select the package you're interested in to get the pdf.

# Summarizing data

1. To get practice manipulating data we're going to use the built in iris dataset.
We have to save the dataset as an object so that we can refer to it later in the script.
Saving objects uses the format `objectName = objectContents`.
Save the iris dataset using `iris = iris`.
2. You can look at the dataset using `view(iris)` or by clicking on the name of the object in the environment pane.
You can also look data using `head()` or `tail()` like we have been in unix.
3. To get a better idea of the contents of the data, we want to summarize it.
The easiest way to do this is to use the `summary()` command.
4. What if you want to look at the summary of only the petal length?
Like most data manipulation, this is most easily done using the dplyr package of the tidyverse.
Tidyverse packages allow you to pipe commands into one another like we were doing in unix.
The symbol for pipe in tidyverse is %>% (or |> in newer versions of R studio), inserted using the keyboard shortcut of `ctrl+shift+M` (on both PC and mac).
When you pipe data in tidyverse you always put the input dataframe first, then pipe into subsequent commands. 
Creating a summary in dplyr requires you to use the `summarise()` command, then specifying each summary statistic as a column.
To replicate the Petal.Length summary column from step 4 we would use this code:

```{R petal_summary}
iris_petal_summary = iris %>% summarise(Min = min(Petal.Length), #We pipe the iris dataset into the summarise command, and say that the Min figure should be the minimum of the Petal.Length column of the dataset
                                        Q1 = quantile(Petal.Length,0.25), #to get the first quartile we have to specify it's the 0.25 quantile
                                        Median = median(Petal.Length),
                                        Mean = mean(Petal.Length),
                                        Q3 = quantile(Petal.Length,0.75),#We have to specify quantile 0.75 to get the third quartile
                                        Max = max(Petal.Length)) 
```

Because we piped iris into summarise we only had to specify the column into each summary function.

5. We can also add other information that were not in the base summary command.
Try re-running the code from 4 again, but add in `n = n()` to get the number of values in the data table and `SD = sd(Petal.Length)` to get the standard deviation.
6. What if we want to look at the summary of petal length for each species of iris?
We can pipe the iris dataset first into `group_by(Species)` and then pipe that into the summarise command from above.
Remake the iris_petal_summary dataframe with the species grouping, then take a screenshot of the contents of this table and submit this as in class activity 1. 

# Manipulate data

1. Now that we have an idea of the data we can manipulate it how we want. This is also done with the dplyr package of tidyverse, and so it uses the same pipe formatting as before.
2. Let's calculate the ratio of petal length/width using the mutate command: `petal_ratio = iris %>% mutate(petal_ratio = Petal.Length/Petal.Width) %>% rownames_to_column(var = "ID")`.
Note that the mutate command creates a new column and leaves all the previous columns untouched.
Because we also want each row of the column to have a ID we use the rownames_to_columns command to take the rownumbers to a column named ID.
3. Create a new object with the sepal ratio using the same steps as in 2. 
4. Let's join the two tables together, again using dplyr. 
The left_join command requires two datatables to be joined together.
The pipe will supply one table and you have to supply the other. `iris_ratios = petal_ratio %>% left_join(sepal_ratio)`
5. Of course we could have just made the table with both new columns in one step, and not have to join tables.
Try making a `iris_mutated` table with a petal and sepal ratio column using a single mutate command. 
Take a screenshot of the command you use to make the table. 
6. What if we want only specific columns or rows? In that case we use `select()` to grab full columns and `filter()` to grab only rows that meet logical criteria.
For select you can specify the column number, or the full name of a column in a list. 
With filter you specify a column as well the criteria the values in that column need to meet.
Filter uses the logical an boolean operators of == (equals), != (does not equal), > or < (greater or less than), >= or <= (greate or equal to).
You can combine multiple filters at once with & (and), | (or). 
Make an iris_filtered table by selecting the columns Species, petal_ratio, and sepal_ratio from your iris_mutated table, and then filter to rows with a petal ratio greater than 7.
7. How many observations have a petal ratio greater than 7?
How many observations have a petal ratio greater or equal to 7?

# Graphing Data

Graphing data in R is most easily done using ggplot2.
This package creates a uniform "language of graphics" that make it easy for other package developers integrate with ggplot.
Thus, most graphing packages you'll see use ggplot2 as the base and add extra functionality on top of it. 
We are just going to use the base ggplot2 packages however. 
For help with ggplot2 functions, reference [their website](https://ggplot2.tidyverse.org/reference/).

Graphing in ggplot calls one or many geom functions to act like layers on your graph, and other functions to modify how those layers look.
A geom function is essentially the type of data representation you want to use.
For example, `geom_point()` creates dots for a scatterplot while `geom_boxplot()` creates a box and whisker diagram. 
Geoms have two types of options to specify: aesthetics and statics.
Aesthetics are options that are based on your data, while statics are based on specific values you add. 
For example, an aesthetic would be which data is on the X axis while a static would be how wide the line is. 

1. To start with ggplot we have to create an object for the plot and specify which data table we want the plot to use.
Use `iris_plot = ggplot(data = iris_mutated)` to create this object.
Try typing iris_plot into the console.
What shows up in the plot pane of R studio?
2. The plot is blank because we only specified what the plot should be based on, not what it should look like. 
Let's try making a basic scatter plot of the ratio values we calculated before.
`iris_scatter = iris_plot + geom_point(aes(x = petal_ratio, y = sepal_ratio))`.
note that anything inside `aes()` is an aesthetic based on data.
Type iris_scatter into the console, do you get a plot now?
3. We might have some points overlapping one another, so let's try separating them.
Create a new graph called iris_jitter using the `geom_jitter()` with the same aesthetics as in 2.
What does the fact the plot doesn't change tell you?
4. What if we want to compare the three different species on the plot?
We just have to add a new aesthetic to the geom. 
For a scatter plot this can be done in one of two ways: `shape` to change the shape of the point or the `color` of the point.
Remake the iris_jitter plot, but this time put `shape = Species, color = Species` inside the aes argument.
5. Using two aesthetics for the same variable probably isn't that useful. 
Try putting the color option outside of the `aes()` command i.e. `geom_jitter(aes(x = petal_ratio, y = sepal_ratio, shape = Species), color = Species)`.
The code doesn't run properly because it's looking for an object named Species, not a column in the dataset, because it's outside the aes command.
Try instead replacing Species with "blue" and making the graph.
6. Lets try a different geom and make a bar graph. 
We have two options for this, `geom_bar` and `geom_col`.
geom_bar makes the bars equal to the number of observations in that category, while geom_col makes the bar height equivalent to a specific value. 
Make a new graph called iris_col with the geom_col boxplot, but this time use the iris_petal_summary as the data.
Use x = Species, y = Median, and fill = Species for the aesthetics.
We use fill for bar graphs because we want the interior to be colored, not the outline. 
8. Let's try changing up the looks of the plot beyond the geom_function.
Here is an example of a plot that has been more customized:

```{r iris_example}
iris_summary = ggplot(data = iris_petal_summary)
iris_col = iris_summary + geom_col(aes(x = Species,
                                       y = Median,
                                       fill = Species),
                                   linewidth = 0.5,
                                   color = "black") + #note that multiple functions are combined using a + instead of a comma
  geom_label(aes(x = Species, #geom_label places text labels on the graph
                 y = Median + 0.5, #places the label at the median value + 0.5 so it's not on the bar
                 label = Median, #Puts the value of the column Median at the correct location
                 color = Species), 
             show.legend = FALSE) + #We don't want the legend for this because it will match the one from geom_col
  geom_point(aes(x = Species, #Adds a dot at the Average
                 y = Mean),
             color = "black",
             size = 5, #Change the size of the point
             shape = 18) + #Change the shape of the point
  theme_bw() + #Changes the base theme to a clean black and white
  scale_color_brewer(palette = "Set2", #use one of the built in color palettes
                     aesthetics = c("fill","color")) + #We want the palette used for both fill and color aesthetics
  labs(title = "A customized ggplot", #Set the plot title
       y = "Median Petal Length", #rename the Y axis
       x = "Iris Species",
       fill = "Iris Species") #rename the fill legend
iris_col
```

9. Now try making a plot named final_plot using the iris_mutated dataset and two geoms: `geom_boxplot` and `geom_jitter`.
In the boxplot specify `outlier.shape = NA` as a static option to remove the outliers.
Put species on the X axis and sepal_ratio on the Y axis. 
Color the points grey and fill the boxplots according to species. 
Set the theme to black and white, change the labels of the x and y axis, and give it a title.
10. Now that we have the plots made, we need to save them.
We can use this with the ggplot function ggsave.
Plots can be saved as a pdf or one of many image formats. 
Save the last plot you made (the boxplot and points) using the code below and attach it to your assignment.

``` {r}
ggsave(filename = "YourName_final_plot.png", #the name and file extension you want to save the plot as. Will save in your working directory
       plot = final_plot,
       device = png, #saves it as a PNG
       dpi = 300) #Saves it using a print-quality resolution
```
