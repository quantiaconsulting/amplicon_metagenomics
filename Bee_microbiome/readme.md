Differences in honey bee bacterial diversity and composition in agricultural and pristine environments
==============
- [Abstract](#abstract)
- [Data Analysis](#data-analysis)
  - [Visualise taxonomically](#visualise-taxonomically)
  - [Filter out chloroplast and mitochondria](#filter-out-chloroplast-and-mitochondria)
  - [Subsetting your data](#Create-a-subset-of-your-data)
  - [Diversity processing and testing](#diversity)
- [Questions to answer](#now-your-go)

![](09805-scicon6-bees.jpg)

## Abstract
Agrochemicals and biocides are suspected to cause a dysbiosis of honey bee microbiota,decreasing colonies ability to respond to the environment. As a first step to investigate agriculture andbeekeeping impact, hives bacteriomes from an anthropized environment (Agri-env) were compared tothat of pristine’s (Prist-env). 16S rRNA sequencing evidenced differences in richness and compositionbetween sample types (Gut (G), Brood (B), Bee-bread (collected pollen) (BB)) and environments. 
Muñoz-Colmenero et. al, 2020
https://link.springer.com/article/10.1007/s13592-020-00779-w

## Data Analysis
We have a collection of samples to the following conditions, with 3x replicates per hive (some samples have failed sequencing and been filtered out). Note that in addition to the environment (clean vs agricultural) and sample types (Gut, Brood, Bee-Bread) there is also Hive internal air (not mentioned in the paper) and additional metadata regarding samples with detected infections. Think about how that could affect your results!

### Input data
Number of hives included

|    | Prist-env | Agri-env | Total |
|----|:---------:|:--------:|:-----:|
|Bees (gut)| 8 | 9 | 17|
Brood | 3 |	5 |	9 |
Bee-bread “with blocking”	| 6 | 5 | 11 |
Bee-bread “without blocking” | 6 | 5 | 11 |

The following steps have already been performed for you to save time:
- import fastq files
- denoise using dada2
- assign taxonomy using SKLEARN and the SILVA database
- generate a rooted tree

Therefore you can copy these files to your local folder and begin your analysis
```
mkdir BEE-analysis
cd BEE-analysis
cp /home/Share/BEE/* .
```
You should now have these files to use for your analysis:
```
table99.qza
taxonomy99.qza
rep-seqs99.qza
rooted-tree.qza
BEE-metadata.tsv
```

## Visualise taxonomically 

### Create taxonomic bar plot visuals
Use this to get a view of the taxonomic make up of your range of samples
```
qiime taxa barplot \
  --i-table table99.qza \
  --i-taxonomy taxonomy99.qza \
  --m-metadata-file BEE-metadata.txt \
  --o-visualization taxa_bar_plots.qzv
```
### Filter out chloroplast and mitochondria
Because we notice that there are 16S labelled as chloroplast we need to remove those ASVs from subsequent analysis.  
```
qiime taxa filter-seqs \
  --i-sequences rep-seqs99.qza \
  --i-taxonomy taxonomy99.qza \
  --p-exclude mitochondria,chloroplast \
  --o-filtered-sequences rep-seqs-mitochondria-no-chloroplast.qza
```

```
qiime taxa filter-table \
  --i-table table99.qza \
  --i-taxonomy  taxonomy99.qza \
  --p-exclude mitochondria,chloroplast \
  --o-filtered-table table-no-mitochondria-no-chloroplast.qza
```

### Create a subset of your data
If you are interested in just one aspect of your data you can select just those samples
```
qiime feature-table filter-samples \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --m-metadata-file BEE-metadata.txt \
  --p-where "[env_matter]='Gut'" \
  --o-filtered-table gut-only-table.qza
```
You could re-make your taxa barplots after this if you wanted.

```
qiime taxa barplot \
  --i-table gut-only-table.qza \
  --i-taxonomy taxonomy99.qza \
  --m-metadata-file BEE-metadata.txt \
  --o-visualization gut-only_taxa_bar_plots.qzv
```
Q -  What is causing the large variation within the Agricultural gut samples?

## Diversity
Lets use all our samples for now
### Rarefaction
Let's perform rarefaction curves to decide the most appropriate rarefaction depth.    
```
qiime diversity alpha-rarefaction \
    --i-table table-no-mitochondria-no-chloroplast.qza \
    --i-phylogeny rooted-tree.qza \
    --p-max-depth 50000 \
    --m-metadata-file BEE-metadata.txt \
    --o-visualization alpha-rarefaction.qzv
```
Q - What is a good value to set sampling depth for? Would it be different based on which conditions you're testing?

### Diversity calculations
First off lets run the core diversity metrics, then you can test the outputs!
 ```
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny rooted-tree.qza \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --p-sampling-depth 30000 \
  --p-n-jobs-or-threads 2 \
  --m-metadata-file BEE-metadata.txt \
  --output-dir core-metrics-results
```
Now run some tests on these outputs using commands such as:
```
qiime diversity alpha-group-significance
qiime diversity alpha-correlation
qiime diversity beta-group-significance
qiime diversity beta-correlation
```
You could run these tests on just a subset (i.e. just bee guts with no problems)

## Now your go!

Use the diversity calculations and taxonomic plots to answer questions about your data. 

*Choose either gut, brood, or Bee-Bread (collected pollen)*


- Is there a significant difference in alpha diversity between Pristine and Agricultural environments?
- Is there a difference in community structure using unifrac distance? Is it significant?
- Are there any significantly associated ASVs? (using ANCOM to test)
- What effect are the samples labelled "problem" causing? Should they be removed?


