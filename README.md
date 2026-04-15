
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
str(av)
```

    ## 'data.frame':    173 obs. of  21 variables:
    ##  $ URL                        : chr  "http://marvel.wikia.com/Henry_Pym_(Earth-616)" "http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)" "http://marvel.wikia.com/Anthony_Stark_(Earth-616)" "http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)" ...
    ##  $ Name.Alias                 : chr  "Henry Jonathan \"Hank\" Pym" "Janet van Dyne" "Anthony Edward \"Tony\" Stark" "Robert Bruce Banner" ...
    ##  $ Appearances                : int  1269 1165 3068 2089 2402 612 3458 1456 769 1214 ...
    ##  $ Current.                   : chr  "YES" "YES" "YES" "YES" ...
    ##  $ Gender                     : chr  "MALE" "FEMALE" "MALE" "MALE" ...
    ##  $ Probationary.Introl        : chr  "" "" "" "" ...
    ##  $ Full.Reserve.Avengers.Intro: chr  "Sep-63" "Sep-63" "Sep-63" "Sep-63" ...
    ##  $ Year                       : int  1963 1963 1963 1963 1963 1963 1964 1965 1965 1965 ...
    ##  $ Years.since.joining        : int  52 52 52 52 52 52 51 50 50 50 ...
    ##  $ Honorary                   : chr  "Full" "Full" "Full" "Full" ...
    ##  $ Death1                     : chr  "YES" "YES" "YES" "YES" ...
    ##  $ Return1                    : chr  "NO" "YES" "YES" "YES" ...
    ##  $ Death2                     : chr  "" "" "" "" ...
    ##  $ Return2                    : chr  "" "" "" "" ...
    ##  $ Death3                     : chr  "" "" "" "" ...
    ##  $ Return3                    : chr  "" "" "" "" ...
    ##  $ Death4                     : chr  "" "" "" "" ...
    ##  $ Return4                    : chr  "" "" "" "" ...
    ##  $ Death5                     : chr  "" "" "" "" ...
    ##  $ Return5                    : chr  "" "" "" "" ...
    ##  $ Notes                      : chr  "Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. " "Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered" "Death: \"Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'"| __truncated__ "Dies in Ghosts of the Future arc. However \"he had actually used a hidden Pantheon base to survive\"" ...

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.0     ✔ readr     2.1.6
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.2     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av %>%
  pivot_longer(cols = Death1:Death5, names_to = "Time", values_to = "Death") %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Death != "")

deaths
```

    ## # A tibble: 282 × 14
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  4 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  5 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 272 more rows
    ## # ℹ 8 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return5 <chr>, Notes <chr>,
    ## #   Time <dbl>, Death <chr>

- Dominic’s note: A ton of double entries, but that should be fine
  because they don’t really conflict and many characters come back or
  die several times

Similarly, deal with the returns of characters.

``` r
returns <- av %>%
  pivot_longer(cols = Return1:Return5, names_to = "Time", values_to = "Return") %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Return != "")

returns
```

    ## # A tibble: 110 × 14
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  3 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ## # ℹ 100 more rows
    ## # ℹ 8 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Notes <chr>,
    ## #   Time <dbl>, Return <chr>

- Dominic’s note: similar to deaths, there are a ton of characters that
  will duplicate due to dying and returning more than once

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(Name.Alias) %>%
  summarise(num_deaths = n()) %>%
  summarise(avg_deaths = mean(num_deaths))

avg_deaths
```

    ## # A tibble: 1 × 1
    ##   avg_deaths
    ##        <dbl>
    ## 1       2.27

- Dominic’s notes: averaging 2.3 deaths per avenger is pretty good. Not
  super human though.

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
