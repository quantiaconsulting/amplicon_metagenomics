# Table of content
1. [Start the analysis](#start-the-analysis)
   1. [Experimental information](#experimental-information)
      1. [Obtain Data](#obtain-data)
      2. [The Metadata file](#the-metadata-file)
      3. [Start QIIME2 session](#start-the-analysis)
2. [STEP1: Importing data, summarize the results, and examining quality of the reads.](#step1-importing-data-summarize-the-results-and-examining-quality-of-the-reads)
   1. [Explore QIIME2 Objects](#explore-qiime2-objects)
3. [STEP2: Quality controlling sequences and building Feature Table and Feature Data](#step2-quality-controlling-sequences-and-building-feature-table-and-feature-data)
   1. [Quality filter of 16S](#quality-filter-of-16s)
4. [STEP3: Summarizing Feature Table and Feature Data](#step3-summarizing-feature-table-and-feature-data)
   1. [Summarize 16S data](#summarize-16s-data)
5. [STEP4: Taxonomy assignment](#step4-taxonomy-assignment)
   1. [16S taxonomy assignment ](#16s-taxonomy-assignment)
6. [STEP5: Generating a phylogenetic tree](#step5-generating-a-phylogenetic-tree)
7. [STEP6: Analyzing Alpha and Beta diversities](#step6-analyzing-alpha-and-beta-diversities)
   1. [Time to test](#time-to-test)
8. [BONUS: Differential abundance analysis in QIIME2](#bonus-differential-abundance-analysis-in-qiime2)

---
**Basic Advice**: before the chunk of command line you will find two emoticons:  
1. :walking: The chunk of code must be executed.
2. :stop_sign: The chunk of code must not be executed, but you just need to the prepared results.   

---

# Start the analysis
In this tutorial you will use QIIME2 to perform an analysis of soil samples from the Atacama Desert in northern Chile.
**You can find the starting tutorial on the official page of QIIME2** ( [link text](https://docs.qiime2.org/2020.2/tutorials/atacama-soils/))

_"The Atacama Desert is one of the most arid locations on Earth, with some areas receiving less than a millimeter of rain per decade.  
The soil microbiomes profiled in this study follow two east-west transects, **Baquedano** and **Yungay**, across which average soil 
relative humidity is positively correlated with elevation (higher elevations are less arid and thus have higher average soil relative humidity). 
Along these transects, pits were dug at each site and soil samples were collected from three depths in each pit."_

![Atacama Desert](dsc-5624-cc.jpg)

## Experimental information
*   The V4 region of the 16S rRNA gene was amplified from all community DNA extracts using barcoded primers 515F/806R. The expected amplicon length is about 300 nt.
![](V4_atacama.png)
*   Amplicon sequencing was performed on ***Illumina MiSeq platform*** and MiSeq control software version 2.2.0.

You can find the paper [here](https://msystems.asm.org/content/2/3/e00195-16.abstract) or in the course Reference folder.  

## Start the Analysis
Start renaming the work directory in  **qiime2-atacama-tutorial**.  
:walking:  
```
cd Analysis/
mv AttacamaAllOutputs/ qiime2-atacama-tutorial
```

```
cd qiime2-atacama-tutorial
```

## The Metadata file

Metadata play a key role in every ecological study. For how is familiar with QIIME1 this file corresponds to "mapping file" or for R user is the _env_ file in *vegan* package.

QIIME2 metadata are usually stored in a **TSV** (i.e. tab-separated values) file. These files typically have a `.tsv` or `.txt` file extension, 
though it does not matter to QIIME2 what file extension is used.

TSV files are simple text files used to store `tabular data`, and the format is supported by many types of software, such as editing, importing, and exporting from spreadsheet programs and databases. 
Thus, it’s usually straightforward to manipulate QIIME 2 metadata using a software like Microsoft Excel or (better) Google Sheets to edit and export your metadata files.

You can find the [file](https://docs.google.com/spreadsheets/d/1a1NFqpBjwb8Ul0c4O68IVFb9JUn5BjLdyHr422LMfE0/edit#gid=1988763045) in the Lab folder of today or download it directly from QIIME2 to your working area by running the command:  
:walking:  
```
wget \
  -O "sample-metadata.tsv" \
  "https://data.qiime2.org/2023.9/tutorials/atacama-soils/sample_metadata.tsv"
```

**If you would like to play around the sample-metadata.tsv file, please copy it into another folder and have fun with it.**  
**Remember this *sample-metadata_16S.tsv* file will be used throughout the rest of the tutorial.**

Since there is no universal standard for TSV files, it is important understand how QIIME2 will interpret the file’s contents to get the most out of your (meta)data!

Sample and feature metadata files stored in Google Sheets can be validated using **Keemei**:  
 1. Select *Add-ons* 
 2. Keemei  
 3. Validate *QIIME 2 metadata file* to validate metadata stored in Google Sheets.  

QIIME 2 will also automatically validate a metadata file anytime it is used by the software.  
However, using Keemei to validate your metadata is recommended because a report of all validation errors and warnings will 
be presented each time Keemei is run.


## Start QIIME2 session

As we discussed during the introduction to UNIX-based environment, we take advantage of using *virtual environments* to avoid affecting the main OS.  

Everytime we start a new session **(!!EVERY TIME!!)**, we need to activate the virtual environment containing our QIIME2 installation.  
:walking:  
```
conda activate /opt/shared_qiime2
```

### Pipeline Overview
Here is an overview of the general steps of the QIIME2 pipeline:  
![](steps.png)


## STEP1: Importing data, summarize the results, and examining quality of the reads.
When performing *paired end* sequencing we obtain 2 *fastq* file per each processed sample. It means that our data are already **demultiplexed**.  

It means we sequenced multiple samples in a single sequencing run and the machine has already seperated our data according to a specific indexing schema.  

---

![alt text](https://sfvideo.blob.core.windows.net/sitefinity/images/default-source/product-page-images/next-generation-sequencing/ngs_adapter_designs.png?sfvrsn=8ce20807_8)

For more details have a look here:  
- [Illumina support](https://support.illumina.com/content/dam/illumina-support/documents/documentation/system_documentation/miseq/indexed-sequencing-overview-guide-15057455-04.pdf)
- [Earth microbiome 16S protocols](http://www.earthmicrobiome.org/protocols-and-standards/16s/)

---

QIIME2 works with files named **artifacts**.  
A QIIME2 artifact contains both data and metadata.

Since QIIME2 works with artifacts instead of data files (e.g. FASTA files), you must create a QIIME2 artifact by importing your data.  
**In the next days we will see how to import and export different objects from QIIME2, and we are going always to use artifacts.**

Artifacts enable QIIME 2 to track, in addition to the data itself, the provenance of how the data came to be.
With an artifact’s provenance, you can trace back to all previous analyses that were run to produce the artifact, including the input data used at each step.  

We need to import our demultiplexed raw data into a qiime artifact.
:walking:  
```
qiime tools import \
    --type 'SampleData[PairedEndSequencesWithQuality]' \
    --input-path ~/Shared_folder/raw_data_tutorial \
    --input-format CasavaOneEightSingleLanePerSampleDirFmt \
    --output-path demux-paired-end.qza
```

We used the QIIME2 plugin *tools* and the function *import*, to embed our data into a ``qza`` artifact.  
To obtain an almost complete list of importable formats type the following line:  
:walking:  
```
qiime tools import --show-importable-formats --help
```

This is only one of the possible ways to import data into QIIME, and there are methods for multiplexed or different types of files.  
[A look at different way to import fastq files in QIIME](../DataImport_and_Tax_management/readme.md#some-example-of-data-import)


## Explore QIIME2 Objects

By importing the sequences you have generated your first object in QIIME2: `demux-paired-end.qza`.

Now lets visualize our data and obtain some specific statistics.
:walking:  
```
qiime demux summarize \
  --i-data demux-paired-end.qza \
  --o-visualization demux-paired-end.qzv
```

With the command  `demux summarize` you've generated a new type of objet: a file with `.qzv` extension. *Visualizations* are another type of data generated by QIIME2.  

Visualizations contain similar types of metadata as QIIME2 artifacts, including provenance information, 
but they are designed to show what is happening with the data.   

Both files (`qza` and `qzv`) can be **extracted** with a Compression/Decompression software (ex. gzip or unzip).

There are also different ways to [export QIIME2 objects](https://docs.qiime2.org/2022.11/tutorials/exporting/).
But remember that all existing provenance will be lost after exporting the files.

---

### Visualize with QIIME2 viewer

You can use [https://view.qiime2.org](https://view.qiime2.org) to easily view QIIME2 artifacts and visualizations files 
without requiring a QIIME installation.

Download, then drag-and-drop your demux-paired-end.qzv.

Automatically, the **Overview** tab is opened and some basic statistics about our data are available.  
In the **Interactive quality plot** tab we can access to the quality distributions of our data. The plot on the left 
represents the quality scores for the **forward** reads and the plot on the right presents the quality scores for the **reverse** reads.  
In this example we have **150-base forward and reverse reads**.  

We’ll use these plots to determine what trimming parameters we want to use for data denoising with DADA2 by using the **dada2 denoise-paired** plugin.

  
## STEP2: Quality filtering sequences and building Feature Table and Feature Data
[**Key concept: OTU or ASV**](https://docs.google.com/presentation/d/1XHQGInyWt9SGmyH6C4UA2-flloR3vQghIzJ2MDAKsqY/edit?usp=sharing)  
After importing the reads we inspected the sequence quality based on randomly selected samples.  
What we now need is to remove the "noise" introduced during amplification and sequencing which is specific to this methodology.

### Quality filter of 16S
Since we need the reads to be long enough to overlap when joining paired ends, we are just going to remove the first 5 bases of both forward and reverse reads,
but no trimming is applied with reads 3' end (full 150 length). 

This step is very subjective to your personal data! There are paremters chosen below as examples, but some may or may not be relevant to you! See the full parameter descriptsions in the [Qiime2 manual](https://docs.qiime2.org/2023.9/plugins/available/dada2/denoise-paired/).

Following are listed the lines to perform denoising. HOWEVER, considering it takes a while to complete we have completed this step for you, rather than wait.

:stop_sign:  
```
# DO NOT RUN THIS COMMAND
>qiime dada2 denoise-paired \
  --i-demultiplexed-seqs demux-paired-end.qza \
  --p-trim-left-f 5 \
  --p-trim-left-r 5 \
  --p-trunc-len-f 150 \
  --p-trunc-len-r 150 \
  --p-n-threads 2 \
  --p-min-overlap 10 \
  --p-max-ee-f 2 \                       
  --p-max-ee-r 2  \
  --p-n-reads-learn 50000 \
  --o-table table_16S.qza \
  --o-representative-sequences rep-seqs_16S.qza \
  --o-denoising-stats denoising-stats_16S.qza \
  --verbose
```

Let's import our already processed data.  
:walking:
```
cp ~/Share_folder/{table_16S.qza,rep-seqs_16S.qza,denoising-stats_16S.qza}  . 
```

We can now generate a qzv file with a table summarizing the denoising process, so we can discuss the effect it had on the data.
:walking:  
```
qiime metadata tabulate \
  --m-input-file denoising-stats_16S.qza \
  --o-visualization denoising-stats_16S.qzv
```

---
#### 454 and Ion Torrent data
You may be interested in this background if you are working with 454 or Ion Torrent data [**link**](https://benjjneb.github.io/dada2/faq.html#can-i-use-dada2-with-my-454-or-ion-torrent-data).

---

## STEP3: Summarizing Feature Table and Feature Data

## Summarize 16S data
You have produced two artifacts containing the feature table and corresponding feature sequences.  

Lets generate summaries for those, so that we can better understand what data they contain.
:walking:
```
qiime feature-table summarize \
  --i-table table_16S.qza \
  --m-sample-metadata-file sample-metadata.tsv \
  --o-visualization table_16S.qzv
```

```
qiime feature-table tabulate-seqs \
  --i-data rep-seqs_16S.qza \
  --o-visualization rep-seqs.qzv
```

As we can see in these outputs, some samples have very few reads remaining. Let's remove any samples that have <1000 reads from our files so we don't have blank columns or misrepresented data.
:walking:
```
qiime feature-table filter-samples \
  --i-table table_16S.qza \
  --p-min-frequency 1000 \
  --o-filtered-table table_16S_gt1k.qza
```
There are A LOT of ways to filter your data. You can read more [here](https://docs.qiime2.org/2023.9/tutorials/filtering/)

## Optional: Step 3.5
You may also wish to cluster your data into OTUs after performing the denoising with DADA2 (which outputs ASVs). That can be done with a simple vsearch command:
```
qiime vsearch cluster-features-de-novo \
  --i-table table_16S.qza \
  --i-sequences rep-seqs_16S.qza \
  --p-perc-identity 0.97 \
  --o-clustered-table table-16S-dn-97.qza \
  --o-clustered-sequences rep-seqs-16S-dn-97.qza
```
Then you can use the ```feature-table``` summarize and tabulate-seqs command again to visualise the results.



## STEP4: Taxonomy assignment 

### 16S taxonomy assignment 
The QIIME 2 plugin [feature-classifier](https://docs.qiime2.org/2023.9/plugins/available/feature-classifier/) supports taxonomic classification of features using a variety of methods, including:  
 1. **Naive Bayes**;  
 2. **vsearch**; 
 3. **BLAST+**.  

The `q2-feature-classifier` contains three different classification methods. **classify-consensus-blast** and **classify-consensus-vsearch** are both *alignment-based methods*, that find a consensus assignment across N top hits.  
These methods take reference database `FeatureData[Taxonomy]` and `FeatureData[Sequence]` files directly, and do not need to be pre-trained.

In our tutorial we are going to use the **q2-feature-classifier plugin** by using a pre-trained ***Naive Bayes classifier***.  
This classifier was trained on the *SILVA 138 NR99 collection*, where the sequences have been trimmed to only include 300 bases from the region of the 16S that was sequenced in this analysis (the V4 region, bound by the 515F/806R primer pair).  


Initially, we need to download the pre-computed classifier, and then assign taxonomy to the sequences in our `FeatureData[Sequence]` QIIME2 artifact.

**!!Again, this step is quite long, and we are not interested in waiting it to complete, so we have already classified our ASVs.** 
:stop_sign:  

```
# DO NOT RUN THIS COMMAND
wget \
  -O "silva-138-99-515-806-nb-classifier.qza" \
  "https://data.qiime2.org/2023.9/common/silva-138-99-515-806-nb-classifier.qza"

# DO NOT RUN THIS COMMAND
qiime feature-classifier classify-sklearn \
   --i-classifier silva-138-99-515-806-nb-classifier.qza \
   --i-reads rep-seqs_16S.qza \
   --o-classification taxonomy_16S_SKLEARN.qza
```

Copy the prepared output to your folder
:walking:  
```
cp ~/Shared_folder/taxonomy_16S_SKLEARN.qza  . 
```

Once the classification is done we can generate the barplot for data visualization:  
:walking:  
```
qiime metadata tabulate \
  --m-input-file taxonomy_16S_SKLEARN.qza \
  --o-visualization taxonomy_16S_SKLEARN.qzv
```

```
qiime metadata tabulate \
  --m-input-file taxonomy_16S_SKLEARN.qza  \
  --m-input-file rep-seqs_16S.qza  \
  --o-visualization annotated_rep-set_16S.qzv
```

```
qiime taxa barplot \
  --i-table table_16S_gt1k.qza \
  --i-taxonomy taxonomy_16S_SKLEARN.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization taxa-bar-plots_16S_SKLEARN.qzv
```

## STEP5: Generating a phylogenetic tree
While a phylogenetic tree of your microbiome may not be useful to interpret, we generate it because QIIME2 supports different phylogenetic diversity metrics including Faith’s Phylogenetic Diversity and weighted and unweighted UniFrac.

In addition to counts of features per sample (i.e., the data in the `FeatureTable[Frequency]` QIIME 2 artifact), these metrics require a rooted phylogenetic tree relating the features to one another. This information will be stored in a `Phylogeny[Rooted]` QIIME 2 artifact. To generate a phylogenetic tree we will use **align-to-tree-mafft-fasttree** pipeline from the q2-phylogeny plugin`.

* First, the pipeline uses the **mafft** program to perform a multiple sequence alignment of the sequences in our `FeatureData[Sequence]` to create a `FeatureData[AlignedSequence]` QIIME 2 artifact. 

* Next, the pipeline **masks** (or filters) the alignment to remove positions that are highly variable. These positions are generally considered to add noise to a resulting phylogenetic tree. 

* Following that, the pipeline applies **FastTree** to generate a phylogenetic tree from the masked alignment. The FastTree program creates an uprooted tree

* The final step in this section midpoint **rooting** is applied to place the root of the tree at the midpoint of the longest tip-to-tip distance in the unrooted tree.

:walking:  
```
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences rep-seqs_16S.qza \
  --o-alignment aligned-rep-seqs.qza \
  --o-masked-alignment masked-aligned-rep-seq.qza \
  --o-tree unrooted-tree_16S.qza \
  --o-rooted-tree rooted-tree_16S.qza
```

## STEP6: Analyzing Alpha and Beta diversities

First, lets look at alpha diversity as a function of sequencing depth, as a test of our sequencing run.  
:walking:  
```
qiime diversity alpha-rarefaction \
  --i-table table_16S_gt1k.qza \
  --i-phylogeny rooted-tree_16S.qza \
  --p-max-depth 8000 \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization alpha-rarefaction.qzv
```

QIIME 2’s diversity analyses are available through the [`q2-diversity plugin`](https://docs.qiime2.org/2023.9/plugins/available/diversity/), which supports computing alpha and beta diversity metrics, applying related statistical tests, and generating interactive visualizations. We’ll first apply the core-metrics-phylogenetic method, which rarefies a `FeatureTable[Frequency]` to a user-specified depth, computes several alpha and beta diversity metrics, and generates **Principle Coordinates Analysis (PCoA)** plots using Emperor for each of the beta diversity metrics. The metrics computed by default are:

**Alpha diversity**
* Shannon’s diversity index (a quantitative measure of community richness)
* Observed OTUs (a qualitative measure of community richness)
* Faith’s Phylogenetic Diversity (a qualitative measure of community richness that incorporates phylogenetic relationships between the features)
* Evenness (or Pielou’s Evenness; a measure of community evenness)

**Beta diversity**
* Jaccard distance (a qualitative measure of community dissimilarity)
* Bray-Curtis distance (a quantitative measure of community dissimilarity)
* unweighted UniFrac distance (a qualitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)
* weighted UniFrac distance (a quantitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)

An important parameter that needs to be provided to this script is **--p-sampling-depth**, which is the even sampling (i.e. rarefaction) depth.
:walking:  
```
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny rooted-tree_16S.qza \
  --i-table table_16S_gt1k.qza \
  --p-sampling-depth 1000 \
  --p-n-jobs-or-threads 1 \
  --m-metadata-file sample-metadata.tsv \
  --output-dir core-metrics-results_16S
````
From these outputs, a range of datasets and visualisations are generated that can be tested in the next steps, or explored visually. Let's look at the 'emperor' PCoA results.

## Time to test

We’ll first test for associations between **categorical** metadata columns and alpha diversity data. We’ll do that here for the Faith Phylogenetic Diversity (a measure of community richness) and evenness metrics.
:walking:  
```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results_16S/faith_pd_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization core-metrics-results_16S/faith-pd-group-significance_16S.qzv
```

```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results_16S/evenness_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization core-metrics-results_16S/evenness-group-significance.qzv
```

```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results_16S/shannon_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization core-metrics-results_16S/shannon-group-significance.qzv
```

Using **Pearson** and **Spearman** correlation it is possible to determine whether numeric sample metadata columns are correlated with
alpha diversity.
:walking:  
```
qiime diversity alpha-correlation \
  --i-alpha-diversity core-metrics-results_16S/shannon_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --p-method spearman \
  --o-visualization core-metrics-results_16S/shannon_correlation.qzv
```

```
qiime diversity alpha-correlation \
  --i-alpha-diversity core-metrics-results_16S/faith_pd_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --p-method spearman \
  --o-visualization core-metrics-results_16S/faith_pd_correlation.qzv
````


Next we’ll analyze sample composition in the context of categorical metadata using **PERMANOVA** (first described in [Anderson (2001)](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1442-9993.2001.01070.pp.x)) using the beta-group-significance command.  
The following commands will test whether distances between samples within a group, are more similar to each other, then they are compared to samples from the other groups. In this case we test only for the Transect_name and for the vegetation
:walking:  
```
qiime diversity beta-group-significance \
  --i-distance-matrix core-metrics-results_16S/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column transect-name \
  --o-visualization core-metrics-results_16S/unweighted-unifrac-tran-name-significance.qzv \
  --p-pairwise
```

```
qiime diversity beta-group-significance \
  --i-distance-matrix core-metrics-results_16S/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column vegetation \
  --o-visualization core-metrics-results_16S/unweighted-unifrac-subject-group-significance.qzv \
  --p-pairwise
```

# BONUS: Differential Abundance Analysis in QIIME2
Sometimes you are also interested in testing whether individual ASVs or taxa are more or less abundant in different sample groups.  
Microbiome data are challenging and conventional methods (i.e. t-test) are not appropriated for this task.  
Microbiome abundance data are inherently sparse (have a lot of zeros) and compositional (everything adds up to 1). 
***ANCOM (Analysis of Composition of Microbiomes)*** relies on a compositional aware approach allowing to identify differentially abundant features. Have a look to the [ANCOM paper](https://www.ncbi.nlm.nih.gov/pubmed/26028277).

First we are going to remove low abundant features in order to improve our ability in inferring features that are really differentially abundant.  
Next, we will retain feature observed in at least the 10% of our samples.  
:walking:  
```
qiime feature-table filter-features \
  --i-table table_16S_gt1k.qza \
  --p-min-frequency 5 \
  --p-min-samples 10 \
  --o-filtered-table filtered_table_16S.qza
```

ANCOM relies on a _log-transform_, so we need to add pseudocounts to our table. We are adding a *1* to each item of our table in order to perform the log-transform.  
:walking:  
```
qiime composition add-pseudocount \
  --i-table filtered_table_16S.qza \
  --o-composition-table filtered_table_16S_pc.qza
```

The ANCOM visualizations is a volcano plot showing the ANCOM W statistic to the CLR (center log transform) for the groups.  
The W statistic is the number of ANCOM sub-hypotheses that have passed for each individual taxon, indicating that the ratios of that taxon’s relative abundance to the relative abundances of W other taxa were detected to be significantly different (typically FDR-adjusted p < 0.05).  
Because differential abundance in ANCOM is based on the ratio between tests, it does not produce a traditional p-value.  
:walking:  
```
qiime composition ancom \
  --i-table filtered_table_16S_pc.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column transect-name \
  --o-visualization ancom_transect-name.qzv
```

Recently also [**ANCOM-BC**](https://docs.qiime2.org/2022.11/plugins/available/composition/ancombc/) has been implemented in QIIME.

[**Back to the program**](../README.md)  
