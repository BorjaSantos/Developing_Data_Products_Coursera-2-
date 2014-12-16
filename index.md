---
title       : Analyzing mtcars database.
subtitle    : Simple regression models using mtcars database.
author      : Borja Santos Zorrozua.
job         : Phd student. University of The Basque Country (UPV/EHU)
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Introduction:
This presentation has been done to explain the shiny application. As the title says, I've used the mtcars database which is implemented in the datasets library.

```r
library(datasets)
packageDescription("datasets")
```

```
## Package: datasets
## Version: 3.1.1
## Priority: base
## Title: The R Datasets Package
## Author: R Core Team and contributors worldwide
## Maintainer: R Core Team <R-core@r-project.org>
## Description: Base R datasets
## License: Part of R 3.1.1
## Built: R 3.1.1; ; 2014-07-10 11:14:30 UTC; windows
## 
## -- File: C:/Archivos de programa/R/R-3.1.1/library/datasets/Meta/package.rds
```

---

## Description
### mt cars dataset: 
This dataset was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models). 
### Citation:
Henderson and Velleman (1981), Building multiple regression models interactively. *Biometrics*, **37**, 391-411.

### Note:
Some discrete variables were considered continuous. To fix this problem, these variables were coerced to be factor variables in order to perform better regression models: cyl, vs, am, gear and carb. So I had to build new dataset to include the correct type of these variables, and I called it database.


---

## Database summary
The structure of the variables in the final dataset is

```r
str(database)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : Factor w/ 3 levels "4","6","8": 2 2 1 2 3 2 3 1 1 2 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : Factor w/ 2 levels "0","1": 1 1 2 2 1 2 1 2 2 2 ...
##  $ am  : Factor w/ 2 levels "0","1": 2 2 2 1 1 1 1 1 1 1 ...
##  $ gear: Factor w/ 3 levels "3","4","5": 2 2 2 1 1 1 1 2 2 2 ...
##  $ carb: Factor w/ 6 levels "1","2","3","4",..: 4 4 1 1 2 1 4 2 2 4 ...
```

---

## Main code of the server.R file

```r
# Compute the forumla text in a reactive expression since it is 
# shared by the output$caption and output$mpgPlot expressions
  formulaText <- reactive({
    paste("mpg ~", input$id1)
  })
  # Return the formula text for printing as a caption
  output$caption <- renderText({
    formulaText()
  })
  # Generate plots of the requested variable against mpg
  output$mpg_model <- renderPrint(summary(lm(as.formula(formulaText()))))
  output$mpgPlot <- renderPlot({
    plot(as.formula(formulaText()), 
            data = database,ylab="Miles per Gallon",main=formulaText(),col="red",pch=16)
  })
  output$diagnostic1 <- renderPlot(plot(lm(as.formula(formulaText())),which=1,col="blue",pch=16))
  output$diagnostic2 <- renderPlot(plot(lm(as.formula(formulaText())),which=2,col="blue",pch=16))
```

