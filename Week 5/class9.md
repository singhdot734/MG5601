# Activity 1 - Install necessary R packages
1. We will need some new R packages to do microarray analysis (or to run tools needed for this analysis).
2. An important general note: Today we will comment on all the code we type in R Studio so that we can track what we did.
3. Open R Studio and in the script/input window, run the following commands one by one:
```
# install necessary packages

install.packages("BiocManager")
install.packages("devtools") # needed to install plogr
devtools::install_github("krlmlr/plogr") # affy package depends on this
BiocManager::install("affy")
BiocManager::install("GEOquery")
```
4. These will take some time to install and we will discuss our overall plan in the meantime.
5. Once the packages are installed, add pound (#) sign to comment them out so that these commands are not run repeatedly. Like this:
```
# install necessary packages

#install.packages("BiocManager")
#install.packages("devtools") # needed to install plogr
#devtools::install_github("krlmlr/plogr") # affy package depends on this
#BiocManager::install("affy")
#BiocManager::install("GEOquery")
```
# Activity 2 - Get microarray data from GEO and normalize it
1. Make a new directory: R-tutorial/microarray and set it as working directory.
 ```
# don't forget to add a comment here 
setwd("/fs/scratch/PAS2698/cmembree/R_tutorial/microarray")
```
2. Load the following libraries: affy, GEOquery, tidyverse, ggplot. The command is: `library()`. The name of the library goes in the parenthesis. Don't forget to add comments.
3. Get data from GEO (supplementary files): `getGEOSuppFiles("GSE26626")`
4. Make a new directory: R-tutorial/microarray/data
5. Uncompress the tar files: `untar("GSE26626/GSE26626_RAW.tar", exdir = "data/")`. Are you adding comments for each line of code?
6. Check your data folder. It should contain four .CEL files. These files have the are raw image data from microarray. Each file has data from each IP (two control IPs and two PUM2 IPs).
7. Next we will read in the .CEL files: `raw.data <- ReadAffy(celfile.path = "data/")`. `raw.data` is the object that contains the raw data. It should appear in the top right environment panel.
8. Next we will perform data normalization using RMA method: `normalized.data <- rma(raw.data)`. rma is the function that uses "raw.data" object to create "normalized.data" object.
9. We will next use "exprs" function to get expression estimates: `normalized.expr <- exprs(normalized.data)`. All these objects should start accumulating in your environment pane.
10. "normalized.expr" is a matrix and we will convert it to a data frame: `normalized.expr1 <- as.data.frame(exprs(normalized.data))`. Compare "normalized.expr" and "normalized.expr1". What difference do you see?
11. Did you add comments to each step? Take a screenshot of all your steps from this activity and of "normalized.expr1" and submit as classwork.

# Activity 3 - adding gene names, tidying data and getting a quick look at data
1. The "normalized.expr1" we get has probe IDs in each row. We need to match these probe IDs to gene IDs.
2. To map probe IDs to gene symbols, we will get the matrix for this particular array, which is available in GEO and has probe IDs and gene names.
3. Run this command: `gse = getGEO("GSE26626", GSEMatrix = TRUE)`
4. From this matrix, we will fetch feature data to map probe ID to gene symbol: `feature.data = gse$GSE26626_series_matrix.txt.gz@featureData@data`.
5. Look at the object "feature.data". What do you see in column 1 and 11? Probe IDs and gene names!
6. We need only these two columns. So we will subset them as follows: `feature.data1 = feature.data[,c(1,11)]`
7. (Are you commenting each line of your code?)
8. Now we will tidy our data little bit and add gene names by matching probe IDs in "normalized.expr1" and "feature.data1".
```
normalized.expr2 = normalized.expr1 %>% 
  rownames_to_column(var = 'ID') %>% # adding a new variable called ID from rownames
  inner_join(.,feature.data1, by = 'ID') %>% # join normalized.expr1 and feature.data1 by variable 'ID'
  rename(Gene = `Gene Symbol`, # Change the Gene Symbol column name to just Gene
         PUM2_r1 = GSM655464.CEL.gz, #Change the name of the column to sample description
         CTR_r1 = GSM655465.CEL.gz, #Change the name of the column to sample description
         PUM2_r2 = GSM655466.CEL.gz, #Change the name of the column to sample description
         CTR_r2 = GSM655467.CEL.gz) #Change the name of the column to sample description
```
9. Next we can calculate fold change in PUM2 IP versus control IP for each gene (probe). Normalized data is in log2 transformed state. So, fold change = PUM2_r1 - CTR_r1 (and not PUM2_r1/CTR_r1).
```
   ### Calculate Log2 Fold Change ####
Fold_change = normalized.expr2 %>% mutate(R1_FC = PUM2_r1 - CTR_r1, #Calculate Log2 fold change from replicate 1
                                          R2_FC = PUM2_r2 - CTR_r2) #Calculate Log2 fold change from replicate 2
```
10. Now we can use `filter` function from tidyverse to look at some trends:
```
#Filter to only the genes with a Log2FC greater than 2 in replicate 1
Rep1_log2 = Fold_change %>% filter(R1_FC > 2)

#Filter to only the genes with a Log2FC greater than 2 in both replicates
Both_log2 = Fold_change %>% filter(R2_FC > 2 & R1_FC > 2)

#Pick the genes with the 50 largest log2FC in replicate 2
top_r1 = Fold_change %>% slice_max(R1_FC, n = 50)
```
11. Modify code above to answer the following questions. Also, submit your code as part of the answers.
 - Which are top-10 genes with the largest log2FC in replicate 1? Screenshot of your output object is fine.
 - How many genes have fold change greater than 1.5 in replicate 2?

# Activity 4 - compare the two replicates
1. We can see how well the two replicates agree with each other by plotting fold change for each gene (probe ID) on a scatter plot.
2. We will use `geom_point` function of ggplot.
```
#### Plot log2 Fold Change ####
FC_scatter = ggplot(data = Fold_change)
FC_scatter = FC_scatter + geom_point(aes(x = R1_FC,
                                         y = R2_FC,
                                         color = ID %in% top_r1$ID)) + #color point based on if they're in the top 50 Log2FC of replicate 1
  theme_bw() +
  labs(x = "Replicate 1 Log2FC",
       y = "Replicate 2 Log2FC",
       color = "Top 50 FC in R1") +
  scale_color_manual(values = c("TRUE" = "navy",
                                "FALSE" = "grey"))
FC_scatter
```
3. So, do the two replicates agree? Briefly summarize your thoughts on the two replicates in the answer to activity 4 question in today's assignment. Include your scatter plot in the answer.
4. We can next try to test which of the two replicates is better, at least qualitatively.
5. We can obtain a list of mRNAs that have PUM binding site (PUM motif) and those that do not. Will mRNAs with PUM binding sites show higher or lower fold change in PUM2 IP versus control?
6. List of genes and their PUM motif status is in file called `pum_Motif.csv`. Here is the path to the file. Copy it into your working directory: `/fs/scratch/PAS2698/class_data/pum_Motif.csv`
```
#### Look at genes with and without PUM binding sites ####
pum_Motif <- read_csv("pum_Motif.csv")
motif_FC = Fold_change %>% left_join(pum_Motif,
                                     by = c("Gene")) %>% #Add the motif annotation to our fold change table
  filter(!is.na(Motif)) #Remove any that were not listed on the motif list
motif_FC = motif_FC %>% pivot_longer(cols = c(R1_FC, #Combine the two fold change column's into one
                                              R2_FC),
                                     names_to = "replicate",
                                     values_to = "fold_change") %>% 
  mutate(replicate = str_remove(replicate,"_.*")) #Remove the _FC from the replicate column name

#Plot the boxplots
motif_plot = ggplot(data = motif_FC)
motif_plot = motif_plot + geom_boxplot(aes(x = replicate, #Put each replicate on the X axis
                                           y = fold_change, #Put the fold change on the Y axis
                                           fill = Motif), #Fill the boxplots based on if there is a PUM motif or not
                                       outlier.shape = 21, #Change the outlier dots to ones that take on fill color
                                       outlier.alpha = 0.5) + #make the outliers semi-transparent
ylim (-2.5,2.5) + # set y-axis limits to between -2.5 and 2.5 fold change
  theme_bw() +
  labs(x = "Replicate",
       y = "Log2 Fold Change",
       fill = "Contains PUM motif") +
  scale_fill_manual(values = c("TRUE" = "#99AA4B", #Set the color of the True boxplot
                               "FALSE" = "#2D5D7B")) #Set the color of the False boxplot
motif_plot
```
6. Based on the result, which of the two replicates do you think is more reliable experiment? Why? Include your answer in the class assignment along with your boxplot.

# Activity 5 - summarizing trends for a select set of genes using heatmap
1. We can also plot the fold changes as heatmap for all or select set of genes to compare signal intensities in PUM2 versus control IP in the two replicates.
2. Here is the code to first manipulate the data frame using `pivot_longer` function of tidyverse and select specific genes.
```
#### Manipulate the dataframe to select a specific set of genes ####
Norm_expr_long = normalized.expr2 %>% 
  pivot_longer(cols = 2:5, #Take the data from columns 2:5
               names_to = "Sample", #Put the column names in a new column called Sample
               values_to = "Expr") #Put the values in a new column called Expr

genes_of_interest = c("MYCBP2","PUM2","PUM1","DHX32","SF3B1","PLRG1") #Genes with a known PUM binding site
Norm_GOI = Norm_expr_long %>% filter(Gene %in% genes_of_interest) #Filter to rows with genes in our genes of interest list
```
3. Next we will plot a heatmap using `geom_tile` function of ggplot:
```
sample_order = c("PUM2_r1","CTR_r1","PUM2_r2","CTR_r2") #The order we want the samples to be listed in 
small_heatmap = ggplot(data = Norm_GOI) #Use the smaller dataset
small_heatmap = small_heatmap + geom_tile(aes(x = factor(Sample, #Put the different samples on the X axis 
                                                         levels = sample_order), #Put the levels in the order we want instead of alphabetically
                                              y = ID, #Put the genes on a Y axis
                                              fill = Expr)) + #Fill in the colors based on the expression values
  scale_fill_continuous(low = "seagreen", #Set the low color on the heatmap to green
                        high = "midnightblue") #set the high color on the heatmap to blue
small_heatmap
```
4. Pick any 5 genes that contain PUM motif (motif=TRUE in pum_Motif.csv). Call this object as "my_Norm_GOI". Take a screenshot of this object and make a heatmap of these genes from only replicate 1 to submit as assignment. 


