
ErmineR
=======

[![Build Status](https://travis-ci.org/PavlidisLab/ermineR.svg?branch=master)](https://travis-ci.org/PavlidisLab/ermineR) [![codecov](https://codecov.io/gh/PavlidisLab/ermineR/branch/master/graph/badge.svg)](https://codecov.io/gh/PavlidisLab/ermineR) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/PavlidisLab/ermineR?branch=master&svg=true)](https://ci.appveyor.com/project/PavlidisLab/ermineR)

This is an R wrapper for Pavlidis Lab's [ermineJ](http://erminej.msl.ubc.ca/). A tool for gene set enrichment analysis with multifunctionality correction.

Usage
-----

See documentation for `ora`, `roc`, `gsr`, `precRecall` and `corr` to see how to use them.

Explanation of what each method does is explained

### Replicable go terms

Go terms are updated frequently so results can differ between versions. The default option of all ermineR functions is to get the latest GO version however this means you may get different results when you repeat the experiment later. If you want to use a specific version of GO, ermineR provides functions to deal with that.

-   `goToday`: Downloads the latest version of go to a path you provide
-   `getGoDates`: Lists all dates where a go version is available, from the most recent to oldest
-   `goAtDate`: Given a valid date, downloads the Go version from a specific date to a file path you provide

To use a specific version of GO, make sure to set `geneSetDescription` argument of all ermineR functions to the file path where you saved the go terms

### Examples

#### Use ORA with a hitlist

``` r
# genes for GO:0051082
hitlist = c("AAMP", "AFG3L2", "AHSP", "AIP", "AIPL1", "APCS", "BBS12", 
            "CALR", "CALR3", "CANX", "CCDC115", "CCT2", "CCT3", "CCT4", "CCT5", 
            "CCT6A", "CCT6B", "CCT7", "CCT8", "CCT8L1P", "CCT8L2", "CDC37", 
            "CDC37L1", "CHAF1A", "CHAF1B", "CLGN", "CLN3", "CLPX", "CRYAA", 
            "CRYAB", "DNAJA1", "DNAJA2", "DNAJA3", "DNAJA4", "DNAJB1", "DNAJB11", 
            "DNAJB13", "DNAJB2", "DNAJB4", "DNAJB5", "DNAJB6", "DNAJB8", 
            "DNAJC4", "DZIP3", "ERLEC1", "ERO1B", "FYCO1", "GRPEL1", "GRPEL2", 
            "GRXCR2", "HEATR3", "HSP90AA1", "HSP90AA2P", "HSP90AA4P", "HSP90AA5P", 
            "HSP90AB1", "HSP90AB2P", "HSP90AB3P", "HSP90AB4P", "HSP90B1", 
            "HSP90B2P", "HSPA1A", "HSPA1B", "HSPA1L", "HSPA2", "HSPA5", "HSPA6", 
            "HSPA8", "HSPA9", "HSPB6", "HSPD1", "HSPE1", "HTRA2", "LMAN1", 
            "MDN1", "MKKS", "NAP1L4", "NDUFAF1", "NPM1", "NUDC", "NUDCD2", 
            "NUDCD3", "PDRG1", "PET100", "PFDN1", "PFDN2", "PFDN4", "PFDN5", 
            "PFDN6", "PIKFYVE", "PPIA", "PPIB", "PTGES3", "RP2", "RUVBL2", 
            "SCAP", "SCG5", "SERPINH1", "SHQ1", "SIL1", "SPG7", "SRSF10", 
            "SRSF12", "ST13", "SYVN1", "TAPBP", "TCP1", "TMEM67", "TOMM20", 
            "TOR1A", "TRAP1", "TTC1", "TUBB4B", "UGGT1", "ZFYVE21")
oraOut = ora(annotation = 'Generic_human',
             hitlist = hitlist)

head(oraOut$results)
```

    ## # A tibble: 6 x 12
    ##   Name         ID    NumProbes NumGenes RawScore      Pval CorrectedPvalue
    ##   <chr>        <chr>     <int>    <int>    <dbl>     <dbl>           <dbl>
    ## 1 unfolded pr… GO:0…       115      115     115. 2.29e-303       9.50e-300
    ## 2 protein bin… GO:0…        29       29      25. 7.47e- 53       1.55e- 49
    ## 3 chaperone-m… GO:0…        59       59      28. 1.67e- 47       2.31e- 44
    ## 4 'de novo' p… GO:0…        35       35      22. 3.72e- 41       3.86e- 38
    ## 5 chaperone b… GO:0…        85       85      22. 2.51e- 30       2.08e- 27
    ## 6 response to… GO:0…       173      173      22. 4.95e- 23       3.42e- 20
    ## # ... with 5 more variables: MFPvalue <dbl>, CorrectedMFPvalue <dbl>,
    ## #   Multifunctionality <dbl>, `Same as` <chr>, GeneMembers <chr>

#### Using your own GO annotations

If you want to use your own GO annotations instead of getting files provided by Pavlidis Lab, you can use `makeAnnotation` after turning your annotations into a list. See the example below

``` r
library('org.Hs.eg.db') # get go terms from the bioconductor affy 
library(dplyr)
goAnnots = as.list(org.Hs.egGO)
goAnnots = goAnnots %>% lapply(names)
goAnnots %>% head
```

    ## $`1`
    ##  [1] "GO:0002576" "GO:0008150" "GO:0043312" "GO:0005576" "GO:0005576"
    ##  [6] "GO:0005615" "GO:0031012" "GO:0031093" "GO:0034774" "GO:0070062"
    ## [11] "GO:0072562" "GO:1904813" "GO:0003674"
    ## 
    ## $`2`
    ##  [1] "GO:0001869" "GO:0002576" "GO:0007597" "GO:0010951" "GO:0022617"
    ##  [6] "GO:0043547" "GO:0048863" "GO:0051056" "GO:0005576" "GO:0005576"
    ## [11] "GO:0005829" "GO:0031093" "GO:0070062" "GO:0072562" "GO:0002020"
    ## [16] "GO:0004867" "GO:0005096" "GO:0005102" "GO:0005515" "GO:0019838"
    ## [21] "GO:0019899" "GO:0019959" "GO:0019966" "GO:0043120" "GO:0048306"
    ## 
    ## $`3`
    ## NULL
    ## 
    ## $`9`
    ## [1] "GO:0006805" "GO:0005829" "GO:0004060"
    ## 
    ## $`10`
    ## [1] "GO:0006805" "GO:0005829" "GO:0004060" "GO:0005515"
    ## 
    ## $`11`
    ## NULL

The goAnnots object we created has go terms per entrez ID. Similar lists can be obtained from other species db packages in bioconductor and some array annotation packages. We will now use the `makeAnnotation` function to create our annotation file. This file will have the names of this list (entrez IDs) as gene identifiers so any score or hitlist file you provide should have the entrez IDs as well.

`makeAnnotation` only needs the list with gene identifiers and go terms to work. But if you want to have a complete annotation file you can also provide gene symbols and gene names. Gene names have no effect on the analysis. Gene symbols matter if you are [providing custom gene sets](http://erminej.msl.ubc.ca/help/input-files/gene-sets/) and using "Option 2" or if same genes are represented by multiple gene identifiers (eg. probes). Gene symbols will also be returned in the `GeneMembers` column of the output. If they are not provided, gene IDs will also be used as gene symbols

Here we'll set them both for good measure.

``` r
geneSymbols = as.list(org.Hs.egSYMBOL) %>% unlist
geneName = as.list(org.Hs.egGENENAME) %>% unlist

annotation = makeAnnotation(goAnnots,
                            symbol = geneSymbols,
                            name = geneName,
                            output = NULL, # you can choose to save the annotation to a file
                            return = TRUE) # if you only want to save it to a file, you don't need to return
```

Now that we have the annotation object, we can use it to run an analysis. We'll try to generate a hitlist only composed of genes annotated with <GO:0051082>.

``` r
mockHitlist = goAnnots %>% sapply(function(x){'GO:0051082' %in% x}) %>% 
    {goAnnots[.]} %>% 
    names

mockHitlist %>% head
```

    ## [1] "14"  "325" "811" "821" "871" "908"

``` r
oraOut = ora(annotation = annotation,
             hitlist = mockHitlist)

head(oraOut$results)
```

    ## # A tibble: 6 x 12
    ##   Name         ID    NumProbes NumGenes RawScore      Pval CorrectedPvalue
    ##   <chr>        <chr>     <int>    <int>    <dbl>     <dbl>           <dbl>
    ## 1 unfolded pr… GO:0…       113      113     113. 1.62e-300       7.02e-297
    ## 2 protein bin… GO:0…        28       28      23. 3.93e- 48       8.51e- 45
    ## 3 chaperone-m… GO:0…        60       60      26. 2.42e- 43       3.50e- 40
    ## 4 'de novo' p… GO:0…        36       36      21. 9.45e- 39       1.02e- 35
    ## 5 chaperone b… GO:0…        89       89      22. 2.58e- 30       2.23e- 27
    ## 6 response to… GO:0…       182      182      25. 1.60e- 27       1.16e- 24
    ## # ... with 5 more variables: MFPvalue <dbl>, CorrectedMFPvalue <dbl>,
    ## #   Multifunctionality <dbl>, `Same as` <chr>, GeneMembers <chr>

We can see <GO:0051082> is the top scoring hit as expected.
