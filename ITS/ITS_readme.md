QIIME2 analysis of ITS data
========

1. [Rationale](#Rationale)
2. [ITSxpress based tutorial](#itsxpress-based-tutorial)
   1. [Import ITS data](#step1-import-its-data)
   2. [Quality controlling sequences and building Feature Table and Feature Data](#step2-quality-controlling-sequences-and-building-feature-table-and-feature-data)
   3. [Summarizing Feature Table and Feature Data](#step-3-summarizing-feature-table-and-feature-data)
   4. [Taxonomy assignment](#step-4-taxonomy-assignment)
3. [Primer trimming based tutorial](#primer-trimming-based-tutorial)


# Rationale
To focus on the individual steps we decided to follow the 16S case study (ATACAMA) as the main workflow.  
In general, this pipeline can be used for any marker in metabarcoding investigation but in some steps the biological 
characteristics (and therefore the issues) of certain marker must be considered.

We therefore decided to show the differences between the workflow to be used for the 16S or ITS.  
**We remind you, however, that each marker has a different evolutionary background given by the selective pressures, 
so this factor should be considered each time when using scripts.**

![alt text](https://image.slidesharecdn.com/suryasahametagenomicstools-130507160129-phpapp02/95/tools-for-metagenomics-with-16sits-and-whole-genome-shotgun-sequences-3-638.jpg?cb=1369657346)

Just few concept about ITS region. When we use the term ITS we refer to the region spanning from ITS1 to ITS2.  
His average length in Fungi is about 615nt so actually is not possible to sequence it completely by using Illumina sequencing.  
Usually, just the ITS1 or the ITS2 are amplified and sequenced.  
***We are going to focus on an example based on ITS1***

Before starting to analyse fungal ITS data just a couple of consideration:
![its](ITS_region_schema.png)  
*[https://benjjneb.github.io/dada2/ITS_workflow.html](https://benjjneb.github.io/dada2/ITS_workflow.html)*  

We could observe three different situations:  
- The amplified ITS1 region is longer than the read lengths, the forward and reverse reads overlap to capture the full amplified ITS region, but do not read into the opposite primer;
- The amplified ITS1 region is longer than the read lengths, the forward and reverse reads do not overlap to capture the full amplified ITS region. **This is the worst condition**; 
- The amplified ITS1 region is shorter than the read lengths, and the forward and reverse reads extend into the opposite primers which will appear in their reverse complement form towards the ends of those reads.    


**PAY ATTENTION**: if the amplified region is longer than about 580 nt we are unable to merge reads.


Considering both the scenarios pictured above typically occur within the same ITS1 dataset, a critical addition to ITS 
workflows is the removal of primers on the forward and reverse reads, in a way that accounts for the possibility of read-through into the opposite primer.  

Since the differences mainly concern steps required to properly prepare your raw data we will focus only on two soil samples (PRJNA483055).  
We are going to reuse the sane data Adam Rivers proposed in its [tutorial](https://forum.qiime2.org/t/q2-itsxpress-a-tutorial-on-a-qiime-2-plugin-to-trim-its-sequences/5780).
This is to simplify the interpretation of the results as much as possible. By avoiding ecological problems we can focus on the differences given only by the nature of the marker itself.  

The ITS1 fungal region was amplified by using the [ITS1f/ITS2 primer pair](https://earthmicrobiome.org/protocols-and-standards/its/).  

|Primer|Sequence|
|:-:|:-:|
|Forward|CTTGGTCATTTAGAGGAAGTAA|  
|Reverse|GCTGCGTTCTTCATCGATGC|   


# ITSxpress based tutorial
***Considering the whole workflow requires more than 1 hour, we are just going to discuss the main steps and copy already processed data for time-consuming steps.***  
```
cd 

mkdir ITSxpress_ITS_tutorial && cd ITSxpress_ITS_tutorial
```
*In BASH, the `&&` allows you to perform two action on the same line*  

If not already active, remember to activate the QIIME2 environment:
```
conda activate qiime2-2021.8
```
 
## Step1: Import ITS data
Let's start by downloading the data:
```
wget https://github.com/USDA-ARS-GBRU/itsxpress-tutorial/raw/master/data/sample1_r1.fq.gz
wget https://github.com/USDA-ARS-GBRU/itsxpress-tutorial/raw/master/data/sample1_r2.fq.gz
wget https://github.com/USDA-ARS-GBRU/itsxpress-tutorial/raw/master/data/sample2_r1.fq.gz
wget https://github.com/USDA-ARS-GBRU/itsxpress-tutorial/raw/master/data/sample2_r2.fq.gz
wget https://raw.githubusercontent.com/USDA-ARS-GBRU/itsxpress-tutorial/master/data/manifest.txt
```

Finally, copy the `mapping.txt` file containing the samples metadata:
```
cp /home/Share/ITSxpress_ITS_tutorial/mapping.txt .
```

Now we can import the **FASTQ** data into a QIIME2 artifact:   
```
qiime tools import \
  --type SampleData[PairedEndSequencesWithQuality] \
  --input-format PairedEndFastqManifestPhred33\
  --input-path manifest.txt \
  --output-path sequences.qza
```
```
qiime demux summarize \
  --i-data sequences.qza \
  --o-visualization sequences.qzv
```

## Step2: Quality controlling sequences and building Feature Table and Feature Data
  
### Quality filter of ITS (optional for Friday groups)
One issue with ITS (and other marker genes with vast length variability) is *read-through*, which occurs when read lengths are longer than the amplicon itself!
Also in this case we want to trim primers from either end of the sequence to eliminate read-through issues.
The polymerase will read through the amplicon, the primer, the barcode, and on into the adapter sequence.  
This is non-biological DNA that will cause major issues downstream, e.g., with sequence classification. So we want to trim primers from either end of the sequence to eliminate read-through issues.  
It is, also, crucial to note that primer sequences correspond to conserved region that are prone to generate confusion in taxonomic classification.  

---
**PAY ATTENTION**:
This is particularly true for ITS1 sequences as highlighted by [Nilsson R.H., et al. 2009](https://doi.org/10.1111/j.1574-6968.2009.01618.x).
---

In order to extract exclusively the ITS1 region we are going to apply [**ITSxpress**](https://doi.org/10.12688/f1000research.15704.1).  
Briefly, it works as follows:
1. Merges reads (if paired-end) using **BBMerge**;
2. Temporarily clusters highly similar sequences that are common in amplicon data using **VSEARCH**;
3. Identifies ITS start and stop sites using **hmmsearch**  on the representative sequences;
4. Trims each original, merged sequence with quality scores, returning the merged or unmerged sequences with quality scores in a .qza file;  

To install ITSexpress plugin in QIIME2 you need to use the following code. Of course we've already installed it for you:  
:stop_sign:  
```
conda install -c bioconda itsxpress
pip install q2-itsxpress

qiime dev refresh-cache
```

Following it is shown the applied command line. **DO NOT EXECUTE IT, BECAUSE IT IS QUITE SLOW**.  
:stop_sign:  
```
qiime itsxpress trim-pair-output-unmerged\
  --i-per-sample-sequences sequences.qza \
  --p-region ITS1 \
  --p-taxa F \
  --p-cluster-id 1.0 \
  --o-trimmed trimmed.qza
```

To import the processed data just type the following line
```
cp /home/Share/ITSxpress_ITS_tutorial/trimmed.qza .
```

Let's visualize whether something is changed in our sequences:  
```
qiime demux summarize \
 --i-data trimmed.qza \
 --o-visualization trimmed.qzv
```

We are ready to apply DADA2 and obtain ASVs.  **DO NOT EXECUTE IT, BECAUSE IT IS QUITE SLOW**.  
:stop_sign:  
```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs trimmed.qza \
  --p-trunc-len-r 0 \
  --p-trunc-len-f 0 \
  --p-n-threads 20 \
  --output-dir dada2out
```
To import the processed data just type the following line
```
cp -r /home/Share/ITSxpress_ITS_tutorial/dada2out .
```


## Step 3: Summarizing Feature Table and Feature Data
We have just generated the artifacts containing the feature table and corresponding feature sequences.  
Let's tabulate all the data:  
```
qiime feature-table summarize \
  --i-table dada2out/table.qza \
  --o-visualization tableviz.qzv
```
```
  qiime metadata tabulate \
  --m-input-file dada2out/denoising_stats.qza \
  --o-visualization denoising_stats.qzv
```

# Step 4 Taxonomic assignment 

## ITS taxonomcic assignment 
The QIIME 2 plugin [feature-classifier](https://docs.qiime2.org/2019.1/plugins/available/feature-classifier/) 
supports taxonomic classification of features using a variety of methods, including Naive Bayes, vsearch, and BLAST+.
We are going to use the Naive Bayes classifier that we've pre-trained by using the [**UNITE**](https://unite.ut.ee/repository.php) collection.
Following is listed the applied procedure on the files obtained from the UNITE repository and how the taxonomic classification was performed:  
**DO NOT EXECUTE THE FOLLOWING CHUNK OF CODE**.  
:stop_sign:  
```
tar xvfz C5547B97AAA979E45F79DC4C8C4B12113389343D7588716B5AD330F8BDB300C9.tgz

qiime tools import \
  --type 'FeatureData[Sequence]' \
  --input-path sh_qiime_release_10.05.2021/sh_refs_qiime_ver8_dynamic_10.05.2021.fasta \
  --output-path unite.qza

qiime tools import \
  --type 'FeatureData[Taxonomy]' \
  --input-format HeaderlessTSVTaxonomyFormat \
  --input-path sh_qiime_release_10.05.2021/sh_taxonomy_qiime_ver8_dynamic_10.05.2021.txt \
  --output-path unite-taxonomy.qza

qiime feature-classifier fit-classifier-naive-bayes \
  --i-reference-reads unite.qza \
  --i-reference-taxonomy unite-taxonomy.qza \
  --o-classifier classifier.qza
  
qiime feature-classifier classify-sklearn \
  --i-classifier classifier.qza \
  --i-reads dada2out/representative_sequences.qza \
  --o-classification taxonomy.qza
```

No we are ready to copy the performed taxonomic classification and generate visualization files:
```
cp /home/Share/ITSxpress_ITS_tutorial/taxonomy.qza .

qiime metadata tabulate \
  --m-input-file taxonomy.qza \
  --o-visualization taxonomy.qzv


qiime taxa barplot \
  --i-table dada2out/table.qza  \
  --i-taxonomy taxonomy.qza \
  --m-metadata-file mapping.txt \
  --o-visualization taxa-bar-plots.qzv
```

# Primer trimming based tutorial

## Step 1: Import data

Create a new folder  
```
cd

mkdir PT_ITS_tutorial && cd PT_ITS_tutorial
```

We are going to reuse soil samples of the _ITSexpress tutorial_: 
```
cp ../ITSxpress_ITS_tutorial/{sequences.qza,mapping.txt} .
```

## Step 2: Primer trimming
*Note that we trim the forward primer and the reverse complement of the reverse primer from the forward reads 
(the forward primers have already been trimmed in the raw reads, but we will demonstrate forward + reverse trimming here since attempting to trim the forward read will not hurt).
We trim the reverse primer and reverse complement of the forward primer from the reverse reads.*
The primer trimming is performed by using the tool `cutadapt`[^1].  

|Primer|Sequence|Reverse-Complement|
|:-:|:-:||
|Forward|CTTGGTCATTTAGAGGAAGTAA|TTACTTCCTCTAAATGACCAAG|  
|Reverse|GCTGCGTTCTTCATCGATGC|GCATCGATGAAGAACGCAGC|  

```
qiime cutadapt trim-paired \
  --i-demultiplexed-sequences sequences.qza \
  --p-front-f CTTGGTCATTTAGAGGAAGTAA \ 
  --p-adapter-f GCATCGATGAAGAACGCAGC \ 
  --p-front-r GCTGCGTTCTTCATCGATGC \   
  --p-adapter-r TTACTTCCTCTAAATGACCAAG \
  --p-cores 20 \
  --o-trimmed-sequences sequences-trimmed.qza
```

```
qiime demux summarize \
 --i-data sequences-trimmed.qza \
 --o-visualization sequences-trimmed.qzv
```

***Why didn't we just cut the primers using length?***

<details>
  <summary markdown="span">Let's try to compare our data pre- and post trimming. Are there any difference?</summary>

    According to Taylor et al. 2016 71, the ITS amplicon length should range among 267-511 bp.  
    So with the read lengths we are using here (250 nt) we should not see any read-through.
    Please note, this is a specific case reagarding fungi, for other Eukaryotes the ITS length ranges could be wider.

</details>

## Step 3: Summarizing Feature Table and Feature Data
So let's denoise our data!!!  
**DO NOT EXECUTE THE FOLLOWING CHUNK OF CODE**.  
:stop_sign:  
```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs sequences-trimmed.qza \
  --p-trunc-len-f 0 \
  --p-trunc-len-r 0 \
  --p-max-ee-f 3 \
  --p-max-ee-r 3 \
  --p-n-threads 20 \
  --o-representative-sequences dada2-repseq_ITS.qza \
  --o-table dada2-table_ITS.qza \
  --o-denoising-stats dada2-stats_ITS.qza
```

Let's copy pre-processed data and tabulate all the available info:  
```
cp /home/share/PT_ITS_tutorial/dada2-* .

qiime metadata tabulate \
  --m-input-file dada2-stats_ITS.qza \
  --o-visualization dada2-stats_ITS.qzv

qiime feature-table summarize \
  --i-table dada2-table_ITS.qza \
  --o-visualization dada2-table_ITS.qzv \
  --m-sample-metadata-file mapping.txt

qiime feature-table tabulate-seqs \
  --i-data dada2-repseq_ITS.qza \
  --o-visualization dada2-repseq_ITS.qzv
```

## Step 4 Taxonomic assignment 

## ITS taxonomcic assignment 
The QIIME 2 plugin [feature-classifier](https://docs.qiime2.org/2019.1/plugins/available/feature-classifier/) 
supports taxonomic classification of features using a variety of methods, including Naive Bayes, vsearch, and BLAST+.

`q2-feature-classifier` contains several different classification methods: **classify-consensus-blast** and 
**classify-consensus-vsearch** are both **alignment-based methods** that find a consensus assignment across N top hits.  
These methods take reference database `FeatureData[Taxonomy]` and `FeatureData[Sequence]` files directly, and do not need to be pre-trained.

The first step in this process is to assign taxonomy to the sequences in our `FeatureData[Sequence]` QIIME 2 artifact. 

We will initially perform the taxonomic classification by using [**classify-consensus-vsearch**](https://docs.qiime2.org/2019.1/plugins/available/feature-classifier/classify-consensus-vsearch/).
***Like the previous time-consuming step, we show you the used line but we are just copyng the pre-computed results.***  
:stop_sign:  
```
qiime feature-classifier classify-consensus-vsearch \
  --i-query dada2-repseq_ITS.qza \
  --i-reference-reads ../ITSxpress_ITS_tutorial/unite.qza \
  --i-reference-taxonomy ../ITSxpress_ITS_tutorial/unite-taxonomy.qza \
  --p-maxaccepts 20 \
  --p-perc-identity 0.9 \
  --p-threads 20 \
  --o-classification vsearch_taxonomy_ITS.qza
```
Now copy the produced data and generate visualization files:
```
cp /home/share/PT_ITS_tutorial/vsearch_taxonomy_ITS.qza .

qiime metadata tabulate \
  --m-input-file vsearch_taxonomy_ITS.qza \
  --o-visualization vsearch_taxonomy_ITS.qzv

qiime taxa barplot \
  --i-table dada2-table_ITS.qza \
  --i-taxonomy vsearch_taxonomy_ITS.qza \
  --m-metadata-file mapping.txt \
  --o-visualization taxa-bar-plots_vsearch_ITS.qzv
```

Now we're also going to use the _sklearn_ classifier.  

:stop_sign: 
```
qiime feature-classifier classify-sklearn \
  --i-classifier unite_ref/sh_refs_qiime_ver8_99_10.05.2021_classifier.qza \
  --i-reads dada2-single-end-rep-seqs_ITS.qza \
  --o-classification taxonomy_ITS_sklearn.qza 
```

Let's copy results and visualize them:  
```
cp /home/share/PT_ITS_tutorial/taxonomy_ITS_sklearn.qza .

qiime metadata tabulate \
    --m-input-file taxonomy_ITS_sklearn.qza \
    --o-visualization taxonomy_ITS_sklearn.qzv

  qiime taxa barplot \
  --i-table dada2-single-end-table_ITS.qza \
  --i-taxonomy taxonomy_ITS_sklearn.qza \
  --m-metadata-file mock-25-sample-metadata.tsv \
  --o-visualization taxa-bar-plots_ITS_SKELARN.qzv
```
---
**Are the obtained classifications comparable? Have a look [here](https://docs.google.com/spreadsheets/d/1e8AI8fgcDECRfXP5uMz1hpZyEoOMRx0Yo0DveuGl4PI/edit?usp=sharing).**

---
[^1]: [https://journal.embnet.org/index.php/embnetjournal/article/view/200/479](https://journal.embnet.org/index.php/embnetjournal/article/view/200/479)

[**Back to the program**](../README.md)