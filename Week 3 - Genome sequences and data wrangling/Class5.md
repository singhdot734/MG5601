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



