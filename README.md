<img src="main_data_dir/image.jpg" width="940" alt="None">  

**METABARCODING OF MICROBIAL COMMUNITIES**  
=====

## ***Rationale***
This course will provide a thorough introduction to the application of metabarcoding techniques in microbial ecology.  
The topics covered by the course range from *bioinformatic* processing of next-generation sequencing data to the most important approaches in multivariate statistics. Using a combination of theoretical lectures and hands-on exercises, the participants will learn the most critical computational steps of a metabarcoding study from the processing of raw sequencing reads down to the final statistical evaluations. After completing the course, the participants should be able to understand the potential and limitations of metabarcoding techniques as well as to process their own datasets to answer the questions under investigation.  
__FORMAT__  
This course is designed for researchers and students with strong interests in applying novel high-throughput DNA sequencing technologies to answer questions in the area of community ecology and biodiversity.  
The course will mainly focus on the analysis of phylogenetic markers to study bacterial, archaeal and fungal assemblages in the environment, but the theoretical concepts and computational procedures can be equally applied to any taxonomic group or gene of interest. 

__ASSUMED BACKGROUND__  
The participants should have some basic background in biology and understand the central role of DNA for biodiversity studies. No programming or scripting expertise is required but completing the preparation materials to be comfortable with linux basics (i.e. making directories & copying files) is important. 

!! **See the [Unix short tutorial](unix_short_tutorial/Readme.md) if you need a reminder/refresher.**

All the hands-on exercises will be carried out using [**QIIME2**](https://qiime2.org/) platform . No previous knowledge of computer science is required but a basic knowledge of “bash” would allow to focus more on the microbial analysis.
__LEARNING OUTCOMES__  
1) Understanding the concept, potential and limitation of microbial metabarcoding techniques.
2) Learning how to process raw sequencing reads to obtain meaningful information.
3) Obtaining experience on how to statistically evaluate and visualize your data.
4) Being able to make informed decisions on best practices for your own data.  
  
:exclamation: :exclamation: :exclamation: **All the papers discussed during the course are available in this [Google Drive Folder](https://drive.google.com/drive/u/1/folders/1-xCnyNCXgHnnArDjwU1jx3o2PmLV_a3V)** :exclamation: :exclamation: :exclamation:
  

## Program
[Program day by day](https://docs.google.com/presentation/d/1kzrlgopqXGkwVBoc_og4AOpFx8ULoJFL/edit#slide=id.p1)

## Day 1
- [X] [The Instructors]():
    - [Javier del Campo](https://scholar.google.it/citations?user=Mty5iQYAAAAJ&hl=en&oi=ao)  
    - [Anna Sandionigi](https://scholar.google.com/citations?hl=it&user=DLDuk_EAAAAJ)  
    - [Luigimaria Borruso](https://scholar.google.it/citations?user=ifNJPJEAAAAJ&hl=it)  
- [X] Presentation - [**Brief introduction** about __Metagenomics__ concepts. *What are we going to learn?*](https://docs.google.com/presentation/d/1sv9m6DGiWOAWHNO0CHFBXDUsSUOhb20h/edit?usp=drive_link&ouid=114200662110058699819&rtpof=true&sd=true)
- [X] Presentation-  [**Experimental design part 1** ](https://docs.google.com/presentation/d/1H6ATnhWfvrl5IrjsF68llsJH_uKH4AHl/edit?usp=drive_link&ouid=114200662110058699819&rtpof=true&sd=true)
- [X] Presentation-  [**Experimental design part 1 - Computer set up** ](https://docs.google.com/presentation/d/1H6ATnhWfvrl5IrjsF68llsJH_uKH4AHl/edit#slide=id.g32c66d1f86a_0_0)
- [X] Practical - [Connecting to the server with Rstudio](https://docs.google.com/presentation/d/1H6ATnhWfvrl5IrjsF68llsJH_uKH4AHl/edit#slide=id.g32c704bc1a0_0_7)  
- [X] Practical - [Connecting to the server SSH](https://drive.google.com/file/d/18l2-7lqNTg5cwf_KJQWOkKWR-C3xYQkn/view?usp=drive_link)
- [X] Presentation - [Introduction to the **FASTQ** format](https://docs.google.com/presentation/d/1wfZsurKewWp0ToKb5PAfUTWHEh1bLBHA/edit?usp=drive_link&ouid=114200662110058699819&rtpof=true&sd=true)
- [X] Practical - [First hands on data (& linux review)](main_data_dir/Session1_Fastq_linux.md)


## Day 2
- [X] Practical - [Let's begin! Atacama Soil Experiment and Loading data in Qiime](16S_ITS_tutorial/readme.md) 
- [X] Presentation - [Concepts on Negative and Positive controls](main_data_dir/controls.md)
- [X] Presentation - [**Key concept: OTU or ASV**;](https://docs.google.com/presentation/d/1Kh_RCdJ2CuojAJCLeEj7-T3Qav-tncSS/edit?usp=drive_link&ouid=103395187960487769781&rtpof=true&sd=true)
  - Different Sequencing Technologies and different denoising approaches
- [X] Practical - [Denoising and **DADA2**](16S_ITS_tutorial/readme.md#step2-quality-controlling-sequences-and-building-feature-table-and-feature-data)
- [X] [Taxonomic assignment](https://docs.google.com/presentation/d/10ExRGzgoCzC_ZKWv1lGKBGesChWdLcq1/edit?usp=drive_link&ouid=103395187960487769781&rtpof=true&sd=true)
## Day 3
- [X] [Taxonomy assignment:**classify-sklearn**](16S_ITS_tutorial/readme.md#step3-summarizing-feature-table-and-feature-data)
- [X] [Fungi and ITS best practices!](https://docs.google.com/presentation/d/14k4cnKy7ZT_b4vvvo2UREiyhZ1HUn5P0/edit?usp=drive_link&ouid=103395187960487769781&rtpof=true&sd=true)

- [X] [Diversity Analysis](https://docs.google.com/presentation/d/1Wjf5QibYwf6lTYIpQTEUVO4HNxc0nYUy/edit#slide=id.g2b75d256dc1_0_564)
- [X] [Running Alpha and Beta Diversity in QIIME2](16S_ITS_tutorial/readme.md#step6-analyzing-alpha-and-beta-diversities)





## Day 4
- [X] [Experimental Design part 2 ](https://docs.google.com/presentation/d/1jkHVJ7l-lWs-Gi0rbFr74giZv8JhOpneiFmUqnFk1Nk/edit#slide=id.g2b75b176bc2_0_489)

- [X] [Lecture  - Multivariate Analysis of Ecological Communities](https://docs.google.com/presentation/d/1SEXLnsAk71ghWJFBjvnSL9-JIU5kHyYi/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true)  
  - [X] Traits of Alpha and Beta Diversity (richness, evenness, dispersion)  
  - [X] Multivariate Tests for differences in microbial community composition  
- [X] [Lab  - Multivariate Statistics](https://drive.google.com/file/d/1T5eGn6qxykXKpbfsEHekV8wB4LBPF-Qf/view?usp=drive_link)  
  - [X] Data import & preparation (normalisations, transformations, metadata)  
  - [X] Multivariate tests for differences in community composition (PERMANOVA, PERMDISP)    
- [X] [Lecture - Differential Abundance Analysis of Microbial Communities](https://docs.google.com/presentation/d/1Z2F2_goIAuuKXQQ7ocClOgq8x6tbpClW/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true) 
  - [X] Lab: Differential Abundance Analysis with DESeq2

## Day 5

- [X] :arrow_right: _It's Case studies Time!!!_
  - [X] [Water-mosquito](Case_Studies/water_mosquito/readme.md)
- [X] [Record your Case Study findings](https://drive.google.com/drive/u/0/folders/1LvRyxTsLQSDU8W9g0RQQUYDGSDly6xfs)
- [X] Beer Time :beers: :beers: :beers: 
- [X] [Some additional processing Tips :volcano: ](DataImport_and_Tax_management/readme.md)
  - [X] Alternative loading methods in Qiime
  - [X] Further, Understanding and Assessing your raw data
