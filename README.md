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

## Input files
The input data format has the following fields structure

V1    |            V2 |   V3| V4  |  V5   |   V6 | V7 | V8
--- | --- | --- | --- | --- | --- | --- | --- 
TGTGCCAGCAGTTTCTCGACCTGTTCGGCTAACTATGGCTACACCTTC | CASSFSTCSANYGYTF | 10372 |  - | miTCR | sample2  | 1 | 1    
TGTGCCAGCAGCCTTGGGACAGATACGCAGTATTTT   |   CASSLGTDTQYF  | 538 | - | miTCR | sample2 |  1 | 1   

All the input files are in the folder `./Data/forPMD.rev1.insilico/` and `./Data/forPMD.rev1/`.

The combination of the first two columns represent the *primary key* of the data. This combination is different for each row. The third column must contain the reads count. All the other columns contain metadata information about the clone. We will use column 5 (analysis tool) as annotation label for the analysis. Each sample is provided as a separate table.

## Expected values
The expected values (richness and evenness) are displayed as balck dot in the clonal plane. The next few lines of code define the expectev values for four samples with coordinates: (0,0), (8.39,2.3) , (8.49,3.91), and (8.5,3.91). Modify in accordance with your expected values
```
# Expected values in the synthetic dataset
expMat<-t(matrix(c(0,	0,	0,	0,	0,
                   8.39,	0,	0,	2.3,	0,
                   8.49,	0,	0,	3.91,	0,
                   8.5,	0,	0,	3.91,	),
                 nrow=5,ncol=4))
```
## Correction values
In the synthetic experiments we generated the CD3 regions randomly so that many of them were not functional. Most of the tools (correctly) discards those sequences. The clonality of the samples has to be corrected by a factor that depends on the fraction of the reads that are functional. For our _in silico_ data we used the table in `./Data/CorrFact.txt` to correct the values.
```
corrFac<-read.table("./Data/CorrFact.txt",header=F,sep="\t")
```

## Read the sample
### Read the data file (experimental dataset S1)
```R
tab<-read.table("./Data/forPMD.rev1/RP_S1_allTools.default.forPMD.final.txt",header=F)
```


### Compute the Diversity Indices
```R
res<-pmd(tab,ISrange=c(1,2),factorRange = c(5))
res$eulerMat
```

### Prepare the scatterplot
```R
d<-pmdscatterplot(res$eulerMat, labelsize=4,spotRadius=4)
```

### (Optional) Add the expected sample clonality

```R
d<-pmdScatterAddSamples(expMat[c(1),],d)
```

### Plot the scatterplot
```R
d<-d+geom_abline(slope=0,intercept=expMat[1,4],colour="gray",linetype=3)
d
```

# License
This notebook is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License<
along with this program.  If not, see [http://www.gnu.org/licenses/](http://www.gnu.org/licenses/).
