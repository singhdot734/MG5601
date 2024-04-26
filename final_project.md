```
# Final project
setwd("/fs/scratch/PAS2698/gsingh/rnaseq/PUM_KO")

library("tidyverse")


Normalized_PUM_KO_counts <- read_csv("Normalized_PUM_KO_counts.csv")



# Compare PUM1 replicate normalized counts
PUM1_counts_plot = ggplot(data = Normalized_PUM_KO_counts)
PUM1_counts_plot = PUM1_counts_plot + geom_point(aes(x = PUM1_R1_norm, #make a scatter plot with the two WT replicates
                                                     y = PUM1_R2_norm)) +
  geom_smooth(aes(x = PUM1_R1_norm, #Add the regression line
                  y = PUM1_R2_norm),
              color = "violet",
              se = FALSE, #Don't include the standard error 
              method = "lm") + #Use a linear regression
  theme_bw() +
  labs(x = "PUM1 Replicate 1 Counts", #rename the X axis
       y = "PUM1 Replicate 2 Counts") #rename the Y axis
PUM1_counts_plot
ggsave("PUM1_count_scatterplot.png", #Save the plot we made as a png
       plot = PUM1_counts_plot,
       device = png(),
       width = 6,
       height = 4,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens

# log10 transform normalized counts
Normalized_PUM_KO_log10 = Normalized_PUM_KO_counts %>% mutate(log10PUM1_R1_norm = log10(PUM1_R1_norm),
                                                              log10PUM1_R2_norm = log10(PUM1_R2_norm))

counts_plot2 = ggplot(data = Normalized_PUM_KO_log10)
PUM1_log10counts_plot = counts_plot2 + geom_point(aes(x = log10PUM1_R1_norm, #make a scatter plot with the two WT replicates
                                                   y = log10PUM1_R2_norm)) +
  geom_smooth(aes(x = log10PUM1_R1_norm, #Add the regression line
                  y = log10PUM1_R2_norm),
              color = "orange",
              se = FALSE, #Don't include the standard error 
              method = "lm") + #Use a linear regression
  theme_bw() +
  labs(x = "PUM1 Replicate 1 log10 Counts", #rename the X axis
       y = "PUM1 Replicate 2 log10 Counts") #rename the Y axis
PUM1_log10counts_plot
ggsave("PUM1_log10count_scatterplot.png", #Save the plot we made as a png
       plot = PUM1_log10counts_plot,
       device = png(),
       width = 6,
       height = 4,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens


#summarize_pum1
PUM_KO_DEseq <- read_csv("combined_PUM_KO_DEseq.csv")
pum1_summary = PUM_KO_DEseq %>% drop_na() %>% summarise(Min = min(PUM1_FC), #We pipe the iris dataset into the summarise command, and say that the Min figure should be the minimum of the Petal.Length column of the dataset
                                                      Q1 = quantile(PUM1_FC,0.25), #to get the first quartile we have to specify it's the 0.25 quantile
                                                      Median = median(PUM1_FC),
                                                      Mean = mean(PUM1_FC),
                                                      Q3 = quantile(PUM1_FC,0.75),#We have to specify quantile 0.75 to get the third quartile
                                                      Max = max(PUM1_FC),
                                                      SD = sd(PUM1_FC),
                                                      n=n())

# Compare log2FC in PUM1 versus PUM2 KO
PUM_KO_DEseq <- read_csv("combined_PUM_KO_DEseq.csv")
PUM1v2_plot = ggplot(data = PUM_KO_DEseq)
PUM1v2_plot = PUM1v2_plot + geom_point(aes(x = PUM1_FC, #make a scatter plot with the PUM1 KO and PUM2 KO1
                                           y = PUM2.K1_FC)) +
  geom_smooth(aes(x = PUM1_FC, #Add the regression line
                  y = PUM2.K1_FC),
              color = "red",
              se = FALSE, #Don't include the standard error 
              method = "lm") + #Use a linear regression
  theme_bw() +
  labs(x = "PUM1 KO fold change", #rename the X axis
       y = "PUM2 KO2 fold change") + #rename the Y axis
  geom_vline(aes(xintercept = 0), #Put a black line on 0 on the x axis
             color = "black") +
  geom_hline(aes(yintercept = 0), #Put a black line on 0 on the y axis
             color = "black") +
  coord_cartesian(xlim = c(-5,5), #Set the graph x axis from -5 to 5
                  ylim = c(-5,5)) #Set the graph y axis from -5 to 5

PUM1v2_plot
ggsave("PUM1v2_plot.png", #Save the plot we made as a png
       plot = PUM1v2_plot,
       device = png(),
       width = 6,
       height = 4,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens

# Compare log2FC in PUM1 versus PUM2 KO only for those genes that change significantly (padj<0.05) in both KOs

PUM_significant = PUM_KO_DEseq%>%filter(PUM1_padj < 0.05 & PUM2.K1_padj < 0.05)
PUM1v2_plot2 = ggplot(data = PUM_significant)
PUM1v2_plot2 = PUM1v2_plot2 + geom_point(aes(x = PUM1_FC, #make a scatter plot with the PUM1 KO and PUM2 KO1
                                             y = PUM2.K1_FC)) +
  geom_smooth(aes(x = PUM1_FC, #Add the regression line
                  y = PUM2.K1_FC),
              color = "blue",
              se = FALSE, #Don't include the standard error 
              method = "lm") + #Use a linear regression
  theme_bw() +
  labs(x = "PUM1 KO fold change", #rename the X axis
       y = "PUM2 KO2 fold change") + #rename the Y axis
  geom_vline(aes(xintercept = 0), #Put a black line on 0 on the x axis
             color = "black") +
  geom_hline(aes(yintercept = 0), #Put a black line on 0 on the y axis
             color = "black") +
  coord_cartesian(xlim = c(-5,5), #Set the graph x axis from -5 to 5
                  ylim = c(-5,5)) #Set the graph y axis from -5 to 5

PUM1v2_plot2
ggsave("PUM1v2_plot2.png", #Save the plot we made as a png
       plot = PUM1v2_plot2,
       device = png(),
       width = 6,
       height = 4,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens

#### Manipulate the dataframe to graph it ####

PUM_all<-read.csv("PUM_KO_DEseq.csv")
PUM_long_new_fc<-PUM_all[,c(1:4,5,7,9)]%>%pivot_longer(cols = c(PUM1_FC,PUM2.K1_FC,PUM2.K2_FC),
                                                       names_to = "Knockout",
                                                       values_to = "FoldChange")

PUM_long_new_padj<-PUM_all[,c(1:4,6,8,10)]%>%pivot_longer(cols = c(PUM1_padj,PUM2.K1_padj,PUM2.K2_padj),
                                                          names_to = "Knockout",
                                                          values_to = "pAdj")

##Binding columns, keeping the order same
PUM_long_new<-bind_cols(PUM_long_new_fc,PUM_long_new_padj[,"pAdj"])

PUM_long_new<-PUM_long_new%>%mutate(Knockout=str_split(Knockout,"\\_",simplify = T)[,1])

names(PUM_long_new)[6]<-"FC"
names(PUM_long_new)[7]<-"padj"
names(PUM_long_new)
write_csv(PUM_long_new, "PUM_DESeq_long.csv")

# boxplots comparing PUM target mRNAs
pum_targets = read_csv("pum_targets.csv")
PUM1_KO_DEseq = PUM_long_new %>% mutate(Target = if_else(external_gene_name %in% pum_targets$Gene, #If the gene name is in the pum_Motif gene column
                                                           "TRUE", #label the gene with TRUE
                                                           "FALSE")) #Otherwise label it with FALSE
boxplot_colors = c("TRUE" = "#D33E43", #Define colors for the boxplots
                   "FALSE" = "#E6C79C")

PUM1_Targets_boxplot = ggplot(data = PUM1_KO_DEseq)
PUM1_Targets_boxplot = PUM1_Targets_boxplot + geom_boxplot(aes(x = Knockout, #Put the two Datasets on the X axis
                                                               y = FC, #Put the log2FC on the y axis
                                                               fill = Target), #Color the boxplot according to if it has a PUM motif
                                                           outlier.shape = 21, #Make the outliers take on the color of the boxplot
                                                           outlier.alpha = 0.5) + #Make the outliers semi-transparent so you can see overlap
  scale_fill_manual(values = boxplot_colors) + #Use the colors we defined previously
  theme_bw() +
  labs(x = "Knockout",
       y = "log2(Fold Change)",
       fill = "PUM targets")
PUM1_Targets_boxplot
ggsave("PUM1_Targets_comparison.png", #Save the plot we made as a png
       plot = PUM1_Targets_boxplot,
       device = png(),
       width = 5,
       height = 5,
       units = "in", #Use inches as our units for width and height
       dpi = 300) #Save it at a good resolution for screens

# For functional annotation
PUM1_upregulated = PUM_long_new %>% filter(Knockout == "PUM1" & FC > 0.32 & padj < 0.05) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
write_csv(PUM1_upregulated,"PUM1_KO_upregulated_genes.csv")

PUM1_upregulated50 = PUM_long_new %>% filter(Knockout == "PUM1" & FC > 0.58 & padj < 0.05) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
write_csv(PUM1_upregulated50,"PUM1_KO_upregulated50_genes.csv")

PUM1_downregulated = PUM_long_new %>% filter(Knockout == "PUM1" & FC < -0.32 & padj < 0.05) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
write_csv(PUM1_downregulated,"PUM1_KO_downregulated_genes.csv")

PUM2_1_upregulated = PUM_long_new %>% filter(Knockout == "PUM2.K1" & FC > 0.32 & padj < 0.05) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
write_csv(PUM2_1_upregulated,"PUM2_1_KO_upregulated_genes.csv")
```
