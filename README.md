<img src="main_data_dir/image.jpg" width="940" alt="None">  

**METABARCODING OF MICROBIAL COMMUNITIES**  
=====

## ***Rationale***
This course will provide a thorough introduction to the application of metabarcoding techniques in microbial ecology.  
The topics covered by the course range from *bioinformatic* processing of next-generation sequencing data to the most important approaches in multivariate statistics. Using a combination of theoretical lectures and hands-on exercises, the participants will learn the most important computational steps of a metabarcoding study from the processing of raw sequencing reads down to the final statistical evaluations. After completing the course, the participants should be able to understand the potential and limitations of metabarcoding techniques as well as to process their own datasets to answer the questions under investigation.  
__FORMAT__  
This course is designed for researchers and students with strong interests in applying novel high-throughput DNA sequencing technologies to answer questions in the area of community ecology and biodiversity.  
The course will mainly focus on the analysis of phylogenetic markers to study bacterial, archaeal and fungal assemblages in the environment, but the theoretical concepts and computational procedures can be equally applied to any taxonomic group or gene of interest.  
__ASSUMED BACKGROUND__  
The participants should have some basic background in biology and understand the central role of DNA for biodiversity studies. No programming or scripting expertise is required and some basic introduction to UNIX-based command line applications will be provided on the first day. However, some basic experience with using command line and/or R is clearly an advantage as not all the basics can be thoroughly covered in that short amount of time.  
All the hands-on exercises will be carried out using [**QIIME2**](https://qiime2.org/) platform . No previous knowledge of computer science is required but a basic knowledge of “bash” would allow to focus more on the microbial analysis.  
__LEARNING OUTCOMES__  
1) Understanding the concept, potential and limitation of microbial metabarcoding techniques.
2) Learning how to process raw sequencing reads to obtain meaningful information.
3) Obtaining experience on how to statistically evaluate and visualize your data.
4) Being able to make informed decisions on best practices for your own data.  
  
:exclamation: :exclamation: :exclamation: **All the papers discussed during the course are available in this [Google Drive Folder](https://drive.google.com/drive/folders/1pld02nU8v6APTbysmT_iQdGw8tKToByr?usp=share_link)** :exclamation: :exclamation: :exclamation:    
  

## Program
[Physalia Welcome](https://drive.google.com/open?id=1zAqld5-NcofYez4QYsGGvX0ZtJGphRNQHCBVwUgYNVE)
[Course participants introduction]
## Day 1
- [X] [Introduction to the course idea and Instructors](Welcome.pptx):
    - [Daniel Antony Pass](https://scholar.google.com/citations?user=XQml0DQAAAAJ&hl=en)  
    - [Anna Sandionigi](https://scholar.google.com/citations?hl=it&user=DLDuk_EAAAAJ)  
    - [Bruno Fosso](https://scholar.google.com/citations?user=TBeT9pIAAAAJ&hl=it)  
- [X] [**Brief introduction** about __Metagenomics__ concepts. *What are we going to talk about?*](https://docs.google.com/presentation/d/1Pei27F-JkJUiJXL7nCe5WaWffws7EHWk5EA3AfyrV-Q/edit?usp=share_link)
- [X] [**Experimental design part 1**  - Presentation](https://docs.google.com/presentation/d/1BGdfq3lH9avWzLAmXq6RMiOr_F5GEy9i9gyphj6JaYk/edit?usp=sharing)
- [X] [Laptop setting: - connecting to the server in Posit (formerly known as RStudio)](https://docs.google.com/presentation/d/1yA5ytQFu-npecFNrh3KDEk6IzHjG4KLotiGIXSruMuo/edit?usp=share_link)  
- [X] [Introduction to the **BASH** shell](unix_short_tutorial/Readme.md)

## Day 2
- [X] [Few concepts about Negative and Positive controls](unix_short_tutorial/controls.md)
- [X] [Introduction to the **FASTQ** format and QIIME objects](https://docs.google.com/presentation/d/1RowyRGCLqAgt6Oxa_h3c33r4SI9reZlq6ZheRv-HAks/edit?usp=share_link)
- [X] [Atacama Soil Experiment and Loading data](16S_ITS_tutorial/readme.md) 
- [X] [**Key concept: OTU or ASV**;](https://docs.google.com/presentation/d/1XHQGInyWt9SGmyH6C4UA2-flloR3vQghIzJ2MDAKsqY/edit?usp=sharing)
  - Different Sequencing Technologies and different denoising approaches
- [X] [Lets review](unix_short_tutorial/Readme.md)
- [X] [Denoising and **DADA2**](16S_ITS_tutorial/readme.md#step2-quality-controlling-sequences-and-building-feature-table-and-feature-data);
  - DADA2 - in Qiime, in R

## Day 3
- [X] [Taxonomic assignment](https://drive.google.com/open?id=1oHTCBiJ1HoHAREZIN2NVSHnC63QphDUJr_cPbgqgDs4)
- [X] [Taxonomy assignment:**classify-sklearn**](16S_ITS_tutorial/readme.md#step3-summarizing-feature-table-and-feature-data)
- [ ] [Diversity Analysis](https://docs.google.com/presentation/d/1wvxFCYMiQW-RH669eK1l6EcmcBqd0q7l2oF4do7okI8/edit?usp=share_link)
- [ ] [Running Alpha and Beta Diversity in QIIME2](16S_ITS_tutorial/readme.md#step6-analyzing-alpha-and-beta-diversities)

## Day 4
- [ ] [ITS sequence analysis](ITS/ITS_readme.md)
- [ ] [Experimental Design part 2 and short introduction to R](https://docs.google.com/presentation/d/1ybw75VKyMK9vJ_yy2SpFYbn8SZMJ7_6yf-BC0gLJ5vg/edit?usp=sharing)
- [ ] [Lecture  - Multivariate Analysis of Ecological Communities](https://docs.google.com/presentation/d/1SEXLnsAk71ghWJFBjvnSL9-JIU5kHyYi/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true)  
  - [ ] Traits of Alpha and Beta Diversity (richness, evenness, dispersion)  
  - [ ] Multivariate Tests for differences in microbial community composition  
- [ ] [Lab  - Multivariate Statistics](https://glcdn.githack.com/bfosso/physalia_metabarcoding_oct2021/raw/main/Day4_5_material/Physalia-Metabarcoding-Course-Oct21.html)  
  - [ ] Data import & preparation (normalisations, transformations, metadata)  
  - [ ] Multivariate tests for differences in community composition (PERMANOVA, PERMDISP)    
- [ ] [Lecture - Differential Abundance Analysis of Microbial Communities](https://docs.google.com/presentation/d/1Z2F2_goIAuuKXQQ7ocClOgq8x6tbpClW/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true) 
  - [ ] Lab: Differential Abundance Analysis with DESeq2

## Day 5
- [ ] [Some additional processing Tips :volcano: ](DataImport_and_Tax_management/readme.md)
  - [ ] Alternative loading methods in Qiime
  - [ ] Further, Understanding and Assessing your raw data
- [ ] :arrow_right: _It's Case studies Time!!!_
  - [ ] [Human microbiome data](human_cancer/readme.md)
  - [ ] [Zoological(Bees bees bees!)](Bee_microbiome/readme.md)
  - [ ] [Water-mosquito](water_mosquito/readme.md)
- [ ] [Record your Case Study findings](https://drive.google.com/drive/folders/1jwEfH0Urm_TjR2lVEO03L1EStmCSWyoC?usp=sharing)
- [ ] Beer Time :beers: :beers: :beers:
