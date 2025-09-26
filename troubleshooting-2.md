Team Troubleshooting Deliverable 2
================

There are **11 code chunks with errors** in this Rmd. Your objective is
to fix all of the errors in this worksheet. For the purpose of grading,
each erroneous code chunk is equally weighted.

Note that errors are not all syntactic (i.e., broken code)! Some are
logical errors as well (i.e. code that does not do what it was intended
to do).

## Exercise 1: Exploring with `select()` and `filter()`

[MovieLens](https://dl.acm.org/doi/10.1145/2827872) are a series of
datasets widely used in education, that describe movie ratings from the
MovieLens [website](https://movielens.org/). There are several MovieLens
datasets, collected by the [GroupLens Research
Project](https://grouplens.org/datasets/movielens/) at the University of
Minnesota. Here, we load the MovieLens 100K dataset from Rafael Irizarry
and Amy Gill’s R package,
[dslabs](https://cran.r-project.org/web/packages/dslabs/dslabs.pdf),
which contains datasets useful for data analysis practice, homework, and
projects in data science courses and workshops. We’ll also load other
required packages.

``` r
### Fixed installing of packages (and commented them out) and loaded packages from library ###


#install.packages("dslabs")
library(dslabs)

#install.packages"("tidyverse")
library(tidyverse)

#install.packages("stringr") 
library(stringr)

#install.packages("devtools") # Do not run this if you already have this package installed! 
library(devtools)

#devtools::install_github("JoeyBernhardt/singer", force = TRUE)

#install.packages("gapminder")
library(gapminder)
```

Let’s have a look at the dataset! My goal is to:

- Find out the “class” of the dataset.
- If it isn’t a tibble already, coerce it into a tibble and store it in
  the variable “movieLens”.
- Have a quick look at the tibble, using a *dplyr function*.

``` r
### Changed the dim function (not a dplyr function) to glimpse so that we could see a quick look at the tibble ###

class(dslabs::movielens) 
```

    ## [1] "data.frame"

``` r
movieLens <- as_tibble(dslabs::movielens)
class(movieLens)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
glimpse(movieLens)
```

    ## Rows: 100,004
    ## Columns: 7
    ## $ movieId   <int> 31, 1029, 1061, 1129, 1172, 1263, 1287, 1293, 1339, 1343, 1371, 1405, 1953, 2105, 2150, 2193, 2294, 2455, 2968, 3671, 10, 17, 39, 4…
    ## $ title     <chr> "Dangerous Minds", "Dumbo", "Sleepers", "Escape from New York", "Cinema Paradiso (Nuovo cinema Paradiso)", "Deer Hunter, The", "Ben…
    ## $ year      <int> 1995, 1941, 1996, 1981, 1989, 1978, 1959, 1982, 1992, 1991, 1979, 1996, 1971, 1982, 1980, 1988, 1998, 1986, 1981, 1974, 1995, 1995,…
    ## $ genres    <fct> Drama, Animation|Children|Drama|Musical, Thriller, Action|Adventure|Sci-Fi|Thriller, Drama, Drama|War, Action|Adventure|Drama, Dram…
    ## $ userId    <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,…
    ## $ rating    <dbl> 2.5, 3.0, 3.0, 2.0, 4.0, 2.0, 2.0, 2.0, 3.5, 2.0, 2.5, 1.0, 4.0, 4.0, 3.0, 2.0, 2.0, 2.5, 1.0, 3.0, 4.0, 5.0, 5.0, 4.0, 4.0, 3.0, 3…
    ## $ timestamp <int> 1260759144, 1260759179, 1260759182, 1260759185, 1260759205, 1260759151, 1260759187, 1260759148, 1260759125, 1260759131, 1260759135,…

Now that we’ve had a quick look at the dataset, it would be interesting
to explore the rows (observations) in some more detail. I’d like to
consider the movie entries that…

- belong *exclusively* to the genre *“Drama”*;
- don’t belong *exclusively* to the genre *“Drama”*;
- were filmed *after* the year 2000;
- were filmed in 1999 *or* 2000;
- have *more than* 4.5 stars, and were filmed *before* 1995.

``` r
### Change month in filter #4 to use year instead. ###

filter(movieLens, genres == "Drama")
```

    ## # A tibble: 7,757 × 7
    ##    movieId title                                    year genres userId rating  timestamp
    ##      <int> <chr>                                   <int> <fct>   <int>  <dbl>      <int>
    ##  1      31 Dangerous Minds                          1995 Drama       1    2.5 1260759144
    ##  2    1172 Cinema Paradiso (Nuovo cinema Paradiso)  1989 Drama       1    4   1260759205
    ##  3    1293 Gandhi                                   1982 Drama       1    2   1260759148
    ##  4      62 Mr. Holland's Opus                       1995 Drama       2    3    835355749
    ##  5     261 Little Women                             1994 Drama       2    4    835355681
    ##  6     300 Quiz Show                                1994 Drama       2    3    835355532
    ##  7     508 Philadelphia                             1993 Drama       2    4    835355860
    ##  8     537 Sirens                                   1994 Drama       2    4    835356199
    ##  9    2702 Summer of Sam                            1999 Drama       3    3.5 1298861796
    ## 10    3949 Requiem for a Dream                      2000 Drama       3    5   1298863174
    ## # ℹ 7,747 more rows

``` r
filter(movieLens, !genres == "Drama")
```

    ## # A tibble: 92,247 × 7
    ##    movieId title                            year genres                           userId rating  timestamp
    ##      <int> <chr>                           <int> <fct>                             <int>  <dbl>      <int>
    ##  1    1029 Dumbo                            1941 Animation|Children|Drama|Musical      1    3   1260759179
    ##  2    1061 Sleepers                         1996 Thriller                              1    3   1260759182
    ##  3    1129 Escape from New York             1981 Action|Adventure|Sci-Fi|Thriller      1    2   1260759185
    ##  4    1263 Deer Hunter, The                 1978 Drama|War                             1    2   1260759151
    ##  5    1287 Ben-Hur                          1959 Action|Adventure|Drama                1    2   1260759187
    ##  6    1339 Dracula (Bram Stoker's Dracula)  1992 Fantasy|Horror|Romance|Thriller       1    3.5 1260759125
    ##  7    1343 Cape Fear                        1991 Thriller                              1    2   1260759131
    ##  8    1371 Star Trek: The Motion Picture    1979 Adventure|Sci-Fi                      1    2.5 1260759135
    ##  9    1405 Beavis and Butt-Head Do America  1996 Adventure|Animation|Comedy|Crime      1    1   1260759203
    ## 10    1953 French Connection, The           1971 Action|Crime|Thriller                 1    4   1260759191
    ## # ℹ 92,237 more rows

``` r
filter(movieLens, year > 2000)
```

    ## # A tibble: 25,481 × 7
    ##    movieId title                                           year genres                              userId rating  timestamp
    ##      <int> <chr>                                          <int> <fct>                                <int>  <dbl>      <int>
    ##  1    5349 Spider-Man                                      2002 Action|Adventure|Sci-Fi|Thriller         3    3   1298923266
    ##  2    5669 Bowling for Columbine                           2002 Documentary                              3    3.5 1298862672
    ##  3    6377 Finding Nemo                                    2003 Adventure|Animation|Children|Comedy      3    3   1298922080
    ##  4    7153 Lord of the Rings: The Return of the King, The  2003 Action|Adventure|Drama|Fantasy           3    2.5 1298921787
    ##  5    7361 Eternal Sunshine of the Spotless Mind           2004 Drama|Romance|Sci-Fi                     3    3   1298922065
    ##  6    8622 Fahrenheit 9/11                                 2004 Documentary                              3    3.5 1298861650
    ##  7    8636 Spider-Man 2                                    2004 Action|Adventure|Sci-Fi|IMAX             3    3   1298932766
    ##  8   44191 V for Vendetta                                  2006 Action|Sci-Fi|Thriller|IMAX              3    3.5 1298932740
    ##  9   48783 Flags of Our Fathers                            2006 Drama|War                                3    4.5 1298862361
    ## 10   50068 Letters from Iwo Jima                           2006 Drama|War                                3    4.5 1298862467
    ## # ℹ 25,471 more rows

``` r
filter(movieLens, year == 1999 | year == 2000)
```

    ## # A tibble: 9,088 × 7
    ##    movieId title                                      year genres                      userId rating  timestamp
    ##      <int> <chr>                                     <int> <fct>                        <int>  <dbl>      <int>
    ##  1    2694 Big Daddy                                  1999 Comedy                           3    3   1298862710
    ##  2    2702 Summer of Sam                              1999 Drama                            3    3.5 1298861796
    ##  3    2762 Sixth Sense, The                           1999 Drama|Horror|Mystery             3    3.5 1298922057
    ##  4    2841 Stir of Echoes                             1999 Horror|Mystery|Thriller          3    4   1298861733
    ##  5    2858 American Beauty                            1999 Drama|Romance                    3    4   1298921825
    ##  6    2959 Fight Club                                 1999 Action|Crime|Drama|Thriller      3    5   1298862874
    ##  7    3510 Frequency                                  2000 Drama|Thriller                   3    4   1298861633
    ##  8    3949 Requiem for a Dream                        2000 Drama                            3    5   1298863174
    ##  9   27369 Daria: Is It Fall Yet?                     2000 Animation|Comedy                 3    3.5 1298862555
    ## 10    2628 Star Wars: Episode I - The Phantom Menace  1999 Action|Adventure|Sci-Fi          4    5    949810582
    ## # ℹ 9,078 more rows

``` r
filter(movieLens, rating > 4.5, year < 1995)
```

    ## # A tibble: 8,386 × 7
    ##    movieId title                                                year genres                                  userId rating  timestamp
    ##      <int> <chr>                                               <int> <fct>                                    <int>  <dbl>      <int>
    ##  1     265 Like Water for Chocolate (Como agua para chocolate)  1992 Drama|Fantasy|Romance                        2      5  835355697
    ##  2     266 Legends of the Fall                                  1994 Drama|Romance|War|Western                    2      5  835355586
    ##  3     551 Nightmare Before Christmas, The                      1993 Animation|Children|Fantasy|Musical           2      5  835355767
    ##  4     589 Terminator 2: Judgment Day                           1991 Action|Sci-Fi                                2      5  835355697
    ##  5     590 Dances with Wolves                                   1990 Adventure|Drama|Western                      2      5  835355395
    ##  6     592 Batman                                               1989 Action|Crime|Thriller                        2      5  835355395
    ##  7     318 Shawshank Redemption, The                            1994 Crime|Drama                                  3      5 1298862121
    ##  8     356 Forrest Gump                                         1994 Comedy|Drama|Romance|War                     3      5 1298862167
    ##  9    1197 Princess Bride, The                                  1987 Action|Adventure|Comedy|Fantasy|Romance      3      5 1298932770
    ## 10     260 Star Wars: Episode IV - A New Hope                   1977 Action|Adventure|Sci-Fi                      4      5  949779042
    ## # ℹ 8,376 more rows

While filtering for *all movies that do not belong to the genre drama*
above, I noticed something interesting. I want to filter for the same
thing again, this time selecting variables **title and genres first,**
and then *everything else*. But I want to do this in a robust way, so
that (for example) if I end up changing `movieLens` to contain more or
less columns some time in the future, the code will still work. Hint:
there is a function to select “everything else”…

``` r
### Added everything() function to select other variables in a robust way ###
movieLens %>%
  filter(!genres == "Drama") %>%
  select(title, genres, everything())
```

    ## # A tibble: 92,247 × 7
    ##    title                           genres                           movieId  year userId rating  timestamp
    ##    <chr>                           <fct>                              <int> <int>  <int>  <dbl>      <int>
    ##  1 Dumbo                           Animation|Children|Drama|Musical    1029  1941      1    3   1260759179
    ##  2 Sleepers                        Thriller                            1061  1996      1    3   1260759182
    ##  3 Escape from New York            Action|Adventure|Sci-Fi|Thriller    1129  1981      1    2   1260759185
    ##  4 Deer Hunter, The                Drama|War                           1263  1978      1    2   1260759151
    ##  5 Ben-Hur                         Action|Adventure|Drama              1287  1959      1    2   1260759187
    ##  6 Dracula (Bram Stoker's Dracula) Fantasy|Horror|Romance|Thriller     1339  1992      1    3.5 1260759125
    ##  7 Cape Fear                       Thriller                            1343  1991      1    2   1260759131
    ##  8 Star Trek: The Motion Picture   Adventure|Sci-Fi                    1371  1979      1    2.5 1260759135
    ##  9 Beavis and Butt-Head Do America Adventure|Animation|Comedy|Crime    1405  1996      1    1   1260759203
    ## 10 French Connection, The          Action|Crime|Thriller               1953  1971      1    4   1260759191
    ## # ℹ 92,237 more rows

## Exercise 2: Calculating with `mutate()`-like functions

Some of the variables in the `movieLens` dataset are in *camelCase* (in
fact, *movieLens* is in camelCase). Let’s clean these two variables to
use *snake_case* instead, and assign our post-rename object back to
“movieLens”.

``` r
### Modified to snake case by adding underscore "_" to movieLens and making lower case. Also removed a "=" from the rename function ###
movie_lens <- movieLens %>%
  rename(user_id = userId,
         movie_id = movieId)
```

As you already know, `mutate()` defines and inserts new variables into a
tibble. There is *another mystery function similar to `mutate()`* that
adds the new variable, but also drops existing ones. I wanted to create
an `average_rating` column that takes the `mean(rating)` across all
entries, and I only want to see that variable (i.e drop all others!) but
I forgot what that mystery function is. Can you remember?

``` r
### Modified the function to be transmute instead of mutate ### 
transmute(movie_lens,
       average_rating = mean(rating))
```

    ## # A tibble: 100,004 × 1
    ##    average_rating
    ##             <dbl>
    ##  1           3.54
    ##  2           3.54
    ##  3           3.54
    ##  4           3.54
    ##  5           3.54
    ##  6           3.54
    ##  7           3.54
    ##  8           3.54
    ##  9           3.54
    ## 10           3.54
    ## # ℹ 99,994 more rows

## Exercise 3: Calculating with `summarise()`-like functions

Alone, `tally()` is a short form of `summarise()`. `count()` is
short-hand for `group_by()` and `tally()`.

Each entry of the movieLens table corresponds to a movie rating by a
user. Therefore, if more than one user rated the same movie, there will
be several entries for the same movie. I want to find out how many times
each movie has been reviewed, or in other words, how many times each
movie title appears in the dataset.

``` r
movieLens %>%
  group_by(title) %>%
  tally()
```

    ## # A tibble: 8,832 × 2
    ##    title                                  n
    ##    <chr>                              <int>
    ##  1 "\"Great Performances\" Cats"          2
    ##  2 "$9.99"                                3
    ##  3 "'Hellboy': The Seeds of Creation"     1
    ##  4 "'Neath the Arizona Skies"             1
    ##  5 "'Round Midnight"                      2
    ##  6 "'Salem's Lot"                         1
    ##  7 "'Til There Was You"                   4
    ##  8 "'burbs, The"                         19
    ##  9 "'night Mother"                        3
    ## 10 "(500) Days of Summer"                45
    ## # ℹ 8,822 more rows

Without using `group_by()`, I want to find out how many movie reviews
there have been for each year.

``` r
### changed tally(year) to count(title,year) ###
movieLens %>%
  count(title,year)
```

    ## # A tibble: 9,060 × 3
    ##    title                               year     n
    ##    <chr>                              <int> <int>
    ##  1 "\"Great Performances\" Cats"       1998     2
    ##  2 "$9.99"                             2008     3
    ##  3 "'Hellboy': The Seeds of Creation"  2004     1
    ##  4 "'Neath the Arizona Skies"          1934     1
    ##  5 "'Round Midnight"                   1986     2
    ##  6 "'Salem's Lot"                      2004     1
    ##  7 "'Til There Was You"                1997     4
    ##  8 "'burbs, The"                       1989    19
    ##  9 "'night Mother"                     1986     3
    ## 10 "(500) Days of Summer"              2009    45
    ## # ℹ 9,050 more rows

Both `count()` and `tally()` can be grouped by multiple columns. Below,
I want to count the number of movie reviews by title and rating, and
sort the results.

``` r
### removed the c() ###
movieLens %>%
  count(title, rating, sort = TRUE)
```

    ## # A tibble: 28,297 × 3
    ##    title                              rating     n
    ##    <chr>                               <dbl> <int>
    ##  1 Shawshank Redemption, The               5   170
    ##  2 Pulp Fiction                            5   138
    ##  3 Star Wars: Episode IV - A New Hope      5   122
    ##  4 Forrest Gump                            4   113
    ##  5 Schindler's List                        5   109
    ##  6 Godfather, The                          5   107
    ##  7 Forrest Gump                            5   102
    ##  8 Silence of the Lambs, The               4   102
    ##  9 Fargo                                   5   100
    ## 10 Silence of the Lambs, The               5   100
    ## # ℹ 28,287 more rows

Not only do `count()` and `tally()` quickly allow you to count items
within your dataset, `add_tally()` and `add_count()` are handy shortcuts
that add an additional columns to your tibble, rather than collapsing
each group.

## Exercise 4: Calculating with `group_by()`

We can calculate the mean rating by year, and store it in a new column
called `avg_rating`:

``` r
movieLens %>%
  group_by(year) %>%
  summarize(avg_rating = mean(rating))
```

    ## # A tibble: 104 × 2
    ##     year avg_rating
    ##    <int>      <dbl>
    ##  1  1902       4.33
    ##  2  1915       3   
    ##  3  1916       3.5 
    ##  4  1917       4.25
    ##  5  1918       4.25
    ##  6  1919       3   
    ##  7  1920       3.7 
    ##  8  1921       4.42
    ##  9  1922       3.80
    ## 10  1923       4.17
    ## # ℹ 94 more rows

Using `summarize()`, we can find the minimum and the maximum rating by
title, stored under columns named `min_rating`, and `max_rating`,
respectively.

``` r
### The error here was that the movie ratings were not grouped by the title, so the min/max ratings were based on the min/max in the entire data set rather than the title ###
movieLens %>%
  group_by(title) %>%
  mutate(min_rating = min(rating), 
         max_rating = max(rating))
```

    ## # A tibble: 100,004 × 9
    ## # Groups:   title [8,832]
    ##    movieId title                                    year genres                           userId rating  timestamp min_rating max_rating
    ##      <int> <chr>                                   <int> <fct>                             <int>  <dbl>      <int>      <dbl>      <dbl>
    ##  1      31 Dangerous Minds                          1995 Drama                                 1    2.5 1260759144        1.5          5
    ##  2    1029 Dumbo                                    1941 Animation|Children|Drama|Musical      1    3   1260759179        1.5          5
    ##  3    1061 Sleepers                                 1996 Thriller                              1    3   1260759182        2            5
    ##  4    1129 Escape from New York                     1981 Action|Adventure|Sci-Fi|Thriller      1    2   1260759185        1            5
    ##  5    1172 Cinema Paradiso (Nuovo cinema Paradiso)  1989 Drama                                 1    4   1260759205        2            5
    ##  6    1263 Deer Hunter, The                         1978 Drama|War                             1    2   1260759151        0.5          5
    ##  7    1287 Ben-Hur                                  1959 Action|Adventure|Drama                1    2   1260759187        0.5          5
    ##  8    1293 Gandhi                                   1982 Drama                                 1    2   1260759148        1.5          5
    ##  9    1339 Dracula (Bram Stoker's Dracula)          1992 Fantasy|Horror|Romance|Thriller       1    3.5 1260759125        1            5
    ## 10    1343 Cape Fear                                1991 Thriller                              1    2   1260759131        2            5
    ## # ℹ 99,994 more rows

## Exercise 5: Scoped variants with `across()`

`across()` is a newer dplyr function (`dplyr` 1.0.0) that allows you to
apply a transformation to multiple variables selected with the
`select()` and `rename()` syntax. For this section, we will use the
`starwars` dataset, which is built into R. First, let’s transform it
into a tibble and store it under the variable `starWars`.

``` r
starWars <- as_tibble(starwars)
```

We can find the mean for all columns that are numeric, ignoring the
missing values:

``` r
starWars %>%
  summarise(across(where(is.numeric), function(x) mean(x, na.rm=TRUE)))
```

    ## # A tibble: 1 × 3
    ##   height  mass birth_year
    ##    <dbl> <dbl>      <dbl>
    ## 1   175.  97.3       87.6

We can find the minimum height and mass within each species, ignoring
the missing values:

``` r
### Modified the summarise() function to group the height and mass columns by using c() ###
starWars %>%
  group_by(species) %>%
  summarise(across(c("height", "mass"), function(x) min(x, na.rm=TRUE)))
```

    ## Warning: There were 6 warnings in `summarise()`.
    ## The first warning was:
    ## ℹ In argument: `across(c("height", "mass"), function(x) min(x, na.rm = TRUE))`.
    ## ℹ In group 4: `species = "Chagrian"`.
    ## Caused by warning in `min()`:
    ## ! no non-missing arguments to min; returning Inf
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 5 remaining warnings.

    ## # A tibble: 38 × 3
    ##    species   height  mass
    ##    <chr>      <int> <dbl>
    ##  1 Aleena        79    15
    ##  2 Besalisk     198   102
    ##  3 Cerean       198    82
    ##  4 Chagrian     196   Inf
    ##  5 Clawdite     168    55
    ##  6 Droid         96    32
    ##  7 Dug          112    40
    ##  8 Ewok          88    20
    ##  9 Geonosian    183    80
    ## 10 Gungan       196    66
    ## # ℹ 28 more rows

Note that here R has taken the convention that the minimum value of a
set of `NA`s is `Inf`.

## Exercise 6: Making tibbles

Manually create a tibble with 4 columns:

- `birth_year` should contain years 1998 to 2005 (inclusive);
- `birth_weight` should take the `birth_year` column, subtract 1995, and
  multiply by 0.45;
- `birth_location` should contain three locations (Liverpool, Seattle,
  and New York).

``` r
### added a comma after birth_location and added quotations to multi-string words like Liverpool, England ###

fakeStarWars <- tribble(
  ~name,            ~birth_weight,  ~birth_year, ~birth_location,
  "Luke Skywalker",  1.35      ,   1998        ,  "Liverpool, England",
  "C-3PO"         ,  1.80      ,   1999        ,  "Liverpool, England",
  "R2-D2"         ,  2.25      ,   2000        ,  "Seattle, WA",
  "Darth Vader"   ,  2.70      ,   2001        ,  "Liverpool, England",
  "Leia Organa"   ,  3.15      ,   2002        ,  "New York, NY",
  "Owen Lars"     ,  3.60      ,   2003        ,  "Seattle, WA",
  "Beru Whitesun Iars", 4.05   ,   2004        ,  "Liverpool, England",
  "R5-D4"         ,  4.50      ,   2005        ,  "New York, NY",
)
```

## Attributions

Thanks to Icíar Fernández-Boyano for writing most of this document, and
Albina Gibadullina, Diana Lin, Yulia Egorova, and Vincenzo Coia for
their edits.
