## Empirical comparison of TWAS analysis

The data used in this analysis can be downloaded [here](https://tinyurl.com/tqyhnom). The GWAS data is the height data from the GIANT consortium. We constructed GAMBIT weights from PTWAS, TWAS-Fusion (BSLMM), PrediXcan (Elastic-Net), and SMR using the GTEx whole blood eQTL data (v8).

The commands used for various methods are 

+ PTWAS
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas gtex_v8.whole_blood.ptwas_egene.gambit.vcf.gz --ldref "LD_ref/chr*.gz" --ldref-only
```
+ TWAS-Fusion
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas gtex_v8.whole_blood.fusion_egene.gambit.vcf.gz --ldref "LD_ref/chr*.gz" --ldref-only
```
+ PrediXcan
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas gtex_v8.whole_blood.predixcan_egene.gambit.vcf.gz --ldref "LD_ref/chr*.gz" --ldref-only
```
+ SMR
```
GAMBIT --gwas GIANT_Height.gambit.vcf.gz --betas gtex_v8.whole_blood.smr_egene.gambit.vcf.gz --ldref "LD_ref/chr*.gz" --ldref-only
```
