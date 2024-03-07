## Activity 1 - Getting data
1. Log into OSC (ondemand.osc.edu)
2. Go to your personal scratch directory
`/fs/scratch/PAS2698/yourdir/`
3. Make a new directory here called 'unixmore'; use `mkdir`
4. Remember, the general structure of unix commands is: program flags input output. In this case, program is "mkdir" and output is "unixmore", which can be run as follows:
`mkdir unixmore`
5. Enter/change directory to 'unixmore'; use `cd`
6. In this directory, we will get a file with yeast (S. cerevisiae) genome features and investigate some features of yeast genes and genome using basic unix commands. Originally, these files are available from Saccharomyces Genome Database (SGD; yeastgenome.org) but we will get the files that are hosted by biostar handbook (allegedly, links to files and datasets in SGD can change frequently).
7. Two commands to get data from http (or ftp) links are `curl` (step 8) or `wget` (step 9). The usage is like any other unix command: program flags input output. We will use `>` to direct the output to a file.
8. Getting data with `curl` with option `-s` (silent) that will not print progress meter or errors on screen: `curl -s http://data.biostarhandbook.com/data/SGD_features.tab > SGD_features.tab`
9. Getting data with wget (no flags available): `wget http://data.biostarhandbook.com/data/SGD_features.tab > SGD_features.tab`
10. Check if the file is now in your directory; use `ls` or `ll`
11. Now get the README file that explains what is in the SGD_features.tab file: `wget http://data.biostarhandbook.com/data/SGD_features.README > SGD_features.README` (or use `curl`).
12. Use `ls` or `ll` to check directory contents again. The README file should also be there now.
13. View contents of the .tab file. You can use `more` or `less`. Press “space” to move forward, “b” to move backward, q or ESC to exit. Using `head` or `tail` can show 10 lines by default. Again, usage is same: program flags input output. In this case only program and input needs specified. Output will be on your screen. Can you make sense of what type of information is there in the file?
14. View the README file and read through the description. Now look at a few lines of the .tab file (use `head` or `tail`; use `-n` followed by a small number as a flag) and it may make more sense what all information is in the .tab file. 

## Activity 2 - Getting some basic info about .tab file
1. How many lines, words, characters are in the file? Use word count program `wc`. Remember usage: program flags input output.
2. Try `wc` with lines `-l` flag. Now you will get only the number of lines.
3. Now we will use the `cat` program to read the .tab file and start producing an output stream, which can go to your screen (if no output specified) or can be redirected into another program using pipe `|`.
4. Try `cat SGD_features.tab`
5. Now try to pipe output of `cat SGD_features.tab` into `wc -l`. This will also give you the file line count. All roads lead to Rome!
6. Next we will look for certain patterns or strings using `grep`. So there are some genes labeled "Dubious". Use `grep Dubious filename` to see these lines.
7. We want to start linking multiple programs together. So use `cat` to open stream from SGD_features.tab and pipe it into `grep Dubious`. Output should be the same as step 6 above.
8. Now pipe the output from step 7 into `wc -l`. So how many "Dubious" genes are there?
9. Next we will practice `cut` `sort` and `uniq` programs.
10. If you noticed, column 2 of the .tab file is "Feature types". We will use `cut` program to get column 2 into a new file and then get a summary of feature types in yeast genome. To do this,  open stream using `cat`, pipe it to `cut` using flag `-f 2` (field #2) and direct the output using `>` to a new file named "types.txt".
11. Use `ls` or `ll` to see contents.
12. Now use `cat` to open stream from "types.txt", pipe it into `sort` and then into `head` to see what you have.
13. Next instead of head, pipe output of `sort` from step 11 into `uniq` with flag `-c` for counts; then pipe into `head` to see what you have.
14. Next we will reverse sort the output from step 12 by number of features. So instead of head, pipe output of `uniq -c` from step 12 into `sort -rn`. You should have a summary of features print on your screen.
15. Write the output of step 13 into a new file called "feature_types.txt".
16. Use `ls` or `ll` to make sure it is there.
17. Use `more` command to show the contents for the "feature_types.txt" file. Take a screenshot of the output to submit via carmen.

## Activity 3 - How many protein coding genes (feature type = ORF) are in this reference file?
1. Use `cat`, `cut`, `grep` and `wc -l` on SGD_features.tab file, piping output from each program to the next one, to count number of protein coding genes. Take a screenshot of your code and answer to submit as answer to one of the questions for today.
2. Can you count how many of the ORFs are not "Dubious"? This will be similar to step 1 above but you will need to include/select multiple columns using `cut -f 2,3,4` then `grep` ORF and then pipe this into another `grep` with a flag `-v` (lines that do not match a string or pattern, which is Dubious in this case) and then pipe it into `wc -l`. So how many ORFs are not dubious?

## Activity 4 - PUF genes in yeast
1. Use `cat`, `grep` and `wc -l` on SGD_features.tab file to count number of PUF genes in yeast. How many are there?
2. Drop `wc -l` to output the lines on your screen and look through information about these genes.
3. Column 5 in SGD_features.tab has standard gene names. Standard name for PUF1 is JSN1 and for PUF5 is MPT5. JSN = Just say no! More about this in the class!
4. Use `cat`, `grep` (for PUF), `cut` to get columns 5 (standard gene name), 9 (Chromosome), 10 (Start_coordinate), 11 (Stop_coordinate) and 12 (Strand) and write this output into a new file "pufSummary.txt".
5. Use `ls` or `ll` to confirm and `head` (or `more`) to check contents of "pufSummary.txt". Take a screen shot of lines with step 4 and 5, showing the content of "pufSummary.txt". This will be submitted as another answer for today.
6. Try this to calculate length of each PUF gene: Use `cat` to stream "pufSummary.txt", pipe into `awk '{ $6 = $3 - $4 } 1'` and write the output into a new file "pufSummary2.txt".
7. Use `ls` or `ll` to confirm and `head` (or `more`) to check contents of "pufSummary2.txt". A new column appears at the end. Why are some numbers negative?

## Activity 5 - Comparing yeast PUF proteins to human Pumilio
1. Go to your personal scratch directory
`/fs/scratch/PAS2698/yourdir/`
2. Make a new directory here called 'pumilios' `mkdir`
3. Enter/change directory to 'pumilios' `cd`
4. Fasta format sequences of the six yeast PUF proteins is already available in `/fs/scratch/PAS2698/scPUFs`. To copy this directory and its contents into your pumilios directory, go to source directory `/fs/scratch/PAS2698/` and copy scPUFs as follows `cp /fs/scratch/PAS2698/scPUFs /fs/scratch/PAS2698/yourdir/scPUFs`. Don't forget to change yourdir in the destination path to your personal directory. 
5. We will get fasta format sequence of human PUM1 from NCBI using `efetch` program that we installed last week. Like unix commands, `efetch` uses flags to specify options: `-db` is database from which to get sequence, `-id` is ID of sequence, `-format` is file format to get.
6. This command will get the human PUM1 sequence in fasta format: `efetch -db protein -id NP_001018494.1 -format fasta`. Don't forget to write the output to a file (you can name it "hsPUM1.fa".
7. Use yeast PUF and human PUM1 protein sequences to make a multiple sequence alignment using the cobalt tool at: https://www.ncbi.nlm.nih.gov/tools/cobalt/cobalt.cgi
8. There is a question to answer in today's assignment based on this sequence alignment.

## Enjoy the Spring Break!!
