# Activity 1
1. In your scratch folder make a new folder called PUM_KO.
2. Create a new R script and load the tidyverse. Make sure you are adding comments throughout the script you will write today.
3. Set your R working directory to the PUM_KO folder you made
4. Using the terminal copy the file Normalized_PUM_KO_counts.csv from the folder /fs/scratch/PAS2698/PUM1_2_KO to your working directory.
5. Load this data into R with `Normalized_PUM_KO_counts <- read_csv("Normalized_PUM_KO_counts.csv")`.
6. **Discussion**: How were the counts normalized? What does it mean that some genes have a 0 in the counts column?
7. Create an object named counts_plot specifying the data to use in graphs. Hint: use `ggplot(data = Normalized_PUM_KO_counts)`
8. Use the following code to make a plot comparing the normalized counts in both WT replicates:
```
WT_counts_plot = counts_plot + geom_point(aes(x = WT1_norm, #make a scatter plot with the two WT replicates
                                              y = WT2_norm)) +
  geom_smooth(aes(x = WT1_norm, #Add the regression line
                  y = WT2_norm),
              color = "blue",
              se = FALSE, #Don't include the standard error 
              method = "lm") + #Use a linear regression
  theme_bw() +
  labs(x = "WT Replicate 1 Counts", #rename the X axis
       y = "WT Replicate 2 Counts") #rename the Y axis
WT_counts_plot
ggsave("WT_count_scatterplot.png", #Save the plot we made as a png
       plot = WT_counts_plot,
       device = png(),
       width = 5,
       height = 5,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens
```
9. Modify the code from step 8 to make a ggplot object named PUM2_KO1_counts_plot, where PUM2_K1_R1_norm is on the X axis and PUM2_K1_R2_norm is on the y axis. Change the color of the regression line to `"green"` and update the labels on axies.
10. Modify the code from step 8 to make a ggplot object named PUM2_KO2_counts_plot, where PUM2_K2_R1_norm is on the X axis and PUM2_K2_R2_norm is on the y axis. Change the color of the regression line to `"orchid"` and update the labels on axies.
11. **Assignment**: Paste the code from steps 8-10 in the assignment along with the graph generated from each. What can you conclude about the replicates froms the graphs?

# Activity 2
1. Using the terminal copy the file combined_PUM_KO_DEseq.csv from the /fs/scratch/PAS2698/PUM1_2_KO to your working directory.
2. Read in the DEseq table and save it as PUM_KO_DEseq
3. Create a new object called PUM2_significant containing only the genes with a significant log2FoldChange by piping PUM_KO_DEseq into `filter(PUM2.K1_padj < 0.05 | PUM2.K2_padj < 0.05)`
4. Modify the code from step 8 in activity 1 to make a plot compairing PUM2.K1_FC on the X axis and PUM2.K2_FC on the Y axis. Inside the `aes()` of geom_point include `color = external_gene_name %in% PUM2_significant$external_gene_name` to color points based on if they're in the significant list or not. Add the following code after `labs` (don't forget to put a + after the closing bracket of labs):
```
geom_vline(aes(xintercept = 0), #Put a black line on 0 on the x axis
           color = "black") +
geom_hline(aes(yintercept = 0), #Put a black line on 0 on the y axis
           color = "black") +
scale_color_manual(values = c("TRUE" = "seagreen", #Sets the color of the significant genes to green
                              "FALSE" = "grey")) +
coord_cartesian(xlim = c(-5,5), #Set the graph x axis from -5 to 5
                ylim = c(-5,5)) #Set the graph y axis from -5 to 5
```
5. *Optional*: Try putting `geom_vline()` and `geom_hline()` before `geom_point()`. What changed? This demonstrates that geometries are put on top of each other in the order you list them. This can be useful so that you don't cover data up with background elements (like the black lines we added).
6. **Assignment**: Include the plot you generated in your assignment. What does the plot tell you about the similarity in fold change of each PUM2 KO? Are the points with significant fold changes where you would expect?

# Activity 3
1. Using the terminal copy the file PUM2_KO_DEseq_long.csv from the /fs/scratch/PAS2698/PUM1_2_KO to your working directory.
2. Load this data into R as PUM2_KO_DEseq_long. This is the same data as combined_PUM_KO_DEseq, but formatted differently to make the next plots easier. What are the differences between the two tables?
3. Using the terminal copy the file pum_Motif.csv from the /fs/scratch/PAS2698/PUM1_2_KO to your working directory and import it into R as pum_Motif.
4. To compare genes with the PUM motif to ones without, we have to add a new column to the table saying if each gene has a PUM motif or not. In the past we did this by joining the two tables together, however, that means we would have to make a comprehensive list of all genes with and without a PUM motif, otherwise some genes will be unanotated. Instead we will use an if/else statement to label the genes. Run the code below, which will label any genes on the pum_Motif as TRUE and any genes not on the list as FALSE.
```
PUM2_KO_DEseq_long = PUM2_KO_DEseq_long %>% mutate(Motif = if_else(external_gene_name %in% pum_Motif$Gene, #If the gene name is in the pum_Motif gene column
                                                                   "TRUE", #label the gene with TRUE
                                                                   "FALSE")) #Otherwise label it with FALSE
```
5. Use the following code to generate a boxplot comparing the two PUM2 knockouts and the effect they have on genes with a PUM motif.
```
boxplot_colors = c("TRUE" = "#D33E43", #Define colors for the boxplots
                   "FALSE" = "#E6C79C")

Motif_boxplot = ggplot(data = PUM2_KO_DEseq_long)
Motif_boxplot = Motif_boxplot + geom_boxplot(aes(x = Knockout, #Put the two Datasets on the X axis
                                                 y = FC, #Put the log2FC on the y axis
                                                 fill = Motif), #Color the boxplot according to if it has a PUM motif
                                             outlier.shape = 21, #Make the outliers take on the color of the boxplot
                                             outlier.alpha = 0.5) + #Make the outliers semi-transparent so you can see overlap
  scale_fill_manual(values = boxplot_colors) + #Use the colors we defined previously
  theme_bw() +
  labs(x = "Knockout",
       y = "log2(Fold Change)",
        fill = "Contains PUM Motif")
Motif_boxplot
ggsave("PUM2_motif_comparison.png", #Save the plot we made as a png
       plot = Motif_boxplot,
       device = png(),
       width = 5,
       height = 5,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens
```
6. Using the terminal copy the file pum_targets.csv from the /fs/scratch/PAS2698/PUM1_2_KO to your working directory and import it into R as pum_targets.
7. Modify the code from step 4 to create a new column named Target, based on the gene name's presence in `pum_targets$Gene`.
8. **Assignment**: Post a screenshot of your PUM2_KO_DEseq_long table showing both the Motif and Target columns.
9. Modify the code from step 5 to make a boxplot comparing the genes that are a Pum target or not. Make sure to change which column is used to fill the boxplots, and the label of the fill legend.
10. **Assignment**: Paste the two boxplots you made into your assignment. Do the two PUM2 KDs have the same effect on genes with a PUM motif or are a PUM target? What do these boxplots tell you about the effect of PUM2 on it's targets?
