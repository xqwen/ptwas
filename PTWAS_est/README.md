# PTWAS Estimation and Model Validation

The procedures for PTWAS estimation and model validation are implemented in ``ptwas_est``. The unit of analysis is a single gene-trait pair, whose information is summarized in a text file (e.g., ``gene_info.txt``). The command line syntax is 

```
    ptwas_est -d gene_info.txt [-t spip_thresh] [-n gene_name]  [--cluster_info]
```


## Input file format

A sample input file from real data, ``sample.ptwas_est.input.txt`` is provided. Each row of the file corresponds to a single SNP that is inferred as a member of a signal cluster in ``DAP``. The columns are 

+ column 1: SNP name
+ column 2: Signal cluster ID
+ column 3  SNP Posterior inclusion probability (PIP)
+ column 4: eQTL effect 
+ column 5: standard error of estimated eQTL effect
+ column 6: GWAS effect
+ column 7: standard error of GWAS effect

The information for column 1 to 5 can be extracted from the DAP fine-mapping file by
```
grep "((" DAP_output_file  | awk '{ if ($5 != -1) print $2,$5,$3,$6,$7}'  | sort -nk2    | more
```


## Command line options


+ ``-d file_name``: specify the input data file
+ ``-t spip_thresh``: specify the signal-level PIP (SPIP) to admit a strong instrument for analysis. By default, the threshold is set to 0.50. 
+ ``-n gene-name``: specify the gene (or gene-trait pair) name for output
+ ``--cluster_info``: show estimate details for each signal cluster (By default, it is not shown)


