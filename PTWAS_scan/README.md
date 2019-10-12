# PTWAS Scan

The PTWAS scan procedure is implemented in [GAMBIT](https://github.com/corbinq/gambit). There are  two required input files, both of which are in compressed vcf formats:

+ PTWAS SNP weight file: containing eQTL weights constructed by the PTWAS algorithm
+ GWAS summary-level stats file: containing single-SNP association  statistics from a GWAS
Additionally, an LD reference panel is also required. 

The command for the scan analysis is

```
GAMBIT --gwas gwas_file --betas ptwas_weight --ldref LD_panel_files --ldref-only 
```

For more details on running GAMBIT, please refer to the [GAMBIT repository](https://github.com/corbinq/gambit), or contact Corbin Quick (corbinq@gmail.com).


## PTWAS eQTL weight construction

Here we outline the procedure to derive PTWAS weights from eQTL data.

### Step 1: Fine-mapping eQTL by [DAP](https://github.com/xqwen/dap/)

The repository for DAP is https://github.com/xqwen/dap/. The detailed instructions on running DAP for fine-mapping analysis are provided [here](https://github.com/xqwen/dap/tree/master/dap_src#dap-g-c-implementation-of-adaptive-dap-algorithm). In brief, assuming individual-level genotype data are available, one needs to convert the phenotype (gene expression) data nad genotype data into the sbams format for each gene. The command to run DAP for cis-eQTL fine-mapping is 

```
dap-g -d gene_sbams_file -o dap_output_file
```

Please refer to the [DAP repository](https://github.com/xqwen/dap/tree/master/dap_src#dap-g-c-implementation-of-adaptive-dap-algorithm) for more command line options. 

### Step 2: compute PTWAS weight

This is achieved by running [ptwas\_builder](https://github.com/xqwen/dap/tree/master/ptwas_builder) for each gene. (It is practically sufficient to run ptwas\_build only for eGenes.) The sbams file and the DAP output file are required. The command for this step is 

```
ptwas_builder -f dap_output_file -d gene_sbmas_file > gene_ptwas.weights.txt
```


