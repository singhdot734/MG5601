# Activity 1
1. Use the `pum_targets.csv` list from `/fs/scratch/PAS2698/class_data/pum_targets.csv` to do functional association analysis using DAVID (https://david.ncifcrf.gov/).
2. For step 2, select "OFFICIAL_GENE_SYMBOL", step 2a will be "Homo sapiens" and in step 3, select "Gene list". You can also upload the csv or text file in step 1.
3. Click "Submit" and on the next page, click on Step 2 "" on the main pane on right.
4. On the next page you will have options, click on the little plus sign next to "Functional_Annotations" category (one of the nine options in red font) and select only the top three "UP_KW_BIOLOGICAL_PROCESS", "UP_KW_MOLECULAR_FUNCTION" and "UP_KW_BIOLOGICAL_PROCESS".
5. Unclick all options in the remaining 8 categories.
6. You can view "Functional Annotation clustering", "Functional Annotation chart" and "Functional Annotation table". There will be a question to answer in today's assignment based on these results.

# Activity 2
1. Now, use the same `pum_targets.csv` list to do a similar analysis in PANTHER: https://www.pantherdb.org/
2. First do the "Functional classification viewed in graphic charts" option.
3. Then do the "Statistical overrepresentation test". Try different annotations.
4. There will be a question to answer in today's assignment based on "GO - Biological Process complete" annotation.

# Activity 3
1. Perform GO term analysis of the genes that are more than 2-fold enriched in PUM2 IP over control IP (replicate 1) from microarray analysis we did in the previous class.
2. Here is the code to get a csv list of this set of genes. This code can be added after step 10 of activity 3 from the previous class to get the list.
```
Rep1_log2 = Fold_change %>% filter(R1_FC > 2)
Rep1_genes = Rep1_log2 %>% select(Gene) %>% #Pull only the gene column
distinct(Gene) %>%  #Remove duplicate gene names
filter(!str_detect(Gene, "///")) #Remove genes with multiple possible names
write_csv(Rep1_genes,"Rep1_FC2_genes.csv") #Save the previous table as a CSV file
```
3. There is a question to answer from this activity in the assignment.
