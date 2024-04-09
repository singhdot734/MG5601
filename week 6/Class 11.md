# Activity 1
1. In your folder in the scratch directory, create a new folder called fastQC.
Then move to the new folder
2. Copy the file SRX12339763_1.fastq from the class_data folder in the scratch directory to your new folder.
3. Load the bioinfo conda environment you made with the command `conda activate bioinfo`
4. Make a new directory called output.
5. Run fastQC on the demo file using the command `fastqc -o output/ --extract SRX12339763_1.fastq`.
`-o` specifies that the output of the program should be put in the output folder you made, and `--extract` means the contents should be unzipped.
6. In the output folder you should see a .zip file, a .html file, and a folder named demo_fastqc
7. Download the SRX12339763_1_fastqc.html file to your computer and open it.
It should open a new tab in your browser.
8. What does this summary tell you about the quality of the sequencing data?
