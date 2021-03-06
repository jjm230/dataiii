Test statistical assignment
================
jjm230
27th January 2020

Introduction
------------

Please change the author and date fields above as appropriate. Do not change the output format. Once you have completed the assignment you want to knit your document into a markdown document in the "github\_document" format and then commit both the .Rmd and .md files (and all the associated files with graphs) to your private assignment repository on Github.

Reading data (40 points)
------------------------

First, we need to read the data into R. For this assignment, I ask you to use data from the youth self-completion questionnaire (completed by children between 10 and 15 years old) from Wave 9 of the Understanding Society. It is one of the files you have downloaded as part of SN6614 from the UK Data Service. To help you find and understand this file you will need the following documents:

1.  The Understanding Society Waves 1-9 User Guide: <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/user-guides/mainstage-user-guide.pdf>
2.  The youth self-completion questionnaire from Wave 9: <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/questionnaire/wave-9/w9-gb-youth-self-completion-questionnaire.pdf>
3.  The codebook for the file: <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/datafile/youth/wave/9>

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.2.1     ✓ purrr   0.3.3
    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# This attaches the tidyverse package. If you get an error here you need to install the package first. 
Data <- read_tsv("~/Documents/Exeter/Q-Step/POL2094 Data Analysis in Social Science III/Data III Project/Data III Project/data/UKDA-6614-tab/tab/ukhls_w9/i_youth.tab")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
View(Data) ## I note that this .tab file is not too large compared to others within W9 and viewing the organisation of the file might be helpful in understanding certain aspects of the data.
# You need to add between the quotation marks a full path to the required file on your computer.
```

Tabulate variables (10 points)
------------------------------

In the survey children were asked the following question: "Do you have a social media profile or account on any sites or apps?". In this assignment we want to explore how the probability of having an account on social media depends on children's age and gender.

Tabulate three variables: children's gender, age (please use derived variables) and having an account on social media.

``` r
# add your code here
## Relevant variables: i_ypsex; i_age_dv; i_ypsocweb
table(Data$i_ypsex, Data$i_age_dv, Data$i_ypsocweb)             ## Tabulate the variables by ownership of social media account
```

    ## , ,  = -9
    ## 
    ##    
    ##       9  10  11  12  13  14  15  16
    ##   1   0   1   1   4   0   2   0   0
    ##   2   0   1   0   2   3   0   0   0
    ## 
    ## , ,  = 1
    ## 
    ##    
    ##       9  10  11  12  13  14  15  16
    ##   1   0 102 151 208 190 236 204   6
    ##   2   0 121 195 191 231 227 212   3
    ## 
    ## , ,  = 2
    ## 
    ##    
    ##       9  10  11  12  13  14  15  16
    ##   1   0 129  88  35  23  16  15   0
    ##   2   1 106  61  27  16  10   3   0

``` r
addmargins(table(Data$i_ypsex, Data$i_age_dv, Data$i_ypsocweb)) ## Add totals
```

    ## , ,  = -9
    ## 
    ##      
    ##          9   10   11   12   13   14   15   16  Sum
    ##   1      0    1    1    4    0    2    0    0    8
    ##   2      0    1    0    2    3    0    0    0    6
    ##   Sum    0    2    1    6    3    2    0    0   14
    ## 
    ## , ,  = 1
    ## 
    ##      
    ##          9   10   11   12   13   14   15   16  Sum
    ##   1      0  102  151  208  190  236  204    6 1097
    ##   2      0  121  195  191  231  227  212    3 1180
    ##   Sum    0  223  346  399  421  463  416    9 2277
    ## 
    ## , ,  = 2
    ## 
    ##      
    ##          9   10   11   12   13   14   15   16  Sum
    ##   1      0  129   88   35   23   16   15    0  306
    ##   2      1  106   61   27   16   10    3    0  224
    ##   Sum    1  235  149   62   39   26   18    0  530
    ## 
    ## , ,  = Sum
    ## 
    ##      
    ##          9   10   11   12   13   14   15   16  Sum
    ##   1      0  232  240  247  213  254  219    6 1411
    ##   2      1  228  256  220  250  237  215    3 1410
    ##   Sum    1  460  496  467  463  491  434    9 2821

Recode variables (10 points)
----------------------------

We want to create a new binary variable for having an account on social media so that 1 means "yes", 0 means "no", and all missing values are coded as NA. We also want to recode gender into a new variable with the values "male" and "female" (this can be a character vector or a factor).

``` r
# add your code here
Data$recode.i_ypsocweb <- ifelse(Data$i_ypsocweb == 1, 1,                     # Recode social media account ownership
                                 ifelse(Data$i_ypsocweb == 2, 0, NA))
Data$recode.i_ypsex <- as.factor(ifelse(Data$i_ypsex == 1, "male", "female")) # Recode sex
```

Calculate means (10 points)
---------------------------

Produce code that calculates probabilities of having an account on social media (i.e. the mean of your new binary variable produced in the previous problem) by age and gender.

``` r
# add your code here
with(Data, tapply(recode.i_ypsocweb, list(i_age_dv, recode.i_ypsex), mean, na.rm = TRUE))
```

    ##       female      male
    ## 9  0.0000000        NA
    ## 10 0.5330396 0.4415584
    ## 11 0.7617188 0.6317992
    ## 12 0.8761468 0.8559671
    ## 13 0.9352227 0.8920188
    ## 14 0.9578059 0.9365079
    ## 15 0.9860465 0.9315068
    ## 16 1.0000000 1.0000000

Write short interpretation (10 points)
--------------------------------------

Write two or three sentences interpreting your findings above.

A: It appears that almost half of all children in the UK begin creating social media accounts from the age of 10, with a majority of both boys and girls owning at least one account from age 11. Girls usually create their first social media account before boys, but this disparity equalises by the time the children become teenagers. However, a slight gap re-emerges at the age of 14-15, when boys' ownership rates flatten while girls' continue increasing. Finally, 100% of teenagers aged 16 or older indicated that they owned at least one social media account, though the sample size for this group is too small from which to draw meaningful conclusions.

Visualise results (20 points)
-----------------------------

Create a statistical graph (only one, but it can be faceted) illustrating your results (i.e. showing how the probability of having an account on social media changes with age and gender). Which type of statistical graph would be most appropriate for this?

``` r
# add your code here
gg1 <- Data[, c(14:15,177,218)]
gg1$prop <- ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==9, 0, # Brute force assignment because I could not figure out a smarter, faster way to achieve the same result.
                   ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==9, 0,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==16, 1,
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==16, 1,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==10, 0.4415584, 
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==10, 0.5330396,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==11, 0.6317992,
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==11, 0.7617188,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==12, 0.8559671,
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==12, 0.8761468,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==13, 0.8920188, 
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==13, 0.9352227,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==14, 0.9365079,
                          ifelse(gg1$i_ypsex==2 & gg1$i_age_dv==14, 0.9578059,
                          ifelse(gg1$i_ypsex==1 & gg1$i_age_dv==15, 0.9315068, 0.9860465))))))))))))))) 
gg1.1 <- subset(gg1, subset = recode.i_ypsex=="male") # Subset by sex in preparation for generating graph
gg1.1$Male <- gg1.1$prop                              # Column title must be the desired legend label for ggplot2
gg1.2 <- subset(gg1, subset = recode.i_ypsex=="female")
gg1.2$Female <- gg1.2$prop
ggplot(gg1, aes(x=i_age_dv)) +
  geom_line(data = gg1.2, aes(y=Female, color = "Female")) +
  geom_line(data = gg1.1, aes(y=Male, color = "Male")) +
  scale_color_manual(values = c('Female' = 'red',
                                'Male' = 'darkblue')) +
  ggtitle("Proportion of children who have at least one social media account, by age and sex") +
  labs(x="Age", y="Proportion", color="Sex")
```

![](testAssignment_files/figure-markdown_github/unnamed-chunk-5-1.png)

Conclusion
----------

This is a test formative assignment and the mark will not count towards your final mark. If you cannot answer any of the questions above this is fine -- we are just starting this module! However, please do submit this assignment in any case to make sure that you understand the procedure, that it works correctly and you do not have any problems with summative assignments later.
