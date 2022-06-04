Some reasonable actions to address the assigned tasks
=========
- [Working with Alpha Diversity data](#working-with-alpha-diversity-data)
- [Working with Beta Diversity data](#working-with-beta-diversity-data)
- [Differential Abundant Analysis](#differential-abundance-analysis)
  - [DA on ASVs](#da-on-asvs)
  - [Da on Genera](#da-in-genera)

## Working with Alpha Diversity data
First, we try to verify whether there is any relevant difference in alpha diversity distribution
among ADC and SRCC.  

We begin with the _Shannon Index_:  
```
qiime diversity alpha-group-significance \
      --i-alpha-diversity core-metrics-results_16S/shannon_vector.qza \
      --m-metadata-file ijms_metadata.tsv \
      --o-visualization tasks/shannon-Type-significance_16S.qzv
```
Next we investigate the _Faith Phylogenetic Diversity Index_:
```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results_16S/faith_pd_vector.qza \
  --m-metadata-file ijms_metadata.tsv \
  --o-visualization tasks/faith_pd-Type-significance.qzv
```

## Working with Beta Diversity data
Then we could also consider if the whole community structure changes among SRCC and ADC.  
I'm going to evaluate both a _quantitave_ and a _qualitative_ metric (_weigthed_ and _unweighted UniFrac_) by using **PERMANOVA**.  
Let's begin to apply PERMANOVA to the distances among samples measured by using the weighted UniFrac metric.  
```
qiime diversity beta-group-significance \
      --i-distance-matrix core-metrics-results_16S/weighted_unifrac_distance_matrix.qza \
      --m-metadata-file ijms_metadata.tsv \
      --m-metadata-column type \
      --o-visualization tasks/weighted-unifrac-Condition-significance.qzv \
      --p-pairwise
```
Let's begin to apply PERMANOVA to the distances among samples measured by using the unweighted UniFrac metric.  
```
qiime diversity beta-group-significance \
      --i-distance-matrix core-metrics-results_16S/unweighted_unifrac_distance_matrix.qza \
      --m-metadata-file ijms_metadata.tsv \
      --m-metadata-column type \
      --o-visualization tasks/unweighted-unifrac-Condition-significance.qzv \
      --p-pairwise
```

## Differential abundance analysis
We are going to apply ANCOM to ASVs and genera counts.  

### DA on ASVs
Initially, we filter-out low abundant and low prevalent ASVs:  
```
qiime feature-table filter-features \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --p-min-frequency 5 \
  --p-min-samples 5 \
  --o-filtered-table tasks/filtered_table_16S.qza
```

ANCOM relies on a _log-transform_ so we need to add pseudocounts to our table.  
We are adding a *1* to each item of our table in order to perform the log-transform.  

```
qiime composition add-pseudocount \
  --i-table tasks/filtered_table_16S.qza \
  --o-composition-table tasks/filtered_table_16S_pc.qza
```

We are ready to apply ANCOM:

```
qiime composition ancom \
  --i-table tasks/filtered_table_16S_pc.qza \
  --m-metadata-file ijms_metadata.tsv \
  --m-metadata-column type \
  --o-visualization tasks/asv-ancom.qzv
```


### DA in Genera
Initially, we need to aggregate counts at the Genus level.  
```
qiime taxa collapse \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --i-taxonomy taxonomy_16S_SKLEARN.qza \
  --p-level 6 \
  --o-collapsed-table tasks/genera.qza
```
Now we can filter-out low abundant and low prevalent genera, add pseudocounts and apply ANCOM:  
```
qiime feature-table filter-features \
  --i-table tasks/genera.qza \
  --p-min-frequency 5 \
  --p-min-samples 5 \
  --o-filtered-table tasks/filtered_genera.qza
```

```
qiime composition add-pseudocount \
  --i-table tasks/filtered_genera.qza \
  --o-composition-table tasks/filtered_genera_pc.qza
```

```
qiime composition ancom \
  --i-table tasks/filtered_genera_pc.qza \
  --m-metadata-file ijms_metadata.tsv \
  --m-metadata-column type \
  --o-visualization tasks/genera-ancom.qzv
```


Now just download the results and discuss it.  

[Back to the top](../README.md) 