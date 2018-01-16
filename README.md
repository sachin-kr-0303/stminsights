# stminsights

This app enables interactive validation, interpretation and visualisation of [Structural Topic Models](http://structuraltopicmodel.com) (STM). In case you are not familiar with STM, the [package vignette](https://cran.r-project.org/web/packages/stm/vignettes/stmVignette.pdf) is an excellent starting point.

## How to Install

You can download and install the app by running ``devtools::install_github('methodds/stminsights')``

## How to Use

After installing stminsights you can run ``stminsights::run_app()`` to launch the shiny app in your browser. Afterwards you can upload an `.RData` file which should include:

- one or several stm objects.
- one or several estimateEffect objects.
- an object `out` which was used to fit your stm models

As an example, the following code fits two model and estimates effects for the stm corpus poliblog5k and saves all objects required for stminsights in `stm_poliblog5k.RData`. 

```
library(stm)

out <- list(documents = poliblog5k.docs,
            vocab = poliblog5k.voc,
            meta = poliblog5k.meta)
stm_poli <- stm(documents = out$documents, 
                vocab = out$vocab,
                data = out$meta, 
                prevalence = ~ rating * s(day),
                K = 20)

prep_stm_poli <- estimateEffect(1:20 ~ rating * s(day), stm_poli,
                                meta = out$meta)

stm_poli_content <-  stm(documents = out$documents, 
                         vocab = out$vocab,
                         data = out$meta, 
                         prevalence = ~ rating + s(day),
                         content = ~ rating,
                         K = 15)  

prep_poli_content <- estimateEffect(1:15 ~ rating + s(day), stm_poli_content,
                                    meta = out$meta)

save.image('stm_poliblog5k.RData')
```

After launching stminsights and uploading the file, all objects are automatically imported  and you can select which models and effect estimates to analyze.

## Live Demo

In case you want to try out a demo before installing stminsights,  download `stm_poliblog5k.RData` in the data folder of this repo and visit [www.polsoz.uni-bamberg.de/stminsights] to play around with the app.


