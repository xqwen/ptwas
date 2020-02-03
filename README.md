# Probabilistic Transcriptome-wide Association Analysis (PTWAS)

This repository contains the software implementations and necessary annotation resources for a suite of statistical methods to perform transcriptome-wide association analysis (TWAS). These methods are designed to perform rigorous causal inference connecting genes to complex traits. The statistical models and the key algorithms are described in the manuscript [\[1\]](https://www.biorxiv.org/content/10.1101/808295v1).

The repository includes source code, scripts and necessary data to replicate the results described in the manuscript. A detailed tutorial to guide the users through some specific analysis tasks is also included.

For questions/comments regarding to the software package, please contact Xiaoquan (William) Wen (xwen at umich dot edu).



## License

Software distributed under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at you
r option) any later version. See [LICENSE](http://www.gnu.org/licenses/gpl-3.0.en.html) for more details.


## Quick Start

[Quick start instructions for running PTWAS](https://xqwen.github.com/ptwas/)

## Repository directories

* ``PTWAS_scan``: code and tutorial to run PTWAS scan procedure

* ``PTWAS_est``: code and tutorial to run model diagnosis and effect size estimation procedures

* ``PTWAS_paper``: code and downloadable data sources for simulations and real data analysis used in the [PTWAS paper](https://www.biorxiv.org/content/10.1101/808295v1)


## Required software


* [DAP](https://github.com/xqwen/dap/): this software package performs Bayesian multi-SNP fine-mapping analysis and generates the probabilistic annotations (at model, signal, and SNP levels) required by PTWAS. Required if you want to use your own eQTL data.

*  [GAMBIT](https://github.com/corbinq/GAMBIT): this software package implements a fast general burden test (with pre-defined weights). PTWAS requires GAMBIT to perform scan procedure when only summary-level statistics of GWAS, i.e., single-SNP z-scores, are made available.


## Available resources 

* Pre-built PTWAS annotation file using GTEx (v8) data for 49 tissues: [download](https://tinyurl.com/yxe9k6vl)  

## Contributors

+ Corbin Quick (Harvard University)
+ Yuhua Zhang (University of Michigan)
+ Xiaoquan Wem (University of Michigan)


## Citation

* Chen, Y., Quick, C., Yu, K., Barbeira, A., Luca, F., Pique-Regi, R., Im, H.K., Wen, X. and GTEx Consortium, 2019. Investigating tissue-relevant causal molecular mechanisms of complex traits using probabilistic TWAS analysis. bioRxiv:808295.

