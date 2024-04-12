# Activity 1
1. Open the R script you made last class. Run the line where you set your working directory and the line where you imported PUM2_KO_DEseq_long.
2. Use the following code to make a new datatable with genes that are 2 fold upregulated in the PUM2 KOs
```
upregulated = PUM2_KO_DEseq_long %>% filter(FC > 1) %>%  #Because the values in FC are log2(Fold Change) 1 = Fold Change of 2
  select(external_gene_name) %>% #pick only the column we want
  distinct(external_gene_name) #Get rid of any duplicates
```
3. **Discussion**: What do we mean by up or downregulation? Why do we use log2(Fold Change)?
4. Make a new table called downregulated by modifying the code in step to filter to genes with a Fold change of -2.
5. **Assignment**: How many genes are upregulated and how many genes are downregulated?
6. Save the two datatables as CSV files using the following code:
```
write_csv(upregulated,"PUM2_KO_upregulated_genes.csv")
write_csv(downregulated,"PUM2_KO_downregualted_genes.csv")
```
7. Upload the CSV files into DAVID and conduct gene ontologoy analysis for the set of genes that are up and downregulated.
8. **Assignment**: Paste the screenshot of the DAVID clusters for the up and downregulated genesets into your assignment. Briefly discuss what these clusters tell you about the function of PUM2.

# Activity 2
1. In your PUM_KO
