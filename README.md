
<!-- README.md is generated from README.Rmd. Please edit that file -->

# CrossICC

## Table of the content

  - [Overview](#overview)
  - [Installation](#installation)
      - [Via GitHub (latest)](#via-github-latest)
  - [Usage](#usage)
  - [FAQ](#faq)
  - [Contact](#contact)

## Overview

Unsupervised clustering of high-throughput molecular profiling data is
widely adopted for discovering cancer subtypes. However, cancer subtypes
derived from a single dataset are not usually applicable across multiple
datasets from different platforms. We previously published an iterative
clustering algorithm to address the issue (see this
[paper](http://clincancerres.aacrjournals.org/content/21/4/870.long)),
but its use was hampered due to lack of implementation. In this work, we
presented CrossICC that was an R package implementation of this method.
Moreover, many new features were added to improve the performance of the
algorithm. Briefly,
[CrossICC](https://github.com/bioinformatist/CrossICC) utilizes an
iterative strategy to derive the optimal gene set and cluster number
from consensus similarity matrix generated by consensus clustering.
[CrossICC](https://github.com/bioinformatist/CrossICC) is able to deal
with multiple cross platform datasets so that requires no
between-dataset normalizations. This package also provides abundant
functions to help users visualize the identified subtypes and evaluate
the subtyping performance. Specially, many cancer-related analysis
methods are embedded to facilitate the clinical translation of the
identified cancer subtypes.

There are two modes for the integration of clusters derived
cross-platform datasets: `cluster` mode and `sample` mode. For `cluster`
mode, samples from each platform are clustered separately and centroids
of each sub cluster derived from
[ConsensusClusterPlus](https://bioconductor.org/packages/release/bioc/html/ConsensusClusterPlus.htm)
were further clustered to generate super cluster. This process avoided
removing batch effect across platforms. The details step by step
illustration of this algorithm can be found in our previous published
[paper](http://clincancerres.aacrjournals.org/content/early/2014/12/09/1078-0432.ccr-14-2481)
and our recent submitted paper\[coming soon\]. For `sample` mode, sub
clusters were firstly derived from
[ConsensusClusterPlus](https://bioconductor.org/packages/release/bioc/html/ConsensusClusterPlus.htm)
in each platform. We then calculated correlation coefficient between
samples and centroids of clusters to get a new feature vector of each
samples. Based on this new matrix, samples were divided into new
clusters.

## Installation

### Via GitHub (latest)

``` r
install.packages("devtools")
devtools::install_github("bioinformatist/CrossICC")
```

## Usage

CrossICC has the ability to automatically process **arbitrary numbers**
of expression datasets, **no matter which platform they came from**
(Even you can **use sequencing and microarray data together**). What you
**only** need is a list of matrices in R, without any type of
pre-processing (never need manipulation like filtering or
normalization).

``` r
library(CrossICC)
CrossICC.obj <- CrossICC(demo.platforms, skip.mfs = TRUE, max.iter = 100, 
                         cross = "cluster", fdr.cutoff = 0.1, 
                         ebayes.cutoff = 0.1, filter.cutoff = 0.1)
```

CrossICC will automatically iterate your data until it reaches
convergence. By default, CrossICC will generate an `.rds` formatted
object in your home path (`~/`, a.k.a `$HOME` in Linux), followed by an
shiny app as shown below that is opened in your default browser, which
provides you a very intuitive way to view the results.

## Shiny app

Our package also comes with a shiny app. To run
it:

``` r
pkg.suggested <- c('ggalluvial', 'rmarkdown', 'knitr', 'shiny', 'shinydashboard', 'shinyWidgets', "shinycssloaders", 'DT', 'ggthemes', 'ggplot2', 'pheatmap', 'RColorBrewer', 'tibble')
checkPackages <- function(pkg){
  if (!requireNamespace(pkg, quietly = TRUE)) {
    stop("Package pkg needed for shiny app. Please install it.", call. = FALSE)
  }
}
lapply(pkg.suggested, checkPackages)
shiny::runApp(system.file("shiny", package = "CrossICC"))
```

![](inst/imgs/readMe_home.jpg)

## FAQ

  - Question 1: NA values involved in our data set, how to go through
    them?

> A: Users may encounter unexpected errors due to NA values in raw
> dataset. Therefore, we strongly recommanded that users checked the NA
> valus in their data set before loading it into `CrossICC`. To check
> the completed cases in matrix, `completed.cases` can be a good option
> to do that. Here, we also present an example for users to impute there
> data in case they don’t want to remove case in the dataset. The
> imputation method shown here are KNN method from impute package.

``` r
# for a individual matrix, plz do imputation using the following r code
tempdata.impute=impute.knn(as.matrix(tempdata) ,k = 10, rowmax = 0.5, colmax = 0.8)
normalize.Data=as.data.frame(tempdata.impute$data)
```

## Contribution

Qi Zhao [@likelet](https://github.com/likelet) and Yu Sun
[@bioinformatist](https://github.com/bioinformatist) implemented the
packages. Zhixiang Zuo
[@zhixiang](https://scholar.google.com/citations?user=Ln_bw9AAAAAJ&hl=zh-CN)
supervise the project. Zekun Liu performed the test and helped run an
example of the package. For more information or questions, plz contact
either of the authors above.
## Citation 

Zhao, Qi, Yu Sun, Zekun Liu, Hongwan Zhang, Xingyang Li, Kaiyu Zhu, Ze-Xian Liu, Jian Ren, and Zhixiang Zuo. "CrossICC: iterative consensus clustering of cross-platform gene expression data without adjusting batch effect." Briefings in Bioinformatics (2019).
