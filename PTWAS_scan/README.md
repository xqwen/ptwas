# PTWAS Weight Construction and Scan 



Here we provide details on constructing PTWAS weights from users' own eQTL dataset and running the PTWAS scan procedure.


## 1. PTWAS eQTL weight construction

Note that we have pre-computed PTWAS weights from the GTEx (v8) data for 49 tissues, the relevant files can be downloaded [here](https://tinyurl.com/yxe9k6vl). In case that you want to build the weights from your own eQTL data, follow along the following steps.

**Important Note**: the DB making script assumes genomic position information is coded in the SNP IDs. A SNP ID should start with chromosome number and followed by ``_`` and the position.  Other information can follow afterwards e.g., ``chr1_44503918_C_T_b38`` is a valid SNP ID. If such naming convention is not followed, the DB building script ``make_GAMBIT_DB.R`` will report errors.  



### Step 1: Fine-mapping eQTL by [DAP](https://github.com/xqwen/dap/)

The repository for DAP is https://github.com/xqwen/dap/. The detailed instructions on running DAP for fine-mapping analysis are provided [here](https://github.com/xqwen/dap/tree/master/dap_src#dap-g-c-implementation-of-adaptive-dap-algorithm). In brief, assuming individual-level genotype data are available, one needs to convert the phenotype (gene expression) data nad genotype data into the sbams format for each gene. The command to run DAP for cis-eQTL fine-mapping is 

```
dap-g -d gene_sbams_file -o dap_output_file
```

Please refer to the [DAP repository](https://github.com/xqwen/dap/tree/master/dap_src#dap-g-c-implementation-of-adaptive-dap-algorithm) for more command line options. In most cases, we recommend to use the default options and specify only input and output files in the command line. Note that each gene requires a separate DAP run.

### Step 2: Compute PTWAS weight

This is achieved by running [ptwas\_builder](https://github.com/xqwen/dap/tree/master/ptwas_builder) for each gene. (It is practically sufficient to run ptwas\_build only for eGenes.) The sbams file and the DAP output file are required. The command for this step is 

```
ptwas_builder -f dap_output_file -d gene_sbmas_file -g gene_name [-t tissue_name ] [-wt min_abs_weight_thresh] [-o gene_name.ptwas_weights.txt]
```

The command line options are 

+ ``-f dap_output_file``: give the fine-mapping output for the target gene
+ ``-d gene_sbmas_file``: the same input sbmas file for DAP analysis
+ ``-g gene_name``: give the name of the target gene
+ ``-t tissue_name``: (optional) give the tissue name. Required for building multi-tissue weights
+ ``-wt min_abs_weight_thresh``: (optional) defines the minimum absolute value of weight to be output
+ ``-o output_file_name``: (optional) output file name


### Step 3: Convert to GAMBIT database format

First, concatenate PTWAS weights generated from Step 2 into a single file, e.g., 
```
  cat *.ptwas_weights.txt | gzip - > all_gene.ptwas_weights.gz
``` 

Next, run the R script ``make_GAMBIT_DB.R``
```
  Rscript make_GAMBIT_DB.R -d all_gene.ptwas_weights.gz -o all_gene.ptwas_weights.gambit.vcf
```

If there are eQTL data for multiple tissues, create the ``all_gene.ptwas_weights.gz`` and the corresponding index files for each tissue individually, and create a directory, e.g., ``tissue_dir``. Next, place all single-tissue weight files into the directory, and run  
```
 Rscript make_GAMBIT_DB.R -D tissue_dir  -o Multi_tissue_all_gene.ptwas_weights.gambit.vcf
``` 
This will create a multi-tissue GAMBIT DB file.


## 2. Format GWAS summary statistics

The required GWAS summary statistics are single-SNP z-scores. The input file for GWAS should be a bgzipped vcf file. It must be ordered by chromosome and genomic position, with input fields as shown below:

```
#CHR  POS     REF  ALT  SNP_ID      N         ZSCORE   ANNO
1     721290  C    G    rs12565286  58663.62  0.86661  Intergenic
1     752566  G    A    rs3094315   57135     0.5521   Intergenic
1     775659  A    G    rs2905035   54570     1.12098  Intron:LOC643837
1     777122  A    T    rs2980319   54570     1.11906  Exon:LOC643837
```

Note that the first four fields and `ZSCORE` are required, while `SNP_ID`, `ANNO` and `N` (effective sample size) are optional. As a result,`SNP_ID` does not need to follow the genomic position convention. 

An index file is also required, if it is not already generated. Use 
```
  tabix -p vcf gwas_file.vcf.gz
```  
to generate the index file. 

An example GWAS summary file from the GIANT consortium height study is provided in [here](https://tinyurl.com/tqyhnom).

## 3. Run PTWAS scan

The PTWAS scan procedure is implemented in [GAMBIT](https://github.com/corbinq/gambit). There are  two required input files, both of which are in compressed vcf formats:

+ PTWAS SNP weight file: containing eQTL weights constructed by the PTWAS algorithm
+ GWAS summary-level stats file: containing single-SNP association  statistics from a GWAS
+ LD panels for the studied population: these files are zipped vcf files recording haplotypes and organized by chromosomes.

Due to NIH policy, we can't re-distribute the LD panels that we used for the GTEx data, because the panels contain individual-level genotype data. As an alternative, we provide an LD panel built from 1000 Genome project, phase 3 version 5, which can be downloaded [here](https://tinyurl.com/yxe9k6vl) and is suitable to study European population. Run `tar zxf 1KG_phase3_v5_panel.gambit.tgz` to unpack the LD panel. 


The command for the scan analysis is

```
GAMBIT --gwas gwas_file --betas ptwas_weight_file --ldref LD_panel_files --ldref-only 
```

The following command will run multi-tissue PTWAS scan for the GIANT height data with the provided annotation file and the 1KG European LD panel files:
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas PTWAS_scan.weights.hg38.txt.gz --ldref "G1K_EUR_3V5/chr*.vcf.gz" --ldre-only
```

Users can specify using only a subset of tissues for analysis with the ``--tissues`` option, e.g.,
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas PTWAS_scan.weights.hg38.txt.gz --ldref "G1K_EUR_3V5/chr*.vcf.gz" --ldre-only --tissues Whole_Blood,Muscle_Skeletal 
```
This will only analyze TWAS analysis using eQTL weights from Whole_Blood and Muscle_Skeletal. 



For more details on running GAMBIT, please refer to the [GAMBIT repository](https://github.com/corbinq/gambit), or contact Corbin Quick (corbinq@gmail.com).




