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

!! **See the [Unix short tutorial](unix_short_dir/Readme.md) if you need a reminder/refresher.**

All the hands-on exercises will be carried out using [**QIIME2**](https://qiime2.org/) platform . No previous knowledge of computer science is required but a basic knowledge of “bash” would allow to focus more on the microbial analysis.
__LEARNING OUTCOMES__  
1) Understanding the concept, potential and limitation of microbial metabarcoding techniques.
2) Learning how to process raw sequencing reads to obtain meaningful information.
3) Obtaining experience on how to statistically evaluate and visualize your data.
4) Being able to make informed decisions on best practices for your own data.  
  
:exclamation: :exclamation: :exclamation: **All the papers discussed during the course are available in this [Google Drive Folder](https://drive.google.com/drive/u/0/folders/1uPxEeD0vtEoArmq0zDKP7MnP6lmZ4-Nk)** :exclamation: :exclamation: :exclamation:
  

## Program
[Physalia Welcome]([https://drive.google.com/open?id=1zAqld5-NcofYez4QYsGGvX0ZtJGphRNQHCBVwUgYNVE](https://drive.google.com/drive/folders/1uPxEeD0vtEoArmq0zDKP7MnP6lmZ4-Nk))
[Course participants introduction]
## Day 1
- [X] [Introduction to the course and Instructors](Welcome.pptx):
    - [Daniel Antony Pass](https://www.compassbioinformatics.co.uk/about-us)  
    - [Anna Sandionigi](https://scholar.google.com/citations?hl=it&user=DLDuk_EAAAAJ)  
    - [Luigimaria Borruso](https://scholar.google.it/citations?user=ifNJPJEAAAAJ&hl=it)  
- [X] Presentation - [**Brief introduction** about __Metagenomics__ concepts. *What are we going to learn?*](https://docs.google.com/presentation/d/1PuWRR79bvv95SmIo7fnfNiFkjl28xDr3/edit?usp=drive_link&ouid=112247165444752408285&rtpof=true&sd=true)
- [X] Practical - [Connecting to the server via SSH](https://drive.google.com/drive/folders/197xymf3ZAncksVOew1CLLEp--G7MMIMc?usp=sharing)  
- [X] Presentation - [Sequence data in the **FASTQ** format and Quality Filtering](https://docs.google.com/presentation/d/1RowyRGCLqAgt6Oxa_h3c33r4SI9reZlq6ZheRv-HAks/edit?usp=share_link)
- [X] Practical - [First hands on data (& linux review)](main_data_dir/Session1_Fastq_linux.md)
- [X] Presentation-  [**Experimental design part 1** ](https://docs.google.com/presentation/d/1X-9Es3FmfddDbzkanMiRPvwEEoj2bPbG/edit#slide=id.p28)

## Day 2
- [X] Presentation - [Qiime2 & Qiime objects](https://docs.google.com/presentation/d/1IP9xpPLkFUMJgGEURxXN97nxJFSej3lx/edit?usp=sharing&ouid=112247165444752408285&rtpof=true&sd=true)
- [X] Practical - [Let's begin! Atacama Soil Experiment and Loading data in Qiime](16S_ITS_tutorial/readme.md) 
- [X] Presentation - [Concepts on Negative and Positive controls](main_data_dir/controls.md)
- [X] Presentation - [**Key concept: OTU or ASV**;](https://docs.google.com/presentation/d/1Kh_RCdJ2CuojAJCLeEj7-T3Qav-tncSS/edit?usp=drive_link&ouid=103395187960487769781&rtpof=true&sd=true)
  - Different Sequencing Technologies and different denoising approaches
- [X] Practical - [Denoising and **DADA2**](16S_ITS_tutorial/readme.md#step2-quality-controlling-sequences-and-building-feature-table-and-feature-data)
## Day 3
- [X] Presentation - [Experimental Design part 2 ](https://docs.google.com/presentation/d/1jkHVJ7l-lWs-Gi0rbFr74giZv8JhOpneiFmUqnFk1Nk/edit#slide=id.g2b75b176bc2_0_489)
- [X] Presentation - [Diversity Analysis in Qiime](https://docs.google.com/presentation/d/1Wjf5QibYwf6lTYIpQTEUVO4HNxc0nYUy/edit#slide=id.g2b75d256dc1_0_564)
- [X] Practical - [Running Alpha and Beta Diversity in QIIME2](16S_ITS_tutorial/readme.md#step6-analyzing-alpha-and-beta-diversities)
- [X]  Presentation - [Taxonomic assignment](https://docs.google.com/presentation/d/1YdX9hS4BQfj2qfSf79CKtYAHMIXTbI0hIWiemDmmh1s/edit#slide=id.p)
- [X] Practical - [Taxonomy assignment:**classify-sklearn**](16S_ITS_tutorial/readme.md#step3-summarizing-feature-table-and-feature-data)



## Day 4
- [X] [Fungi and ITS best practices!](https://docs.google.com/presentation/d/14k4cnKy7ZT_b4vvvo2UREiyhZ1HUn5P0/edit?usp=drive_link&ouid=103395187960487769781&rtpof=true&sd=true)
- [X] [Lecture  - Multivariate Analysis of Ecological Communities](https://docs.google.com/presentation/d/1SEXLnsAk71ghWJFBjvnSL9-JIU5kHyYi/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true)  
  - [X] Traits of Alpha and Beta Diversity (richness, evenness, dispersion)  
  - [X] Multivariate Tests for differences in microbial community composition  
- [X] [Lab  - Multivariate Statistics](https://glcdn.githack.com/bfosso/physalia_metabarcoding_oct2021/raw/main/Day4_5_material/Physalia-Metabarcoding-Course-Oct21.html)  
  - [X] Data import & preparation (normalisations, transformations, metadata)  
  - [X] Multivariate tests for differences in community composition (PERMANOVA, PERMDISP)    
- [X] [Lecture - Differential Abundance Analysis of Microbial Communities](https://docs.google.com/presentation/d/1Z2F2_goIAuuKXQQ7ocClOgq8x6tbpClW/edit?usp=sharing&ouid=113644278417838041864&rtpof=true&sd=true) 
  - [X] Lab: Differential Abundance Analysis with DESeq2

## Day 5

- [X] :arrow_right: _It's Case studies Time!!!_
  - [X] Practical - [Human microbiome data](human_cancer/readme.md)
  - [X] Practical - [Zoological(Bees bees bees!)](Bee_microbiome/readme.md)
  - [X] Practical - [Water-mosquito](water_mosquito/readme.md)
  - [X] [Record your Case Study findings](https://drive.google.com/drive/folders/1jwEfH0Urm_TjR2lVEO03L1EStmCSWyoC?usp=sharing)
- [X] [Some additional Tips :volcano: ](DataImport_and_Tax_management/readme.md)
  - [X] Alternative loading methods in Qiime
  - [X] What computer to get, and how to install Qiime
  - [X] Further, Understanding and Assessing your raw data
- [X] Beer Time :beers: :beers: :beers: 
