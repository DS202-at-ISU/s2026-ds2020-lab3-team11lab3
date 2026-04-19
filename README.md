
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

deaths <- av %>%
  select(URL, Name.Alias, Death1:Death5, Return1:Return5) %>%
  pivot_longer(
    cols = c(Death1:Death5, Return1:Return5),
    names_to = c(".value", "Time"),
    names_pattern = "(Death|Return)(\\d)"
  ) %>%
  mutate(Time = as.numeric(Time)) %>%
  filter(Death != "")
# pivoted the death and return columns for later, late edit

deaths
```

    ## # A tibble: 194 × 5
    ##    URL                                             Name.Alias  Time Death Return
    ##    <chr>                                           <chr>      <dbl> <chr> <chr> 
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)   "Henry Jo…     1 YES   "NO"  
    ##  2 http://marvel.wikia.com/Janet_van_Dyne_(Earth-… "Janet va…     1 YES   "YES" 
    ##  3 http://marvel.wikia.com/Anthony_Stark_(Earth-6… "Anthony …     1 YES   "YES" 
    ##  4 http://marvel.wikia.com/Robert_Bruce_Banner_(E… "Robert B…     1 YES   "YES" 
    ##  5 http://marvel.wikia.com/Thor_Odinson_(Earth-61… "Thor Odi…     1 YES   "YES" 
    ##  6 http://marvel.wikia.com/Thor_Odinson_(Earth-61… "Thor Odi…     2 YES   "NO"  
    ##  7 http://marvel.wikia.com/Richard_Jones_(Earth-6… "Richard …     1 NO    ""    
    ##  8 http://marvel.wikia.com/Steven_Rogers_(Earth-6… "Steven R…     1 YES   "YES" 
    ##  9 http://marvel.wikia.com/Clint_Barton_(Earth-61… "Clinton …     1 YES   "YES" 
    ## 10 http://marvel.wikia.com/Clint_Barton_(Earth-61… "Clinton …     2 YES   "YES" 
    ## # ℹ 184 more rows

- Dominic’s note: A ton of double entries, but that should be fine
  because they don’t really conflict and many characters come back or
  die several times

- Tanisha’s note: It makes sense that some names show up more than once
  here, since this table records each death event by time point instead
  of keeping only one row per character. Those repeated rows are useful
  because they track separate events rather than creating
  contradictions.

Similarly, deal with the returns of characters.

``` r
returns <- av %>%
  select(URL, Name.Alias, Return1:Return5) %>%
  pivot_longer(cols = Return1:Return5, names_to = "Time", values_to = "Return") %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Return != "")
# added a select, late edit

returns
```

    ## # A tibble: 110 × 4
    ##    URL                                                   Name.Alias  Time Return
    ##    <chr>                                                 <chr>      <dbl> <chr> 
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)         "Henry Jo…     1 NO    
    ##  2 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)    "Janet va…     1 YES   
    ##  3 http://marvel.wikia.com/Anthony_Stark_(Earth-616)     "Anthony …     1 YES   
    ##  4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-6… "Robert B…     1 YES   
    ##  5 http://marvel.wikia.com/Thor_Odinson_(Earth-616)      "Thor Odi…     1 YES   
    ##  6 http://marvel.wikia.com/Thor_Odinson_(Earth-616)      "Thor Odi…     2 YES   
    ##  7 http://marvel.wikia.com/Thor_Odinson_(Earth-616)      "Thor Odi…     2 NO    
    ##  8 http://marvel.wikia.com/Steven_Rogers_(Earth-616)     "Steven R…     1 YES   
    ##  9 http://marvel.wikia.com/Clint_Barton_(Earth-616)      "Clinton …     1 YES   
    ## 10 http://marvel.wikia.com/Clint_Barton_(Earth-616)      "Clinton …     2 YES   
    ## # ℹ 100 more rows

- Dominic’s note: similar to deaths, there are a ton of characters that
  will duplicate due to dying and returning more than once

- Tanisha’s note: The return data also includes repeated names, which is
  expected because several Avengers reappear after being gone more than
  one time. Keeping those repeated observations lets us follow each
  comeback across the different time values.

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
    ## 1       1.39

- Dominic’s notes: averaging 1.4 deaths per avenger is pretty good. Not
  super human though.

- Tanisha’s note: The mean comes out to about 1.4 deaths for the
  Avengers who died at least once, so most characters in this summary do
  not stay dead many times. That average suggests repeated deaths
  happen, but they are still fairly limited overall.

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

- Dominic’s statement from doc: “2-in-3 chance of returning from first
  death, but only 50% from a second or third”

- Tanisha’s note: Characters who return tend to die again.

### Include the code

Dominic’s code -

``` r
death_return_rates <- deaths %>%
  filter(Death == "YES") %>%
  group_by(Time) %>%
  summarise(
    total_deaths = n(),
    total_returns = sum(Return == "YES", na.rm = TRUE),
    return_rate = round(total_returns / total_deaths, 2)
  )


death_return_rates
```

    ## # A tibble: 5 × 4
    ##    Time total_deaths total_returns return_rate
    ##   <dbl>        <int>         <int>       <dbl>
    ## 1     1           69            46        0.67
    ## 2     2           16             8        0.5 
    ## 3     3            2             1        0.5 
    ## 4     4            1             1        1   
    ## 5     5            1             1        1

Tanisha’s code -

``` r
returned_chars <- returns %>%
  filter(Return == "YES") %>%
  distinct(Name.Alias)

repeat_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(Name.Alias) %>%
  summarise(num_deaths = n()) %>%
  mutate(died_multiple_times = num_deaths > 1)

analysis <- returned_chars %>%
  left_join(repeat_deaths, by = "Name.Alias") %>%
  summarise(
    total_returned = n(),
    multiple_deaths = sum(died_multiple_times, na.rm = TRUE),
    proportion = round(multiple_deaths / total_returned, 2)
  )

analysis
```

    ## # A tibble: 1 × 3
    ##   total_returned multiple_deaths proportion
    ##            <int>           <int>      <dbl>
    ## 1             45              16       0.36

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Dominic’s answer - Yeah he is exactly right that there is a “2-in-3
chance of returning from first death, but only 50% from a second or
third”. Looking at the total returns / total deaths shows that he was
pretty spot on. Even though the 3rd time a character has returned has a
sample size of 2, which I feel like is cheap.

Tanisha’s answer - The results partially support the statement. Out of
45 characters who returned at least once, 16 of them died multiple
times, giving a proportion of 0.36. This means that about 36% of
returning characters go on to die again, which suggests that repeat
deaths are somewhat common, but not the majority. Therefore, while the
statement is directionally true, it slightly overstates how often
returning characters die again.

Upload your changes to the repository. Discuss and refine answers as a
team.
