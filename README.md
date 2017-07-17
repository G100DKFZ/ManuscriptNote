# Installation
In the present Rhtml document we embedded the R code sufficient to run the PMD clonal analysis on the four experimental samples described in the manuscript.
Unzip the distribution file in a folder of choice (i.e. `~/PMD/`) and set in the next chunk the working directory accordingly.
```R
setwd("~/PMD/")
getwd()
```

This code depends on two external packages, `ggplot2` and `ggrepel`. Consider to install them before to run the notebook.

The notebook is written in Rhtml and can be parsed via [Knitr](http://yihui.name/knitr/). We suggest to use [RStudio](http://www.rstudio.com/) to play with the document.
# Usage
This version contains the instructions to use it as an R notebook.


The input data format has the following fields structure

V1    |            V2 |   V3| V4  |  V5   |   V6 | V7 | V8
--- | --- | --- | --- | --- | --- | --- | --- 
TGTGCCAGCAGTTTCTCGACCTGTTCGGCTAACTATGGCTACACCTTC | CASSFSTCSANYGYTF | 10372 |  - | miTCR | sample2  | 1 | 1    
TGTGCCAGCAGCCTTGGGACAGATACGCAGTATTTT   |   CASSLGTDTQYF  | 538 | - | miTCR | sample2 |  1 | 1   

The combination of the first two columns represent the *primary key* of the data. This combination is different for each row. The third column must contain the reads count. All the other columns contain metadata information about the clone. We will use column 5 (analysis tool) as annotation label for the analysis. Each sample is provided as a separate table.
