Test statistical assignment
================
Thea Smith
27 January 2020

## Introduction

Please change the author and date fields above as appropriate. Do not
change the output format. Once you have completed the assignment you
want to knit your document into a markdown document in the
“github\_document” format and then commit both the .Rmd and .md files
(and all the associated files with graphs) to your private assignment
repository on Github.

## Reading data (40 points)

First, we need to read the data into R. For this assignment, I ask you
to use data from the youth self-completion questionnaire (completed by
children between 10 and 15 years old) from Wave 9 of the Understanding
Society. It is one of the files you have downloaded as part of SN6614
from the UK Data Service. To help you find and understand this file you
will need the following documents:

1)  The Understanding Society Waves 1-9 User Guide:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/user-guides/mainstage-user-guide.pdf>
2)  The youth self-completion questionnaire from Wave 9:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/questionnaire/wave-9/w9-gb-youth-self-completion-questionnaire.pdf>
3)  The codebook for the file:
    <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/datafile/youth/wave/9>

<!-- end list -->

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.5.2

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.2.1     ✓ purrr   0.3.3
    ## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
    ## ✓ tidyr   1.0.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.4.0

    ## Warning: package 'ggplot2' was built under R version 3.5.2

    ## Warning: package 'tibble' was built under R version 3.5.2

    ## Warning: package 'tidyr' was built under R version 3.5.2

    ## Warning: package 'purrr' was built under R version 3.5.2

    ## Warning: package 'dplyr' was built under R version 3.5.2

    ## Warning: package 'stringr' was built under R version 3.5.2

    ## Warning: package 'forcats' was built under R version 3.5.2

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# This attaches the tidyverse package. If you get an error here you need to install the package first. 
youth <- read_tsv("/Users/TheaSmith/Desktop/Data Analysis III/UKDA-6614-tab/tab/ukhls_w9/i_youth.tab")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
# You need to add between the quotation marks a full path to the required file on your computer.
```

## Tabulate variables (10 points)

In the survey children were asked the following question: “Do you have a
social media profile or account on any sites or apps?”. In this
assignment we want to explore how the probability of having an account
on social media depends on children’s age and gender.

Tabulate three variables: children’s gender, age (please use derived
variables) and having an account on social media.

``` r
## social media yes or no = i_ypsocweb
## gender = i_sex_dv
## age = i_age_dv

table(youth$i_ypsocweb)
```

    ## 
    ##   -9    1    2 
    ##   14 2277  530

``` r
table(youth$i_ypsex)
```

    ## 
    ##    1    2 
    ## 1411 1410

``` r
table(youth$i_age_dv)
```

    ## 
    ##   9  10  11  12  13  14  15  16 
    ##   1 460 496 467 463 491 434   9

## Recode variables (10 points)

We want to create a new binary variable for having an account on social
media so that 1 means “yes”, 0 means “no”, and all missing values are
coded as NA. We also want to recode gender into a new variable with the
values “male” and “female” (this can be a character vector or a factor).

``` r
youth$socialmedia <- NA
youth$socialmedia[youth$i_ypsocweb == 1] <- 1
youth$socialmedia[youth$i_ypsocweb == 2] <- 0

table(youth$socialmedia)
```

    ## 
    ##    0    1 
    ##  530 2277

``` r
youth$gender <- NA
youth$gender[youth$i_sex_dv == 1] <- "male"
youth$gender[youth$i_sex_dv == 2] <- "female"

table(youth$gender)
```

    ## 
    ## female   male 
    ##   1408   1411

## Calculate means (10 points)

Produce code that calculates probabilities of having an account on
social media (i.e. the mean of your new binary variable produced in the
previous problem) by age and
    gender.

``` r
mean(youth$socialmedia, na.rm = TRUE)
```

    ## [1] 0.8111863

``` r
tapply(youth$socialmedia, youth$i_age_dv, mean, na.rm = TRUE)
```

    ##         9        10        11        12        13        14        15        16 
    ## 0.0000000 0.4868996 0.6989899 0.8655098 0.9152174 0.9468303 0.9585253 1.0000000

``` r
tapply(youth$socialmedia, youth$gender, mean, na.rm = TRUE)
```

    ##    female      male 
    ## 0.8395150 0.7826087

## Write short interpretation (10 points)

Write two or three sentences interpreting your findings above.

Just over 81 percent of the youth have social media. There is an
increase in social media usership with age, and even though 9 and 16 fit
with this trend, we should ignore these due to the small sample sizes.
Almost 84% of females have social media and just over 78% of males have
social media

## Visualise results (20 points)

Create a statistical graph (only one, but it can be faceted)
illustrating your results (i.e. showing how the probability of having an
account on social media changes with age and gender). Which type of
statistical graph would be most appropriate for
this?

``` r
YoungMedia <- tapply(youth$socialmedia, list(youth$gender, youth$i_age_dv), mean, na.rm = TRUE)

barplot(YoungMedia, 
        main = "Bar plot showing the proportion of young people using social media", 
        xlab = "Age",
        ylab = "Proportion of young people using social media", 
        legend = youth$gender[1:2], 
        beside = TRUE)
```

![](testAssignment_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Conclusion

This is a test formative assignment and the mark will not count towards
your final mark. If you cannot answer any of the questions above this is
fine – we are just starting this module\! However, please do submit this
assignment in any case to make sure that you understand the procedure,
that it works correctly and you do not have any problems with summative
assignments later.
