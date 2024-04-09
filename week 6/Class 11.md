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

# Activity 3
1. Make sure the bioinfo environment is activated.
2. We will use utilites in the bioinfon environment to get data from the paper of our interest.
3. First, lets look at the summary of data in the BioProject: `esearch -db sra -query PRJNA648706`
4. Now lets get info on each of the runs (samples) in the BioProject: `esearch -db sra -query PRJNA648706 | efetch -format runinfo > runinfo.csv`
5. Download the runinfo.csv file and open it on your computer. Note that run ids are in column 1 (we use this next).
6. Assignment: How many replicates are present in the study for PUM1 and PUM2 knockouts? Is the cell line being used of male or female origin?
7. We can get one sample at a time but wouldn't it be better to get all samples we are interested in at once! We will use "parallel" function of unix to do this.
8. Lets first make a text file with run ids from this project: `cat runinfo.csv | cut -f 1 -d ',' | grep SRR > runids.txt`
10. Lets try what "parallel" can do: `cat runids.txt | parallel "echo This is number {}"`
11. Assignment: How many IDs printed on your screen? Take a screenshot of step 10 command and its output to submit.
12. We will use only couple of samples not all twelve. Open the runids.txt using the nano editor as follows and remove all but any two samples that end in numbers in 63-66 or 71-74 range:`nano runids.txt`. Control X to exit and save changes.
13. Now we will use this runids.txt file with fastq-dump to fetch the two samples specified, extract 100k sequences from them and split them by read1 and read 2 (this is a paired-end sample): `cat runids.txt | parallel fastq-dump -X 100000 --split-files {}`
14. There should be four new fastq files in your directory now.
15. Run fastqc as in activity 1 on one of the new fastq files and take a look at the sample properties in the html file.
16. Paste screenshots of "Per base sequence quality" and "Sequence Length Distribution" of one of your PUM fastq files. Briefly discuss how the quality scores and sequence lengths of this data compares to demo.fastq file above.


