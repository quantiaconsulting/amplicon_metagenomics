Some reasonable actions to address the assigned tasks
=========
- [Working with Alpha Diversity data](#working-with-alpha-diversity-data)
- [Working with Beta Diversity data](#working-with-beta-diversity-data)
- [Differential Abundant Analysis in QIIME2](#differential-abundance-analysis-in-qiime2)
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
I'm going to evaluate both a _quantitative_ and a _qualitative_ metric (_weighted_ and _unweighted UniFrac_) by using **PERMANOVA**.  
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

## Differential abundance analysis in QIIME2
We are going to apply ANCOM-BC to ASVs and genera counts.  

### DA on ASVs
Initially, we filter-out low abundant and low prevalent ASVs:  
```
qiime feature-table filter-features \
  --i-table table-no-mitochondria-no-chloroplast.qza \
  --p-min-frequency 5 \
  --p-min-samples 5 \
  --o-filtered-table tasks/filtered_table_16S.qza
```
We are ready to apply ANCOM:

```
qiime composition ancombc \
  --i-table tasks/filtered_table_16S.qza \
  --m-metadata-file ijms_metadata.tsv \
  --p-formula type \
  --o-differentials tasks/asv-ancom.qza
```
Finally, we generate a visualization file:
```
qiime composition tabulate \
  --i-data tasks/asv-ancom.qza \
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
Now we can filter-out low abundant and low prevalent genera and apply ANCOM-BC:  
```
qiime feature-table filter-features \
  --i-table tasks/genera.qza \
  --p-min-frequency 6 \
  --p-min-samples 6 \
  --o-filtered-table tasks/filtered_genera.qza
```

```
qiime composition ancombc \
  --i-table tasks/filtered_genera.qza \
  --m-metadata-file ijms_metadata.tsv \
  --p-formula type \
  --o-differentials tasks/genera-ancom.qza
```
Finally, we generate a visualization file:
```
qiime composition tabulate \
  --i-data tasks/genera-ancom.qza \
  --o-visualization tasks/genera-ancom.qzv
```

Now just download the results and discuss it.  

[Back to the top](../README.md) 