# Activity 1 - Genome GC/AT content and our first regular expression
1. Human GC content is 42%. So that would predict that AT content of chr22 will be 58% and hence 29% should be A. Can we determine that ourselves?
2. Recall from the last class how many nucleotides or base-pairs are in chr22.
3. How can we count the number of "A" in chr22.fa? Ha! thats simple! `grep "A" chr22.fa | wc -c` Or is it?
4. Last time when we were counting number of EcoRI sites, we first used `grep -v ">"` to ignore the first line.
5. Next, again like last time, we can pipe into another `grep` for "A" with options `-o` (output only the string that is being searched) and `-n` (print line numbers where string is found). Pipe this to `wc -l` to count lines. Is this it? Do the math to check.
6. Hmm...let's see what is going on. In the last step, instead of piping into `wc -l` pipe into `uniq -c` (show unique lines with string count for each line) and then pipe into `head`
7. So there are no A's on lines 210206-210210? 
8. To check what is going on, try this `head -210210 chr22.fa | tail -10`
9. Whats up with some lines with sequence in lowercase? This is how repetitive sequence is indicated.
10. `grep "A"` does not count "a". How can you count these?
11. This is where we can use regular expression to search for either "A" or "a". This can be done by doing `grep` for `"[A|a]"`.
12. So, is this it? Do the math.
13. Just do `head chr22.fa`. What do you see and how could this affect your calculation?
14. So, how do you account for the Ns?
15. Submit your code that you used to count the numerator and denominator for the final calculation.
16. Now, download another chromosome as you did chr22 (get a short one) and calculate A content of that chromosome. Take a screenshot of your command to count A's in this chromosome to submit as classwork.

# Activity 2 - BED file of chr22 transcript start and ends
1. For the second activity, first we will make a bed file of transcripts on chr22.
2. In activity 3 in the last class, we used a combination of `grep` and `awk` commands to make a bed file of exons, as follows:
3. `grep "chr22" hg38.ncbiRefSeq.gtf | awk 'OFS="\t" {if ($3=="exon") {print $1,$4-1,$5,$3,$6,$7}}' > chr22exon.bed`
4. Modify the above command to instead get only those rows that have "transcript" in column 3. Output it as chr22tra.bed
5. Take a look at the file using `head`.
6. We have the start and end positions of all transcripts on chr22 but there are a few issues to address before this file can be useful.
7. One, we have rows that come from chr22 alternate assemblies that may not have well annotated transcripts. That is what you see when you simply do `head chr22tra.bed`. It looks like `chr22_KI270928v1_alt    82357   137686  transcript      .       -`
8. Use the following command to see all unique entries in column 1 along with their counts (we did something very similar while counting EcoRI sites in chr22): `cat chr22tra.bed | cut -f 1 | sort | uniq -c | head`
9. How can we limit our selection to include only those rows that are just "chr22"?
10. If we add `\b` to end of our grep string, it will match only those strings that end with that word, like this: `grep "chr22\b"`. This will get only those instances of chr22 where the ending matches exactly to the string.
11. Make a new bed file with chr22 transcripts that have only "chr22" in column 1. Output it as chr22tra1.bed. Use `head` to see the file. Take a screenshot to submit as classwork.
13. Now the next issue is that our bed file rows do not have any gene ID or transcript ID for us to track which gene/transcript do these features belong to.
14. Gene and transcript IDs are there in column 9. Let's take a look at entry for TBX1 gene: `grep "chr22" hg38.ncbiRefSeq.gtf | grep "TBX1" | head -1`
15. Our next goal is to get one of the three features from column 9 (gene_id "TBX1"; transcript_id "NM_001379200.1";  gene_name "TBX1") into column 4 or 5 of the bed file. More on this next week.
