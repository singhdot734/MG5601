# Activity 1 - Mapping fastq files to genome with HISAT2
1. We will use a software called HISAT2 for mapping reads to human genome. HISAT2 is among many tools and packages for genomics that are already available on OSC.
2. To check what is available on OSC, log in to ondemand, go to your personal directory in scratch space and type command `module avail` and enter.
3. You will see a list of software available. Use space bar to move down the list. Find HISAT2 in the list.
4. Make a new folder named HISAT in your personal directory.
5. Load hisat2 using `module load hisat2`.
6. To map reads to the genome HISAT2 first requires a genome index to be made, which we have already done and made available in `/fs/scratch/PAS2698/HISAT_index/`.
7. For one pair of the fastq files for one run id (e.g., SRR12339771_1.fastq and SRR12339771_2.fastq) use the following command to map paired-end reads in these files and save the output to the HISAT folder you just made.
Note that you have to specify the path to the pair of fastq files, which we stored in the fastQC folder in the previous class. Because these are paired reads you have to specify both mates within a pair (each file ending in _1 and _2). Also, replace the entire `<RUNID>` with the accession number of your file. 

```
hisat2 -x /fs/scratch/PAS2698/HISAT_index/genome_tran -1 fastQC/<RUNID>_1.fastq -2 fastQC/<RUNID>_2.fastq -S HISAT/<RUNID>_hisat.sam --summary-file HISAT/<RUNID>_sum.txt
```
8. You will see a summary of the alignment output on your terminal screem. The summary file (<RUNID>_sum.txt) is the same output as what HISAT outputs onto the terminal.
9. Assignment: Take a screenshot of the command from step 7 and the output and paste it into your assignment.
10. Repeat step 7 with the fastq files of the other accession number you downloaded.
11. Assignment: Take a screenshot of your HISAT folder showing the .sam and .txt file for each run. Which run has more reads mapping to the genome? What percentage of pairs mapped discordantly in each of the two samples?
12. Discussion: we will discuss sam file format. Run `more <RUNID>_hisat.sam` and follow along the discussion.

# Activity 2 - Quantification of gene expression in alignment files using featureCounts
1. To quantify changes in gene expression, we will need to find out how many reads map to each gene in each sample before we can compare conditions A and B to see which genes are going up or down in the experiment. Before we do this, we will need to do steps.
2. First step is to sort the sam file and convert it into its binary format, bam file.
3. samtools is available as a module in OSU. Load it using `module load samtools`
4. Move into your HISAT directory where .sam files are from the previous step. Run the following command: `samtools sort <RUNID>_hisat.sam -o <RUNID>_hisat.bam`
5. Make sure to replace <RUNID> with the accession number.
6. Repeat it for the second run id by replacing <RUNID> its accession number.
7. Try `head <RUNID>_hisat.bam`. How does this compare to sam file? Can you understand this?
8. Second step is to get a gtf file. We already got one for NCBI RefSeq genes in an earlier class when we were trying to make a BED file. We will get a different GTF file that has ENSEMBL gene IDs (this is just a choice that aligns with other decisions we made for next steps in the next class).
9. Go to hg38 directory in your personal directory in scratch space. Once there, download the GTF file from ENSEMBL with this command: `wget https://ftp.ensembl.org/pub/release-111/gtf/homo_sapiens/Homo_sapiens.GRCh38.111.gtf.gz`
10. Unzip the GTF file with `gunzip`.
11. Go back out into your personal directory. Make a new directory called quant where output from next step will be stored.
12. Mapped reads in bam (sam) files that fall within annotated sections of the genome can be counted by many tools. The one that we will used is called featureCounts, which is part of the bioinfo conda environment we set up in an earlier class. So activate this environment by issuing command `conda activate bioinfo`
13. We are now ready to quantify reads that fall within genes. Run featureCounts as follows:
```
featureCounts -p -a hg38/Homo_sapiens.GRCh38.111.gtf -o quant/<RUNID>_featurecounts.txt HISAT/<RUNID>_hisat.bam
```
-p means paired-end data
-a refers to annotation file
-o specifies where output will go (change <RUNID> to your accession numbers)
Input is at the end

15. Go into the quant directory and look at the output of featureCounts using `more <RUNID>_featurecounts.txt`. Note that first line has the summary of the program and the command you used to generate this file. Next line has column headers. Counts for each gene are in the last column.
16. We can cut out the first and last column for easy viewing as follows (this should sound very familiar now): `cat <RUNID>_featurecounts.txt | cut -f1,7 | head`
17. Multiple bam files can be fed into featureCounts. In our case, as both bam files are in the same folder HISAT, we can run featureCounts on both using the wildcard (*). Remember to go back to your personal directory as file paths in the code below are relative to that directory: `featureCounts -p -a hg38/Homo_sapiens.GRCh38.111.gtf -o quant/twoBams_featurecounts.txt HISAT/*_hisat.bam`
18. Go into the quant folder and take a look at the twoBams_featurecounts.txt.summary file.
19. Assignment: take a screenhot of your command from step 17 and submit. Also submit a screenshot of the contents of the twoBams_featurecounts.txt.summary file (use `more`).
20. We can cut the columns 1 (gene IDs), 7 (gene read counts in <RUNID1>) and 8 (gene read counts in sample run2) and sort them by column 2 (-k option) in reverse order (descending) by number (-rn option) as follows: `cat twoBams_featurecounts.txt | cut -f1,7,8 | sort -k2 -rn | head`
21. Assignment: Save the output of the previous command in a new text file. Call it twoBamsSorted.txt. Take a screenshot of the contents of your current directory as well as of the first 30 lines of the twoBamsSorted.txt file to submit. Which are the top three genes with most signal? Copy their ENSG IDs and look up their gene names and function. Summarize this information in your answer.
