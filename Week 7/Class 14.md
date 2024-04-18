# Activity
1. Open the R script you made in the last class. Run the line where you set your working directory and the line where you imported PUM2_KO_DEseq_long.
2. Use the following code to make a new data table with genes that are 2 fold or more upregulated in the PUM2 KOs
```
upregulated = PUM2_KO_DEseq_long %>% filter(FC > 1 & padj < 0.05) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
```
3. Make a new table called downregulated by modifying the code in step 2 to filter to genes with a Fold change of -2 or less.
4. **Assignment**: How many genes are upregulated and how many genes are downregulated? What do we mean by up or downregulation?
5. Save the two datatables as CSV files using the following code:
```
write_csv(upregulated,"PUM2_KO_upregulated_genes.csv")
write_csv(downregulated,"PUM2_KO_downregualted_genes.csv")
```
6. Upload the CSV files into DAVID (https://david.ncifcrf.gov/) and conduct gene ontology analysis for the set of genes that are up and downregulated. Guidance for functional analysis is available in class 10 notes. Focus on Biological Process and Molecular Function GO terms.
7. Note: Fold change of 2 or more (or 2 or less) is an arbitrary threshold. Sometimes we need to alter these thresholds to find a slice of the data that can be informative. Even genes changing in expression by 50% or more (i.e., 1.5 fold change) can be meaningful. Try changing FC in step 2 to FC > 0.58 (log2 of 1.5 is 0.584). For assignment, use FC cut-off that gives you more interpretable results.
8. **Assignment**: Paste the screenshot of the DAVID clusters for the up and downregulated genesets into your assignment. Include up to five clusters that have Benjamini corrected p-value less than 0.05. Briefly discuss what these clusters tell you about the function of PUM2.  
9. Analyze the up and downregulated genes using the PANTHER functional annotation tool (https://www.pantherdb.org/).
10. **Assignment**: Paste the screenshot of the PANTHER terms for the up and downregulated genesets into your assignment. Briefly compare your results from PANTHER and DAVID, and discuss your overall thoughts about the function of PUM2.

