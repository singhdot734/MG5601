# Activity 1 - Finish making a BED file with start and end coordinates of protein coding genes on chr22
1. We will first complete making the custom BED file that contains start and end coordinates of all protein coding genes on human chr22.
2. To do this, we will get a new gff3 file from GENCODE genes (a joint initiative of NIH and EMBL for gene annotations).
3. Either you should already have made or make a directory in your scratch space called `hg38`.
4. In this directory, download the gff3 file: `curl https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_45/gencode.v45.basic.annotation.gff3.gz > gencode45.gff3.gz`
5. Extract the file `gunzip gencode45.gff3.gz`.
6. To view the columns of the gff3 file, try `grep "chr22" gencode45.gff3 | grep "TBX1" | head -1`.
7. Let's compare the gff3 file with the gtf file we were working with last week, you can try this `grep "chr22" hg38.ncbiRefSeq.gtf | grep "TBX1" | head -1`.
8. Compare column 9 of each file and try to note the differences. It is the spaces in column 9 of the GTF file that were proving to be tough nut to crack.
9. First we will match word "gene_type" in column 9 and output this feature in one of the columns in an intermediate BED file, as follows:
10. `grep "chr22" gencode45.gff3 | awk 'OFS="\t" {match($9,/gene_type=(\w+);/,a); print $1,$4,$5,a[1],$3,$6,$7,$9}' > chr22gene_type.bed`
11. View the top of the file: `head -3 chr22gene_type.bed`
12. Now, we will make another intermediate BED file of only "protein_coding" genes: `cat chr22gene_type.bed | awk 'OFS="\t" {if ($4=="protein_coding") {print $1,$2,$3,$5,$6,$7,$8}}' > chr22proteincoding.bed`
13. We are still keep the last column from gff3 file as we will need to extract "gene_name" from that column.
14. View the top of the file: `head -3 chr22proteincoding.bed`
15. Next, we will use awk regular expression to find "gene_name" word match and extract gene names into a new column: `cat chr22proteincoding.bed | awk 'OFS="\t" {match($7,/gene_name=(\w+);/,a); print $1,$2,$3,a[1],$4,$6}' > chr22pc_gene_name.bed`
16. This BED file will have separate lines for features for each gene such as gene, transcript, exon. To get just the lines with "gene" as feature: `grep "gene" chr22pc_gene_name.bed > chr22pc_genes.bed`
17. Look at the top of our final BED file: `head -3 chr22pc_genes.bed`
18. Can you make a similar BED file for another human chromosome? Take a screenshot of all your code from this last step and the top 10 lines of this new BED file to submit as classwork.

# Activity 2 - Intro to R/R Studio
1. Make a vector called `my_numbers <- c(1,2,3,41,5,6,25,33)
2. Make another vector. Called it your_numbers (put 8 random numbers between 1-100).
3. Make a plot(my_numbers, your_numbers)
4. Download the plot as PNG file and submit as classwork.

