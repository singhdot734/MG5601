# Getting human chromosome 22 sequence
1. Make a new directory in your personal directory: `mkdir chr22`
2. Go to this new directory `cd chr22`
3. Get human chr22 fasta file using `curl https://hgdownload.soe.ucsc.edu/goldenPath/hg38/chromosomes/chr22.fa.gz > chr22.fa.gz`

# Activity 1: Data decompression and compression
1. Three ways to decompress the `chr22.fa.gz` file (use any one):

```
      gzip -d chr22.fa.gz
      gunzip chr22.fa.gz
      gunzip -c chr22.fa.gz > chr22.fa
```

2. Let's try making new files out of `chr22.fa`, move them into a directory of their own and compress this directory using tar
3. Write out first 10 lines of the `chr22.fa` into a new file called `chr22.head.fa`. There are many ways to do this (try `cat chr22.fa | head -10 > chr22.head.fa`).
4. Now let's write the last 10 lines of the `chr22.fa` file into another file and call this `chr22.tail.fa`
5. Make a new directory. Let's call it `headtail`
6. Move `chr22.head.fa` and `chr22.tail.fa` to this directory: `mv chr22.*.fa headtail/`
7. Now archive this directory: `tar czfv chr22headtail.tar.gz headtail/*`
8. Take a screen shot of steps 1-8 to submit as class activity 1.


# Activity 2 - Finding patterns in chr22
1. Check the sequence at the start and end of chromosome: `head` or `tail` for a defined set of lines; try `more` or `less` and use space to scroll through, ctrl+c to terminate.
2. How many nucleotides are in chr22? Per Genome Reference Consortium, it is **50,818,468**
3. Try `wc chr22.fa`. It gives line, word and character counts, respectively. The character count can give us chr length but it is a bigger number than expected. Why?
4. Use `grep -v ">"` to search the file, `pipe` output to `wc -c`
5. Still a bigger number! Why? We will get to the bottom of it.
6. Next we will look for restriction enzyme sites in chr22 sequence.
7. Let's look for EcoRI site "GAATTC". Try `grep --color=ALWAYS "GAATTC" chr22.fa | head`
8. You will see only those lines which have a GAATTC sequence (once or more than once).
9. Pipe out the `grep` output to `wc -l`. This is the number of lines with GAATTC sequence. Is this the final answer?
10. There could be some lines with more than one EcoRI site. To find which are these lines, try adding following flags to the `grep` command above: `-o` (output only the string that is being searched); `-n` (print line numbers where string is found).
11. Now pipe output from `grep` into `sort` then into `uniq -c` (report only unique lines with count), and then into `sort -rn` (sort in reverse order by number).
12. How many lines have more than one EcoRI site?
13. So, how many EcoRI sites are there in chr22?
14. Is this the final answer? Why not?
15. Pipe the output of line 11 into `head` and take a screenshot to submit as answer for class activity 2.
16. What other patterns can one look for in chr22 sequence?

# Activity 3 - Get hg38 GTF file, investigate genes on chr22 and make a custom BED file of all exons on chr22
1. Make a new directory in your personal directory: `mkdir hg38`
2. Go to hg38 directory and get the GTF file for the RefSeq annotation: `curl -s https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/genes/hg38.ncbiRefSeq.gtf.gz > hg38.ncbiRefSeq.gtf.gz`
3. Unzip the gtf file and save it as `hg38.ncbiRefSeq.gtf`
4. How many lines are in the GTF file?
5. Take a look at the GTF file using `head`
6. Too many lines can be hard to read. Can you try to see if you can print only one line on the screen using a combination of `head` and `tail` (and `pipe`)?
7. Look at the individual tab separated fields of one line and cross check with GTF/GFF file format description in lecture slides to understand what is listed in each field of the GTF file.
8. Can you use a combination of `grep` for chr22 and `wc -l` to see how many features are on chr22?
9. Is there a quick way to get an estimate of how many protein coding genes are on chr22? Think about what is a feature unique to only protein coding genes that is listed in the third field of the GTF file. Then you can combine multiple `grep` commands to look for particular strings and pipe it into line count.
10. So how many (estimated) protein coding genes are on chr22? Take a screenshot of your code and answer from line 9 to submit one of the two activity 3 answers.
11. To make a custom BED file of all exons on chr22, `grep` first for "chr22" in the GTF file, pipe it into this awk one-liner: `awk 'OFS="\t" {if ($3=="exon") {print $1,$4-1,$5,$3,$6,$7}}' > chr22exon.bed`
12. Use `head` to view the top 10 lines of the newly generated BED file.
13. Take a screenshot of steps 11-12 to submit your work as part of activity 3.
