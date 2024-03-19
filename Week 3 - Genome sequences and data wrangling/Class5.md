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

