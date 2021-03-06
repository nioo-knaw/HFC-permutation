Contents
========

* [How to use this file](#how-to-use-this-file)
* [Introduction](#introduction)
* [Copying the pipeline](#copying-the-pipeline)
* [Directories](#directories)
* [Prerequisites](#prerequisites)
* [Start the pipeline](#start-the-pipeline)
* [More Reading](#more-reading)


How to use this file
---------------------

This README file gives a global introduction on how to work on the server, cloning the pipeline repository and how to run the pipeline. We tried to be as complete and precise as possible.
If you have any question, comment, complain or suggestion or if you encounter any conflicts or errors in this document or the pipeline, please contact your Bioinformatics Unit (Bioinformatics-support@nioo.knaw.nl) or open an `Issue`!

###### Enjoy your analysis and happy results!

```
Text written in boxes is code, which usually can be executed in your Linux terminal. You can just copy/paste it.
Sometimes it is "special" code for R or any other language. If this is the case, it will be explicitly mentioned in the instructions.
```

`Text in a red box indicates directory or file names`

Text in brackets "<>" indicates that you have to replace it and the brackets by your own appropiate text.

Introduction
------------

Investigating inbreeding depression has a long history in the fields of ecology, evolution, and conservation biology. However, many of the conclusions remain controversial and especially our understanding of the underlying genetic mechanisms of inbreeding depression in the wild is limited. It has been shown that marker number can be an important factor to be considered. Furthermore, the development and availability of high-throughput genetic data has made a more precise measurement of inbreeding of many species feasible. With this permutation script it is possible to study how different heterozygosity measurements vary in your dataset when you change the amount of SNPs in the input file. The two heterozygosity measurements are runs of homozygosity (FROH) and proportion of homozygous loci (FHOM). These are calculated with the program PLINK. Furthermore, the identity disequilibrium (ID) is calculated from the data. ID is the covariance in heterozygosity among markers within individuals, which should reflect identity by descent (IBD) of those markers i.e. how well heterozygosity-fitness correlations are detected. It can be calculated in two different ways: two-locus heterozygosity disequilibrium (g2) and heterozygosity-heterozygosity correlation (HHC). These calculations are done with program Inbreedr. 

Plink: Purcell et al. 2007. Am. J. Hum. Genet. 81:559-575.

Inbreedr: Stoffel et al. 2016. Methods Ecol. Evol. 7:1331-1339.

More about ID: David et al. 2007. Mol. Ecol. 16:2474–2487.


Copying the pipeline
------------------

To start a new analysis project based on this pipeline, follow the following steps:

- Clone and rename the pipeline-skeleton from our GitLab server by typing in the terminal. Replace <name> by your NIOO login-name. Cloning will only work, if you have logged in to gitlab at least once before:

```
git clone https://github.com/nioo-knaw/HFC-permutation
```

- Enter `HFC-permutation`

```
cd HFC-permutation
```

Directories
--------------------------

##### The toplevel `README` file

This file contains this file with general information about how to run this pipeline.

##### The `data` directory

Place your plink .ped and .map file here. Also contains a file with the length of all chromosomes. If you do not use great tit, please replace it by the appropriate values but save the file under the same name.

##### The `src` directory

Contains templates for the concatenated results file. Templates for the Froh and Fhom output `Froh.tmp` and `Fhom.tmp` contain a single column with all family ID names. The template for inbreedR `inbreedR.tmp` contains a single column with all values calculated by the package (g2, g2_p_val, g2_se, mean_HCC, sd_HCC). Please adjust the files appropriately, before running the pipeline.

##### The `docs` directory

Originally, this pipeline was written in pure bash code and parallelized with gnu parallels. The original scripts are stored in a sub-directory. All older versions of the snakemake pipeline (work-in-progress) are also deposited here.

##### The `results, permutation, Froh, Fhom, inbreedR, scratch` directories

These directories are not copied by cloning but will be produced during execution of the pipeline. Your final results will be found in `results` all other intermediate results will be stored in one of the corresponding directories.

Prerequisites
------------------

You have to install `plink` using conda before starting the pipeline:

```
conda create -n plink2
source activate plink2
conda install -c bioconda plink2=1.90b3.35
```

if you already have created the environment `plink2`, than activate it by

```
source activate plink2
```

You can deactivate the environment after pipeline execution with

```
source deactivate
```

Start the pipeline
------------------

##### Activate the snake environment

(see [Prerequisites](#prerequisites) for detailed instructions).
```
source activate plink2
```

##### Adjust or create input files in the `data` directory.

see [Directories](#directories)

##### Adjust `config.yaml`

Open and adjust the config file, appropriately:
```
nano config.yaml
prefix: The basename of your .ped and .map files in data
snp: The number of SNP's you want to sample, randomly.
range: The number of times you want to sample.
```

##### Execute the pipeline

Make sure that you are in the executive directory `HFC-permutation` and perform a dry run:
```
snakemake -np
```
If everything looks fine, run the pipeline with:
```
snakemake -p
```

More Reading
------------------

[PLINK 2.0 alpha](https://www.cog-genomics.org/plink/2.0/)
