# FASTQ & fastqc

This short tutorial outlines the steps to prepare a new directory, copy a FASTQ file for analysis, and run quality control using FastQC.

[FASTQ](https://en.wikipedia.org/wiki/FASTQ_format) files are text files containing sequence data with a quality (Phred) score for each base, represented as an ASCII character.   
The quality score is an integer (Q) which is typically in the range 2 - 40, but higher and lower values are sometimes used. In particular, versions 1.8 and later of the Illumina platform generate reads with Q scores up to 41.  
![](fastq_fig.jpg)  
*FASTQ read with 50nt in Illumina format (ASCII_BASE=33).*  
There are always four lines per read:
1. The first line starts with '@', followed by the label (read identifier);  
2. The second line contains the observed sequence;
3. The third line starts with '+'. In some variants, the '+' line contains a second copy of the label;
4. The fourth line contains the Q scores represented as ASCII characters.

## Steps

### 1. Create Analysis Directory

First lets create a directory where the analysis will be conducted. We can use cd to ensure we're in the home folder

```
cd ~
mkdir -p Analysis
```

The -p flag ensures that the command does not return an error if the directory already exists and allows the creation of nested directories if necessary.

### 2. Copy FASTQ File
Next, we'll copy a FASTQ file from the shared directory to the folder you just created.

```
cp ~/Shared/raw_data ~/Analysis/
```
Here we specify ```~/``` before the folder name to be explicit that it's in the home folder, although not technically required in this moment.

Enter the folder, and check that the file is there, and how large it is 
<details>
  <summary>Commands here (try to remember yourself first!)</summary>
  
  use ```cd``` to enter the folder, and then ```ls -l``` to list the contents of the folder, and it's details
  ```
  cd Analysis
  ls -l
  ```
  
</details>


### 3. Extract all .gz Files 

As our .gz file contains only one compressed file, we use (To run this command, you must ensure that you are in the file's directory! **IF not you have to specify the path**):

```
gunzip *.gz
```

### 4. Run FastQC on a single file
Now let's run FastQC on the FASTQ file to assess its quality. FastQC provides a comprehensive overview of data quality, including basic statistics, per-base quality scores, sequence content, and GC content.

The command is pretty simple (we just pick up the first one)!
```
fastqc BAQ895.2_54_L001_R1_001.fastq
```
You can also use the -o option specifying the output directory for the results to be more organised of if you have many files.

Use ```ls``` to check that some output has been generated!

### 5. View the file
Use filezilla (or a different transfer programme) to download the html you just made, and open it on your local computer to inspect the outputs.

### 6. What to do when there are many files?

 It's commonly in genomics workflows summarize quality control metrics from tools like FastQC, STAR, HISAT2, Bowtie2, SAMtools, and more.

Let's try

```
fastqc *.fastq



### 7. Run MultiQC on a Directory

Stay into the directory containing the raw fastq  files, then run:

```
multiqc .
```
🔹 The . tells MultiQC to scan the current directory.
🔹 It automatically detects supported logs and generates a summary report.

