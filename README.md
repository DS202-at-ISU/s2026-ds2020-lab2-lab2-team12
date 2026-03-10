
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#1

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

``` r
library(classdata)
head(ames)
```

    ##    Parcel ID                       Address             Style
    ## 1 0903202160      1024 RIDGEWOOD AVE, AMES 1 1/2 Story Frame
    ## 2 0907428215 4503 TWAIN CIR UNIT 105, AMES     1 Story Frame
    ## 3 0909428070        2030 MCCARTHY RD, AMES     1 Story Frame
    ## 4 0923203160         3404 EMERALD DR, AMES     1 Story Frame
    ## 5 0520440010       4507 EVEREST  AVE, AMES              <NA>
    ## 6 0907275030       4512 HEMINGWAY DR, AMES     2 Story Frame
    ##                        Occupancy  Sale Date Sale Price Multi Sale YearBuilt
    ## 1 Single-Family / Owner Occupied 2022-08-12     181900       <NA>      1940
    ## 2                    Condominium 2022-08-04     127100       <NA>      2006
    ## 3 Single-Family / Owner Occupied 2022-08-15          0       <NA>      1951
    ## 4                      Townhouse 2022-08-09     245000       <NA>      1997
    ## 5                           <NA> 2022-08-03     449664       <NA>        NA
    ## 6 Single-Family / Owner Occupied 2022-08-16     368000       <NA>      1996
    ##   Acres TotalLivingArea (sf) Bedrooms FinishedBsmtArea (sf) LotArea(sf)  AC
    ## 1 0.109                 1030        2                    NA        4740 Yes
    ## 2 0.027                  771        1                    NA        1181 Yes
    ## 3 0.321                 1456        3                  1261       14000 Yes
    ## 4 0.103                 1289        4                   890        4500 Yes
    ## 5 0.287                   NA       NA                    NA       12493  No
    ## 6 0.494                 2223        4                    NA       21533 Yes
    ##   FirePlace              Neighborhood
    ## 1       Yes       (28) Res: Brookside
    ## 2        No    (55) Res: Dakota Ridge
    ## 3        No        (32) Res: Crawford
    ## 4        No        (31) Res: Mitchell
    ## 5        No (19) Res: North Ridge Hei
    ## 6       Yes   (37) Res: College Creek

1.  What variables are there?

There are 16 total variables for each house

Parcel ID: House mailing number? unique 10 digit number Address: House
address Style: Building style (1 Story Frame, 2 Story Frame, etc.)
Occupancy: Type of housing (Single-Family/Owner Occupied, Condominium,
Townhouse, etc.) Sale Date: Date the house was sold (date in yyyy-mm-dd)
Sale Price: Sale price of house (\$) Multi Sale: N/A or Y YearBuilt:
Year of construction Acres: Land TotalLivingArea: House squarefootage
(0-4000?) Bedrooms: (NA - 8) FinishedBsmtArea: Extra sqft if basement
finished (NA - 2000) LotArea: sqft of lot (NA - 200000) AC: House air
conditioned (Yes / No) Fireplace: (Y / N) Neighborhood: House location

2.  Is there a variable of special interest? Yes, “Sale Price”

3.  Sale Price Exploration

Range: 0 - 20,500,000

``` r
summary(ames$ 'Sale Price')
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##        0        0   170900  1017479   280000 20500000

``` r
library(ggplot2)
ggplot(ames, aes(x = `Sale Price`)) + 
  geom_histogram(binwidth = 100000, color = "white", fill = "blue") + 
  labs(title = "House Sale Prices in Ames",
       x = "Sale Price ($)",
       y = "Number of times")
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- --> The histogram
is heavily right skewed with many extreme outliers.

4.  Acres (Will):

``` r
summary(ames$ 'Acres')
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##  0.0000  0.1502  0.2200  0.2631  0.2770 12.0120      89

``` r
library(ggplot2)
ggplot(ames, aes(x = `Acres`)) + 
  geom_histogram(binwidth = .05, color = "white", fill = "blue") + 
  labs(title = "Acre lot size in Ames",
       x = "Acres",
       y = "Number")
```

    ## Warning: Removed 89 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- --> The pattern
for the histogram is right skewed with some extreme outliers. This
pattern is very similar to the main variable Sale Price in its right
skew shape.

``` r
ggplot(ames, aes(x = Acres, y = `Sale Price`)) +
  geom_point(alpha = 0.3)  + xlim(0, 5) + ylim(0, 1000000) +
  labs(title = "Relationship Between Lot Size and Sale Price",
       x = "Acres",
       y = "Sale Price ($)")
```

    ## Warning: Removed 469 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- --> On this graph
I noticed, similar to the histograms, a large density of points in the
bottom left around the origin. This is a weak-positive relationship with
many outliers. It seems that generally higher priced homes have more
acreage, but some oddities are the houses with high acreage but low
price and vice-versa.

4.TotalLivingArea (Blake)

``` r
summary(ames$ 'TotalLivingArea (sf)')
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##       0    1095    1460    1507    1792    6007     447

``` r
library(ggplot2)
ggplot(ames, aes(x = `TotalLivingArea (sf)`)) + 
  geom_histogram(binwidth = 500, color = "white", fill = "blue") + 
  labs(title = "House Living Area Ames",
       x = "Living Area (sqft)",
       y = "Number of homes")
```

    ## Warning: Removed 447 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- --> Range: 0-6007
The data is very slightly right skewed

``` r
ggplot(ames, aes(x = `TotalLivingArea (sf)`, y = `Sale Price`)) +
  geom_point(alpha = 0.3, color = "blue") + xlim(1, 6000) + ylim(1, 2000000)
```

    ## Warning: Removed 2980 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
  labs(title = "Relationship: Living Area vs. Sale Price",
       x = "Living Area (sqft)",
       y = "Sale Price ($)")
```

    ## <ggplot2::labels> List of 3
    ##  $ x    : chr "Living Area (sqft)"
    ##  $ y    : chr "Sale Price ($)"
    ##  $ title: chr "Relationship: Living Area vs. Sale Price"

There is a strong positive relationship between TotalLivingArea (sf) and
Sale Price of the homes in Ames. There are many outliers in this data
set so they had to be removed.
