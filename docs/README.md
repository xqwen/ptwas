# PTWAS Quick Start

## Software requirement

The bare minimum set of software packages for running PTWAS scan, validation, and estimation procedures are
+ **GAMBIT**: software package implementing generalized burden test using GWAS summary statistics [\[download\]](https://github.com/corbinq/GAMBIT)
+ **PTWAS_est**: perl script for causal assumption validation and effect estimation [\[download\]](https://https://github.com/xqwen/ptwas/tree/master/PTWAS_est/)
+ **tabix**: [\[download\]](https://github.com/samtools/tabix)

To compute PTWAS composite instrumental variables/eQTL weights for scan analysis from users' own eQTL data, it requires additional software packages/libraries

+ **DAP**: Bayesian multi-SNP analysis software for fine-mapping eQTLs [\[download\]](https://github.com/xqwen/dap)
+ **ptwas_builder**: utility to extract information from DAP output (part of the DAP distribution): [\[download\]](https://github.com/xqwen/dap/tree/master/ptwas_builder)
+ **make_GAMBIT_DB.R**: R script to format GAMBIT weights file. Requires ``data.table`` library [\[download\]](https://github.com/xqwen/ptwas/tree/master/PTWAS_scan/)


## Data requirement

#### PTWAS scan using GWAS summary statistics  (single-SNP z-scores)

+ **PTWAS weights files**: used by PTWAS scan procedure to construct composite IVs, also known as the eQTL weights for burden test. We provide the pre-computed weights file from [49 tissues](https://gtexportal.org/home/tissueSummaryPage) in the GTEx project. They can be downloaded from [\[google drive\]](https://tinyurl.com/yxe9k6vl). To construct weights file from users' own eQTL data, follow the detailed instructions [here](https://github.com/xqwen/ptwas/tree/master/PTWAS_scan#1-ptwas-eqtl-weight-construction). Note that, the corresponding index file for a PTWAS weight file created by tabix is also
needed.

+ **GWAS summary statistics file**: GWAS single-SNP z-scores should be organized in a bgzipped vcf file. The format of the file is described [here](https://github.com/xqwen/ptwas/tree/master/PTWAS_scan#2-format-gwas-summary-statistics). The formatted GWAS input files for 114 complex traits described in the PTWAS paper and GTEx consortium paper can be downloaded from [\[google drive\]](https://preview.tinyurl.com/y2hfhehj).

+ **LD reference panel**: consists of a set of vcf files used for computing LD information. We **can not** distribute the LD panels constructed from the GTEx samples because of privacy constraints. (Users want to use GTEx panel should apply from NIH dbGap.) As an alternative, we distribute the [LD panel](https://tinyurl.com/yxe9k6vl) from the 1000 Genome project (phase 3 version 5).


#### PTWAS validation and estimation

The input data should be formatted for each gene-trait pair. The input file is in plain text format, which is explained [here](https://github.com/xqwen/ptwas/tree/master/PTWAS_est#input-file-format).


## Running analysis

#### Running PTWAS scan

In the simplest case, run
```
GAMBIT --gwas gwas_input.vcf.gz --betas ptwas_weights.vcf.gz --ldref "LD_panel_dir/chr*.vcf.gz" --ldref-only
```
More detailed instructions and more options are described [here](https://github.com/xqwen/ptwas/tree/master/PTWAS_scan#3-run-ptwas-scan)


#### Running PTWAS validation and estimation

In the simplest case, run
```
 ptwas_est -d gene_info.txt
```

For more options, please refer to [here](https://github.com/xqwen/ptwas/tree/master/PTWAS_est#command-line-options)
