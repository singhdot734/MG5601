# Activity 1
1. In your folder in the scratch directory, create a new folder called fastQC.
Then move to the new folder
2. Copy the file demo.fastq from the class_data folder in the scratch directory to your new folder.
3. Load the bioinfo conda environment you made with the command `conda activate bioinfo`
4. Make a new directory called output.
5. Run fastQC on the demo file using the command `fastqc -o output/ --extract demo.fastq`.
`-o` specifies that the output of the program should be put in the output folder you made, and `--extract` means the contents should be unzipped.
6. In the output folder you should see a .zip file, a .html file, and a folder named demo_fastqc
7. Download the demo_fastqc.html file to your computer and open it.
It should open a new tab in your browser.
8. What does this summary tell you about the quality of the sequencing data?
9. Assignment: How many lines are in the demo.fastq file? How many sequence reads are in the demo.fastq file?  

# Activity 2
1. We will trim the reads in demo.fastq file that are below a certain threshold.
2. Make sure the bioinfo environment is activated.
3. Run the trimmomatic command with single-end option, on file demo.fastq to produce demo_trimmed.fastq where bases on 3'-end that are below quality score threshold of 10 are trimmed and quality scores are in phred-33 format: `trimmomatic SE demo.fastq demo_trimmed.fastq TRAILING:10 -phred33`
4. Run fastqc as in activity 1 on the demo_trimmed.fastq file. Download and view the .html file.
5. Assignment: Paste screenshots of "Per base sequence quality" and "Sequence Length Distribution" side-by-side for the untrimmed and trimmed demo fastq file. Briefly discuss the differences that you notice.
