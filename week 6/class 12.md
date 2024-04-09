# HISAT2 mapping
1. HISAT2 is already installed on the supercomputer, so all we need to do is load it.
Do this with `module load hisat2`
2. Move to the fastQC folder where you downloaded the fastq files from the PUM knockout in Tuesday's class.
3. make a new folder named HISAT
4. To map reads to the genome HISAT2 first requires an index to be made, which we have already done in `/fs/scratch/PAS2698/HISAT_index/`.
For one of the files use the following command to map the file and save the output to the HISAT folder you just made.
Because these are paired reads you have to specify both mates (each file ending in _1 or _2) by replacing the entire `<RUNID>` with the accession number.

```
hisat2 -x /fs/scratch/PAS2698/HISAT_index/genome_tran -1 <RUNID>_1.fastq -2 <RUNID>_2.fastq -S HISAT/<RUNID>_hisat.sam --summary-file HISAT/<RUNID>_sum.txt
```
5. The summary file is the same output as HISAT outputs into the terminal.
Take a screenshot of the command from step 4 and the output and paste it into your assignment.
6. Repeat step 4 with the other accession number you downloaded.
7. Take a screenshot of your HISAT folder showing the .sam and .txt file for each run.
8. Which run has more reads mapping to the genome?  
