# Reproducibility repository for a SEER-Medicare study of brain metastases

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1300052.svg)](https://doi.org/10.5281/zenodo.1300052) [![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)

This is a repository enabling replication of results from a manuscript on brain metastases. The manuscript examines frequencies (incidence proportions) of brain metastasis and the classification accuracy of Medicare claims for identifying brain metastasis at primary cancer staging workup. 

The study itself is available [here](http://cebp.aacrjournals.org/content/28/5/917.abstract), and the citation is as follows: 

Lifetime Occurrence of Brain Metastases Arising from Lung, Breast, and Skin Cancers in the Elderly: A SEER-Medicare Study
Mustafa S. Ascha, Quinn T. Ostrom, James Wright, Priya Kumthekar, Jeremy S. Bordeaux, Andrew E. Sloan, Fredrick R. Schumacher, Carol Kruchko and Jill S. Barnholtz-Sloan. Cancer Epidemiol Biomarkers Prev May 1 2019 (28) (5) 917-925; DOI: 10.1158/1055-9965.EPI-18-1116

## Instructions 

### Replication dry-run (no data required)   

If you use a Mac, you can obtain `make` by installing the command-line tools that come as part of XCode (see [here](https://stackoverflow.com/questions/10265742/how-to-install-make-and-gcc-on-a-mac), [here](http://railsapps.github.io/xcode-command-line-tools.html), or [here](https://gist.github.com/rtrouton/f92f263414aaeb946e54) for more).

If you use a Linux distribution, you almost certainly already have `make` installed.
        
Once you have installed `make`, you can take these steps to see what would run without actually executing any scripts. 

1. [Download](https://github.com/mustafaascha/brain-mets-seer/archive/master.zip) and unzip the project folder   
2. Open your terminal and change your working directory to the project folder
    - To do this, run the command `cd {project/folder/path}`, where `{project/folder/path}` is your project folder path. Your project folder path will probably be something like `/Users/your-user-name/Downloads/brain-mets-seer-master/`
3. Run `make --just-print`.     


### Manuscript results replication (requires data)

Replication will require you to:   

1. Download and install dependencies  
    - Depends on: 
        - R 3.4.2, a variety of R packages
        - GNU `make` 4.2.1
        - `pandoc` 1.19.2.1. 
2. Download the project folder 
    - Download the project [here](https://github.com/mustafaascha/brain-mets-seer/archive/master.zip), or 
    - Use `git clone https://github.com/mustafaascha/brain-mets-seer` to clone this repository 
3. Place data in the `seerm` folder. **Note that data is not provided**     
4. Run `make`  

Note that this project takes several hours to run on a single-processor machine with 64GB of memory. You can run `make` with the `-jN` option (where N is some integer) to go through claims files in parallel, but that's not recommended because `make` will begin munging before all of the claims have been scanned/extracted (couldn't get those prerequisites to work right).     


## Project folder contents

- `augur` - This R package reads SEER-Medicare data and extracts relevant rows from the claims data.      

- `extraction` - This folder contains R scripts to extract relevant claims data.     

- `manuscript` - This R package supports analysis and manuscript preparation.    

- `munge` - These scripts convert the data to an analyzable format. Additionally, it contains a histology code lookup table.      

- `analysis` - These scripts are the last step before results are presentable and can be easily used in RMarkdown.    

- `seerm` - This is the folder meant to hold data as provided by IMS. It is provided to demonstrate where data should be placed for study replication. 

### Input: Data  

Data is not provided, though data files may be placed in the `seerm` folder to be used as input for replication studies. 

### Output: Manuscript products

Manuscript tables and figures are output to the `tables-and-figures.html` file after completing `make`. A `cache` folder is created to hold intermediate products, namely, extracted data and PEDSF table transformations. 

Four claims-based brain metastasis identification algorithms are implemented in this work. Identification criteria are presented here as a two-by-two table of synchronous and lifetime chronology against diagnosis code only versus diagnosis and brain imaging code requirements. 

| Chronology      | Claims diagnosis code only        | Claims diagnosis and imaging codes |  
| --------------- | --------------------------------- | ---------------------------------- | 
| **Synchronous** |                                   | Most restrictive
|                 |                                   | 
| **Lifetime**    | Least restrictive                 | 
|                 |                                   | 

For synchronous brain metastasis identification, codes must have occurred within 60 days of primary cancer diagnosis. For lifetime brain metastasis identification, codes may have occurred at any time throughout claims history. When both diagnosis and imaging codes were used, the two must have occurred within 60 days of each other. 

Tables describing sensitivity, specificity, positive predictive value, and Cohen's kappa concordance are generated for each of the most frequent histology categories among lung, breast, and skin cancers. In addition, demographic and clinical characteristics tables are created for each of the listed cancer sites.    

Two figures, each composed of two bar graphs, are generated as part of the `tables-and-figures.html` output. These depict incidence proportions for each primary cancer site, stratified by race (for lung and breast cancers) and sex (for lung and skin cancers).      

## Appendix: References

- Karl Broman has done excellent work on [reproducibility in research](https://kbroman.org/steps2rr/), in particular [advocating the use of `make`](http://kbroman.org/minimal_make/).   

- [ROpenSci](http://ropensci.github.io/reproducibility-guide/) provides practical guidance on reproducibility.     

- [Stodden et al (2013)](http://stodden.net/icerm_report.pdf) offer operationalized definitions of reproducibility, which is especially helpful to discourse.    

## Appendix: Filetree  

Noting that `seerm` is an empty directory where we will place the data, the structure looks like this: 

(top level)   
├── augur    
│   └── ...    
├── extraction     
│   └── ...    
├──manuscript   
│   └── ...    
├── LICENSE    
├── Makefile    
├── munge    
│   └── ...    
├── README.md    
├── analysis    
│   └── ...    
├── seerm     
└── tables-and-figures.Rmd      
  
After adding the data, the file structure will appear as follows: 

(top level)    
├── augur     
│   └── ...     
├── extraction       
│   └── ...    
├──manuscript   
│   └── ...    
├── LICENSE    
├── Makefile    
├── munge    
│   └── ...    
├── README.md    
├── analysis    
│   └── ...    
├── seerm     
│   ├── CCflag07.txt.gz    
│   ├── ...    
│   ├── dme07.file01.txt.gz    
│   ├── ...    
│   ├── medpar07.txt.gz    
│   ├── ...    
│   ├── nch07.file001.txt.gz    
│   ├── ...    
│   ├── outsaf07.file001.txt.gz    
│   ├── ...    
│   ├── pdesaf07.file01.txt.gz    
│   ├── ...    
│   ├── pedsf.breast.cancer.file01.txt.gz    
│   ├── pedsf.breast.cancer.file02.txt.gz    
│   ├── pedsf.lung.cancer.file01.txt.gz    
│   ├── pedsf.lung.cancer.file02.txt.gz    
│   └── pedsf.skin.cancer.txt.gz    
└── tables-and-figures.Rmd       


