Human microbiome in gastric cancer
==============
- [Abstract](#abstract)
- [Data Analysis](#data-analyis)


## Abstract
Gastric cancer (GC) is the fifth most prevalent cancer worldwide and the third leading cause of global cancer mortality.  
Recently, the involvement of the microbiota in gastric carcinogenesis has been described.  
To deepen this aspect [Ravegnini et al. IJMS 2021](https://www.mdpi.com/1422-0067/21/24/9735) compared microbiota composition in signet-ring cell carcinoma (SRCC) and adenocarcinoma (ADC).  

## Data Analyis
We are going to show the analysis and the results of a subset of data used in the paper. In particular, we will analyse:  
- 5 SRCCC samples;  
- 4 ADC samples;  
The V3-V4 amplicon was amplified and sequenced.     
**Considering the available time, data were pre-computed. So start to copy the folder:**
```
cd

cp -r ~/Share/IJMS_data .
```

Following the main steps of the applied workflow.  
**All the described steps have been already performed so just follow the explanation.***  

### Data import and quality check
Initially, we need to import raw data in a QIIME2 artifacts through a manifest file.
Create a directory for our data
```
cd

mkdir IJMS_data && mv IJMS_data
```
Let's prepare the manifest file:
```
echo sample-id,forward-absolute-filepath,reverse-absolute-filepath > manifest_file.tsv
echo 211446F203610,$HOME/Share/IJMS_data/IJMS_input_data/211446F203610_S155_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/211446F203610_S155_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 211454F203618,$HOME/Share/IJMS_data/IJMS_input_data/211454F203618_S163_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/211454F203618_S163_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 211456F203620,$HOME/Share/IJMS_data/IJMS_input_data/211456F203620_S165_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/211456F203620_S165_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 211460F203624,$HOME/Share/IJMS_data/IJMS_input_data/211460F203624_S169_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/211460F203624_S169_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 214981F203626,$HOME/Share/IJMS_data/IJMS_input_data/214981F203626_S2_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/214981F203626_S2_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 214991F203636,$HOME/Share/IJMS_data/IJMS_input_data/214991F203636_S12_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/214991F203636_S12_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 214993F203638,$HOME/Share/IJMS_data/IJMS_input_data/214993F203638_S14_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/214993F203638_S14_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 214997F203642,$HOME/Share/IJMS_data/IJMS_input_data/214997F203642_S18_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/214997F203642_S18_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 215001F203646,$HOME/Share/IJMS_data/IJMS_input_data/215001F203646_S22_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/215001F203646_S22_L001_R2_001.fastq.gz >> manifest_file.tsv
echo 215003F203648,$HOME/Share/IJMS_data/IJMS_input_data/215003F203648_S24_L001_R1_001.fastq.gz,$HOME/Share/IJMS_data/IJMS_input_data/215003F203648_S24_L001_R2_001.fastq.gz >> manifest_file.tsv
```
Finally, import the data and visualize it:
```
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path manifest_file.tsv \
  --output-path pe-demux.qza \
  --input-format PairedEndFastqManifestPhred33V2

qiime demux summarize \
    --i-data  pe-demux.qza \
    --o-visualization pe-demux.qzv
```

### Primer trimming
Following we need to trim the primer sequences:
```
qiime cutadapt trim-paired \
    --i-demultiplexed-sequences pe-demux.qza \
    --p-front-f CCTACGGGNGGCWGCAG \
    --p-front-r GACTACHVGGGTATCTAATCC \
    --o-trimmed-sequences demux-trimmed.qza \
    --p-discard-untrimmed \
    --p-cores 10

qiime demux summarize --i-data demux-trimmed.qza --o-visualization demux-trimmed.qzv
 --o-visualization pe-demux.qzv
```
### Denoising
Considering the quality of our data and the expected length of our amplicon (~500nt), we trim  30 and 40 nt from our forward and reverse reads, respectively.
Moreover we increase the EE to 4 and 5.
```
qiime dada2 denoise-paired \
  --i-demultiplexed-seqs demux-trimmed.qza \
  --p-trunc-len-f 270 \
  --p-trunc-len-r 260 \
  --p-max-ee-f 4 \
  --p-max-ee-r 5 \
  --p-n-threads 2 \
  --o-table table_16S.qza \
  --o-representative-sequences rep-seqs_16S.qza \
  --o-denoising-stats denoising-stats_16S.qza
```

```
qiime feature-table tabulate-seqs \
--i-data rep-seqs_16S.qza \
--o-visualization rep-seqs.qzv

qiime metadata tabulate \
    --m-input-file denoising-stats_16S.qza \
    --o-visualization denoising-stats_16S.qzv

qiime feature-table summarize \
   --i-table table_16S.qza \
   --o-visualization table_16S.qzv \
   --m-sample-metadata-file IJMS_input_data/ijms_metadata.tsv 
```

## Taxonomic Classification  
Initially we need to extract the 16S regions corresponding to the amplified amplicon and then train the classifier on it.  
The SILVA release was downloaded directly from [QIIME2 data resources page](https://docs.qiime2.org/2021.8/data-resources/).   
```
qiime feature-classifier extract-reads \
  --i-sequences silva-138-99-seqs.qza \
  --p-f-primer CCTACGGGNGGCWGCAG \
  --p-r-primer GACTACHVGGGTATCTAATCC \
  --o-reads v3v4.ref-seqs.qza

qiime feature-classifier fit-classifier-naive-bayes  \
    --i-reference-reads v3v4.ref-seqs.qza \
    --i-reference-taxonomy silva-138-99-tax.qza \
    --o-classifier v3v4.SILVA_138_NR_99_classifier.qza
```
Now we are ready to classify
```
qiime feature-classifier classify-sklearn \
  --i-classifier v3v4.SILVA_138_NR_99_classifier.qza \
  --i-reads rep-seqs_16S.qza \
  --o-classification taxonomy_16S_SKLEARN.qza 

qiime metadata tabulate \
    --m-input-file taxonomy_16S_SKLEARN.qza \
    --o-visualization taxonomy_16S_SKLEARN.qzv

qiime taxa barplot \
    --i-table table_16S.qza \
    --i-taxonomy taxonomy_16S_SKLEARN.qza \
    --m-metadata-file IJMS_input_data/ijms_metadata.tsv  \
    --o-visualization taxa-bar-plots_16S_SKLEARN.qzv
```

## Filter out mitochondrion and chloroplast
Cause we notice that there are 16S labelled as mitochondrion and chloroplast we need to remove those ASVs from subsequent analysis.  
```
qiime taxa filter-seqs \
  --i-sequences rep-seqs_16S.qza \
  --i-taxonomy taxonomy_16S_SKLEARN.qza \
  --p-exclude mitochondria,chloroplast \
  --o-filtered-sequences rep-seqs-mitochondria-no-chloroplast.qza
```

### Phylogenetic tree inference
Now we are ready to generate a rooted tree and rarefaction curves.  
```
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences rep-seqs-mitochondria-no-chloroplast.qza \
  --o-alignment aligned-rep-seqs_16S.qza \
  --o-masked-alignment masked-aligned-rep-seqs_16S.qza \
  --o-tree unrooted-tree_16S.qza \
  --o-rooted-tree rooted-tree_16S.qza
```

## Alpha e Beta Diversity
Time to infer Alpha and Beta diversity and run some tests.  
```
qiime diversity alpha-rarefaction \
    --i-table table-no-mitochondria-no-chloroplast.qza \
    --i-phylogeny rooted-tree_16S.qza \
    --p-max-depth 50000 \
    --m-metadata-file IJMS_input_data/ijms_metadata.tsv \
    --o-visualization alpha-rarefaction.qzv

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny rooted-tree_16S.qza \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --p-sampling-depth 15000 \
  --p-n-jobs-or-threads 2 \
  --m-metadata-file IJMS_input_data/ijms_metadata.tsv \
  --output-dir core-metrics-results_16S


qiime diversity alpha-group-significance \
      --i-alpha-diversity core-metrics-results_16S/shannon_vector.qza \
      --m-metadata-file IJMS_input_data/ijms_metadata.tsv \
      --o-visualization core-metrics-results_16S/shannon-Type-significance_16S.qzv
    
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results_16S/faith_pd_vector.qza \
  --m-metadata-file IJMS_input_data/ijms_metadata.tsv \
  --o-visualization core-metrics-results_16S/faith_pd-Type-significance.qzv

qiime diversity beta-group-significance \
      --i-distance-matrix core-metrics-results_16S/weighted_unifrac_distance_matrix.qza \
      --m-metadata-file IJMS_input_data/ijms_metadata.tsv \
      --m-metadata-column type \
      --o-visualization core-metrics-results_16S/weighted-unifrac-Condition-significance.qzv \
      --p-pairwise
```

Now just download the result and discuss about.  

[Back to the top](../README.md) 