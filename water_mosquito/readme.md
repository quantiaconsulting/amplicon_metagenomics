## Exploring Changes in the Microbiota of <em>Aedes albopictus</em>: Comparison Among Breeding Site Water, Larvae, and Adults

==============
- [Abstract](#abstract)
- [Data Analysis](#data-analysis)
  - [Descriptive statistics](#descriptive-statistics)
  - [Visualise taxonomically](#visualise-taxonomically)
  - [Diversity processing and testing](#diversity)
- [Questions](#questions)

![](09805-scicon6-bees.jpg)

## Abstract
The mosquito body hosts highly diverse microbes, which influence different physiological traits of both larvae and adults. The composition of adult mosquito microbiota is tightly linked to that of larvae, which are aquatic and feed on organic detritus, algae and prokaryotic microorganisms present in their breeding sites. Unraveling the ecological features of larval habitats that shape the structure of bacterial communities and their interactions with the mosquito host is still a poorly investigated topic in the Asian tiger mosquito Aedes albopictus, a highly invasive species that is vector of numerous arboviruses, including Dengue, Chikungunya, and Zika viruses. In this study, we investigated the composition of the bacterial community present in the water from a natural larval breeding site in which we separately reared wild-collected larvae and hatched eggs of the Foshan reference laboratory strain. Using sequence analysis of bacterial 16S rRNA gene amplicons, we comparatively analyzed the microbiota of the larvae and that of adult mosquitoes, deriving information about the relative impact of the breeding site water on shaping mosquito microbiota. We observed a higher bacterial diversity in breeding site water than in larvae or adults, irrespective of the origin of the sample. Moreover, larvae displayed a significantly different and most diversified microbial community than newly emerged adults, which appeared to be dominated by Proteobacteria. The microbiota of breeding site water significantly increased its diversity over time, suggesting the presence of a dynamic interaction among bacterial communities, breeding sites and mosquito hosts. The analysis of Wolbachia prevalence in adults from Foshan and five additional strains with different geographic origins confirmed the described pattern of dual wAlbA and wAlbB strain infection. However, differences in Wolbachia prevalence were detected, with one strain from La Reunion Island showing up to 18% uninfected individuals. These findings contribute in further understanding the dynamic interactions between the ecology of larval habitats and the structure of host microbiota, as well as providing additional information relative to the patterns of Wolbachia infection.

https://www.frontiersin.org/articles/10.3389/fmicb.2021.624170/full

## Data Analysis
Water from Ae. albopictus larval breeding sites was collected in a private garden in Crema, Italy (45°21′51.2′′N 9°40′57.7′′E), which was an accessible site characterized by high population density, at the end of August 2018, when climate conditions are optimal for mosquito development. Domestic collection of water, eggs, and larvae originated from two plastic buckets of a maximum volume of 500 ml that were placed in the same garden next to ornamental plants. Environmental water collected in these buckets derived from dew and rain and was monitored for its level and clarity every day for 4 days. No sedimentary layer was observed. Water and eggs/larvae were then collected in sterile 50-ml Falcon tubes and transferred to the insectary of the University of Pavia, Italy.


Illumina MiSeq 16S libraries were generated following the standard protocol “16S Metagenomic Sequencing Library Preparation, Part # 15044223 Rev B.” Amplicon PCR was performed using PCR primers 341F (5′-CCTACGGGNGGCWGCAG-3′) and 805R (5′-GACTACHVGGGTATCTAATCC-3′) with Illumina library adaptors.

Illumina MiSeq 2 × 300 paired-end chemistry (MiSeq Reagent Kit v3).

**One of the main goals of this tutorial is to understand what the limitations of this experiment are**


### Let's start

The following steps have already been performed for you to save time:
- import fastq files
- denoise using dada2
- assign taxonomy using SKLEARN and the SILVA database
- generate a rooted tree

Therefore you can copy these files to your local folder and begin your analysis
```
mkdir water_mosquito
cd water_mosquito
cp /home/Share/water_mosquito/* .
```
You should now have these files to use for your analysis:
```
table_water_mosquito.qza
taxonomy_water_mosquito.qza
map_water_mosquito.tsv
rooted_tree_water_mosquito
rep_set_water_mosquito
```

## Descriptive statistics
Generate the "summarize-table.qzv" to explore the data.
```
qiime feature-table summarize \
  --i-table table_water_mosquito.qza \
  --o-visualization table_water_mosquito.qzv \
  --m-sample-metadata-file map_water_mosquito.tsv
```



### Filter site CR1 and Cr2

```
qiime feature-table filter-samples \
  --i-table xxx \
  --m-metadata-file xxx \
  --p-where "type_water IN ('CR1', 'CR2')" \
  --o-filtered-table table_h2o_host_CR12.qza
```
Generate the new "summarize-table.qzv" so that you get the information you need to fill the sentence below:

"The microbiota of breeding site water (W), larval (L), and adult (A) Ae. albopictus samples was examined by sequencing of the bacterial 16S rRNA gene. Sequences of a total of ... libraries (i.e., ... water samples; ... CR and ... FO larval samples; ... CR and ... FO adults, respectively; two DNA extraction negative controls)"

## Visualise taxonomically 

### Create taxonomic bar plot visuals
Use this to get a view of the taxonomic make up of your range of samples
```
qiime taxa barplot \
  --i-table xxx \
  --i-taxonomy xxx \
  --m-metadata-file xxx \
  --o-visualization taxa_bar_plots_water.qzv
```






## Diversity

Let's perform rarefaction curves to decide the most appropriate rarefaction depth.    

You have to decide the sampling depht. What is a good value to set sampling depth for?
Please discuss your choise

```
qiime diversity alpha-rarefaction \
    --i-table xxx \
    --i-phylogeny xxx \
    --p-max-depth xxx \
    --m-metadata-file xxxx \
    --o-visualization alpha-rarefaction.qzv
```

### Diversity calculations
First off lets run the core diversity metrics, then you can test the outputs!
 ```
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny xxx \
  --i-table xxx \
  --p-sampling-depth xxxx \
  --p-n-jobs-or-threads 2 \
  --m-metadata-file xxx \
  --output-dir core-metrics-results
```
Now run some tests on these outputs using commands such as:
```
qiime diversity alpha-group-significance
qiime diversity alpha-correlation
qiime diversity beta-group-significance
qiime diversity beta-correlation
```



Use the diversity calculations and taxonomic plots to answer questions about your data. 

- Among which groups is the greatest bacterial diversity observed?
- Different metrics lead to different considerations, what's your idea?
- Are there any significanlty associated ASVs? (using ANCOM to test). Compare the results obtained in the paper 


