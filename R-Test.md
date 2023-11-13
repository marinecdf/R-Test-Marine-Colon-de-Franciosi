# R-Test
Marine de Franciosi
2023-11-09

# Loading packages and data

``` r
pacman::p_load(tidyverse, readr, waldo,
               skimr, data.table, matrixStats, 
               labelled, testit, ggthemes, purrr, 
               kableExtra, knitr)
```

Loading dataset as instructed:

``` r
tag      <- "202311081903"
base_url <- "https://github.com/randrescastaneda/pub_data/raw/"
data_url <- paste0(base_url, tag, "/data/Rtest1/")

wdi <- readr::read_rds(paste0(data_url, "wdi_in1.Rds"))

#install.packages("DescTools")
# first taking a glimpse at the dataset: 
dim(wdi) # number of observations and variables 
```

    [1] 5029   12

``` r
glimpse(wdi)
```

    Rows: 5,029
    Columns: 12
    $ region   <chr> "Sub-Saharan Africa", "Sub-Saharan Africa", "Sub-Saharan Afri…
    $ iso3c    <chr> "AGO", "AGO", "AGO", "AGO", "AGO", "AGO", "AGO", "AGO", "AGO"…
    $ date     <dbl> 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2…
    $ country  <chr> "Angola", "Angola", "Angola", "Angola", "Angola", "Angola", "…
    $ pov_ofcl <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ gdp      <dbl> 5793.085, 5659.119, 5158.384, 3799.195, 3728.886, 4149.446, 4…
    $ gini     <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, 52.0, NA, NA, NA, NA,…
    $ lifeex   <dbl> 41.893, 43.813, 42.209, 42.101, 43.422, 45.849, 46.033, 46.30…
    $ pop      <dbl> 11828638, 12228691, 12632507, 13038270, 13462031, 13912253, 1…
    $ pov_intl <dbl> 0.1652797, 0.1680163, 0.1919029, 0.2736178, 0.2789797, 0.2487…
    $ pov_lmic <dbl> 0.3093024, 0.3142586, 0.3537655, 0.4874785, 0.4950096, 0.4519…
    $ pov_umic <dbl> 0.5843191, 0.5963407, 0.6382768, 0.7609357, 0.7659570, 0.7248…

``` r
skimr::skim(wdi)
```

<table style="width: auto;" class="table table-condensed">
<caption>
Data summary
</caption>
<tbody>
<tr>
<td style="text-align:left;">
Name
</td>
<td style="text-align:left;">
wdi
</td>
</tr>
<tr>
<td style="text-align:left;">
Number of rows
</td>
<td style="text-align:left;">
5029
</td>
</tr>
<tr>
<td style="text-align:left;">
Number of columns
</td>
<td style="text-align:left;">
12
</td>
</tr>
<tr>
<td style="text-align:left;">
Key
</td>
<td style="text-align:left;">
iso3c, date
</td>
</tr>
<tr>
<td style="text-align:left;">
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
</td>
<td style="text-align:left;">
</td>
</tr>
<tr>
<td style="text-align:left;">
Column type frequency:
</td>
<td style="text-align:left;">
</td>
</tr>
<tr>
<td style="text-align:left;">
character
</td>
<td style="text-align:left;">
3
</td>
</tr>
<tr>
<td style="text-align:left;">
numeric
</td>
<td style="text-align:left;">
9
</td>
</tr>
<tr>
<td style="text-align:left;">
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
</td>
<td style="text-align:left;">
</td>
</tr>
<tr>
<td style="text-align:left;">
Group variables
</td>
<td style="text-align:left;">
None
</td>
</tr>
</tbody>
</table>

**Variable type: character**

<table>
<thead>
<tr>
<th style="text-align:left;">
skim_variable
</th>
<th style="text-align:right;">
n_missing
</th>
<th style="text-align:right;">
complete_rate
</th>
<th style="text-align:right;">
min
</th>
<th style="text-align:right;">
max
</th>
<th style="text-align:right;">
empty
</th>
<th style="text-align:right;">
n_unique
</th>
<th style="text-align:right;">
whitespace
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
region
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
26
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
iso3c
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
165
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;">
country
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
165
</td>
<td style="text-align:right;">
0
</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<thead>
<tr>
<th style="text-align:left;">
skim_variable
</th>
<th style="text-align:right;">
n_missing
</th>
<th style="text-align:right;">
complete_rate
</th>
<th style="text-align:right;">
mean
</th>
<th style="text-align:right;">
sd
</th>
<th style="text-align:right;">
p0
</th>
<th style="text-align:right;">
p25
</th>
<th style="text-align:right;">
p50
</th>
<th style="text-align:right;">
p75
</th>
<th style="text-align:right;">
p100
</th>
<th style="text-align:left;">
hist
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
date
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
2005.00
</td>
<td style="text-align:right;">
8.90
</td>
<td style="text-align:right;">
1990.00
</td>
<td style="text-align:right;">
1997.00
</td>
<td style="text-align:right;">
2005.00
</td>
<td style="text-align:right;">
2013.00
</td>
<td style="text-align:right;">
2.021000e+03
</td>
<td style="text-align:left;">
▇▇▇▇▇
</td>
</tr>
<tr>
<td style="text-align:left;">
pov_ofcl
</td>
<td style="text-align:right;">
4851
</td>
<td style="text-align:right;">
0.04
</td>
<td style="text-align:right;">
37.54
</td>
<td style="text-align:right;">
14.94
</td>
<td style="text-align:right;">
7.40
</td>
<td style="text-align:right;">
25.36
</td>
<td style="text-align:right;">
36.80
</td>
<td style="text-align:right;">
48.38
</td>
<td style="text-align:right;">
6.650000e+01
</td>
<td style="text-align:left;">
▃▆▇▅▅
</td>
</tr>
<tr>
<td style="text-align:left;">
gdp
</td>
<td style="text-align:right;">
217
</td>
<td style="text-align:right;">
0.96
</td>
<td style="text-align:right;">
14860.03
</td>
<td style="text-align:right;">
17242.33
</td>
<td style="text-align:right;">
436.38
</td>
<td style="text-align:right;">
3292.98
</td>
<td style="text-align:right;">
8498.03
</td>
<td style="text-align:right;">
18835.49
</td>
<td style="text-align:right;">
1.206478e+05
</td>
<td style="text-align:left;">
▇▂▁▁▁
</td>
</tr>
<tr>
<td style="text-align:left;">
gini
</td>
<td style="text-align:right;">
3225
</td>
<td style="text-align:right;">
0.36
</td>
<td style="text-align:right;">
37.79
</td>
<td style="text-align:right;">
8.83
</td>
<td style="text-align:right;">
20.70
</td>
<td style="text-align:right;">
31.17
</td>
<td style="text-align:right;">
35.70
</td>
<td style="text-align:right;">
42.92
</td>
<td style="text-align:right;">
6.580000e+01
</td>
<td style="text-align:left;">
▃▇▅▂▁
</td>
</tr>
<tr>
<td style="text-align:left;">
lifeex
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
67.92
</td>
<td style="text-align:right;">
9.51
</td>
<td style="text-align:right;">
14.10
</td>
<td style="text-align:right;">
61.89
</td>
<td style="text-align:right;">
69.85
</td>
<td style="text-align:right;">
75.00
</td>
<td style="text-align:right;">
8.456000e+01
</td>
<td style="text-align:left;">
▁▁▂▇▇
</td>
</tr>
<tr>
<td style="text-align:left;">
pop
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
39254767.21
</td>
<td style="text-align:right;">
141141150\.25
</td>
<td style="text-align:right;">
9182.00
</td>
<td style="text-align:right;">
2581242.00
</td>
<td style="text-align:right;">
8321496.00
</td>
<td style="text-align:right;">
25501941.00
</td>
<td style="text-align:right;">
1.411100e+09
</td>
<td style="text-align:left;">
▇▁▁▁▁
</td>
</tr>
<tr>
<td style="text-align:left;">
pov_intl
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
0.18
</td>
<td style="text-align:right;">
0.23
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.01
</td>
<td style="text-align:right;">
0.06
</td>
<td style="text-align:right;">
0.28
</td>
<td style="text-align:right;">
9.500000e-01
</td>
<td style="text-align:left;">
▇▂▁▁▁
</td>
</tr>
<tr>
<td style="text-align:left;">
pov_lmic
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
0.32
</td>
<td style="text-align:right;">
0.31
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.02
</td>
<td style="text-align:right;">
0.20
</td>
<td style="text-align:right;">
0.58
</td>
<td style="text-align:right;">
9.900000e-01
</td>
<td style="text-align:left;">
▇▂▂▂▂
</td>
</tr>
<tr>
<td style="text-align:left;">
pov_umic
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.00
</td>
<td style="text-align:right;">
0.50
</td>
<td style="text-align:right;">
0.36
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.12
</td>
<td style="text-align:right;">
0.54
</td>
<td style="text-align:right;">
0.86
</td>
<td style="text-align:right;">
1.000000e+00
</td>
<td style="text-align:left;">
▇▃▃▅▇
</td>
</tr>
</tbody>
</table>

``` r
# from a glance at the dataset, it would seem country and date uniquely identify observations
```

# Basic Stats

## 1. Replicating Summary Table of GDP per capita by region (from 1990 to 2021)

``` r
# Let's summarize the wdi dataset and display main statistics for the GDP per capita variable:
# statistics should be population-weighted as instructed: 

# using dplyr syntax: 
wdi_summ_out_repl <- wdi %>% 
 # filter(!is.na(gdp)) %>%    # removing missing GDP values
  group_by(region, date) %>% # grouping by region and year
  rename(year = date) %>%    # renaming date as year variable
  summarise(N = n(),
            Mean = stats::weighted.mean(gdp, pop, na.rm = TRUE),
            SD =  matrixStats::weightedSd(x = gdp, w = pop, na.rm = TRUE), 
            Min = min(gdp, na.rm = TRUE), 
            Max = max(gdp, na.rm = TRUE)) 
```

    `summarise()` has grouped output by 'region'. You can override using the
    `.groups` argument.

**displaying dataset**:

``` r
wdi_summ_out_repl %>% 
  kable(align = "c", format = "markdown") %>% 
  kable_styling(bootstrap_options = c("striped", "hover"), 
                full_width = TRUE) %>%
  scroll_box(width = "600px", height = "600px")
```

    Warning in kable_styling(., bootstrap_options = c("striped", "hover"),
    full_width = TRUE): Please specify format in kable. kableExtra can customize
    either HTML or LaTeX outputs. See https://haozhu233.github.io/kableExtra/ for
    details.

|           region           | year |  N  |   Mean    |     SD     |    Min     |    Max     |
|:--------------------------:|:----:|:---:|:---------:|:----------:|:----------:|:----------:|
|    East Asia & Pacific     | 1990 | 22  | 4913.103  | 8496.5363  |  581.6133  | 32846.390  |
|    East Asia & Pacific     | 1991 | 22  | 5105.010  | 8690.7581  |  579.3788  | 33870.374  |
|    East Asia & Pacific     | 1992 | 22  | 5290.810  | 8666.6926  |  597.2022  | 34048.785  |
|    East Asia & Pacific     | 1993 | 22  | 5482.790  | 8576.3969  |  635.1072  | 33782.735  |
|    East Asia & Pacific     | 1994 | 22  | 5740.088  | 8619.3205  |  669.3751  | 34053.524  |
|    East Asia & Pacific     | 1995 | 22  | 6037.219  | 8796.3479  |  708.5049  | 34867.580  |
|    East Asia & Pacific     | 1996 | 22  | 6339.777  | 9011.0270  |  746.4266  | 35878.789  |
|    East Asia & Pacific     | 1997 | 22  | 6537.701  | 9045.0361  |  781.5961  | 36144.618  |
|    East Asia & Pacific     | 1998 | 22  | 6449.271  | 8823.3366  |  816.5262  | 36099.023  |
|    East Asia & Pacific     | 1999 | 22  | 6634.843  | 8835.6933  |  875.4185  | 37475.979  |
|    East Asia & Pacific     | 2000 | 23  | 6942.470  | 9054.5790  |  973.3955  | 38494.887  |
|    East Asia & Pacific     | 2001 | 23  | 7157.720  | 9025.5625  | 1083.4194  | 38779.600  |
|    East Asia & Pacific     | 2002 | 23  | 7449.350  | 9035.9387  | 1198.0947  | 39872.148  |
|    East Asia & Pacific     | 2003 | 23  | 7802.672  | 9087.5070  | 1340.8727  | 40642.562  |
|    East Asia & Pacific     | 2004 | 24  | 8225.192  | 9234.0642  | 1511.1715  | 41905.850  |
|    East Asia & Pacific     | 2005 | 24  | 8690.507  | 9306.1560  | 1702.3097  | 42704.442  |
|    East Asia & Pacific     | 2006 | 24  | 9237.262  | 9327.5678  | 1914.2416  | 43286.724  |
|    East Asia & Pacific     | 2007 | 24  | 9920.135  | 9347.9717  | 1973.7840  | 44109.669  |
|    East Asia & Pacific     | 2008 | 24  | 10367.217 | 9163.8404  | 1895.9227  | 44777.274  |
|    East Asia & Pacific     | 2009 | 24  | 10668.371 | 8587.6795  | 1874.5419  | 44684.401  |
|    East Asia & Pacific     | 2010 | 24  | 11431.945 | 8816.4615  | 1819.2329  | 44965.393  |
|    East Asia & Pacific     | 2011 | 24  | 12031.219 | 8731.2663  | 1819.6881  | 45405.365  |
|    East Asia & Pacific     | 2012 | 24  | 12627.012 | 8762.4940  | 1883.2061  | 46360.607  |
|    East Asia & Pacific     | 2013 | 24  | 13242.288 | 8822.4279  | 1932.2688  | 46744.624  |
|    East Asia & Pacific     | 2014 | 24  | 13828.938 | 8773.7102  | 1883.2632  | 47240.274  |
|    East Asia & Pacific     | 2015 | 24  | 14454.771 | 8805.5109  | 2038.5218  | 47567.681  |
|    East Asia & Pacific     | 2016 | 24  | 15096.085 | 8786.3075  | 1997.8575  | 48109.203  |
|    East Asia & Pacific     | 2017 | 24  | 15806.715 | 8837.0970  | 1970.8259  | 48400.246  |
|    East Asia & Pacific     | 2018 | 24  | 16530.946 | 8826.3467  | 2039.4298  | 49052.818  |
|    East Asia & Pacific     | 2019 | 24  | 17185.302 | 8728.5384  | 1963.5349  | 49379.093  |
|    East Asia & Pacific     | 2020 | 24  | 17115.830 | 8430.0314  | 1897.0619  | 48747.852  |
|   Europe & Central Asia    | 1990 | 48  | 24285.554 | 10745.7044 | 3638.8768  | 70860.819  |
|   Europe & Central Asia    | 1991 | 48  | 23886.687 | 11314.0006 | 3496.3696  | 75961.669  |
|   Europe & Central Asia    | 1992 | 48  | 23171.591 | 11902.3609 | 2521.4127  | 76323.305  |
|   Europe & Central Asia    | 1993 | 48  | 22680.486 | 12058.0356 | 2085.1926  | 78468.922  |
|   Europe & Central Asia    | 1994 | 48  | 22383.985 | 12962.3047 | 1616.6777  | 80365.225  |
|   Europe & Central Asia    | 1995 | 48  | 22355.489 | 13357.4078 | 1389.0972  | 80379.128  |
|   Europe & Central Asia    | 1996 | 48  | 22630.425 | 13593.7704 | 1134.2220  | 80401.032  |
|   Europe & Central Asia    | 1997 | 48  | 23228.872 | 13954.2318 | 1137.9248  | 83702.814  |
|   Europe & Central Asia    | 1998 | 48  | 23679.543 | 14477.9804 | 1190.6469  | 88185.800  |
|   Europe & Central Asia    | 1999 | 48  | 24311.996 | 14852.3857 | 1228.1605  | 94115.384  |
|   Europe & Central Asia    | 2000 | 49  | 25431.049 | 15201.2313 | 1312.7018  | 99301.527  |
|   Europe & Central Asia    | 2001 | 49  | 26009.219 | 15348.5338 | 1407.9864  | 101143.148 |
|   Europe & Central Asia    | 2002 | 49  | 26498.888 | 15169.7353 | 1528.3448  | 103317.331 |
|   Europe & Central Asia    | 2003 | 49  | 27077.313 | 14894.3344 | 1663.2232  | 104743.000 |
|   Europe & Central Asia    | 2004 | 49  | 28046.647 | 14816.4691 | 1799.8168  | 107634.837 |
|   Europe & Central Asia    | 2005 | 49  | 28859.935 | 14715.6447 | 1884.9458  | 108632.360 |
|   Europe & Central Asia    | 2006 | 49  | 30063.582 | 14778.6321 | 1980.2341  | 113346.036 |
|   Europe & Central Asia    | 2007 | 49  | 31244.859 | 14796.9589 | 2095.7978  | 120647.823 |
|   Europe & Central Asia    | 2008 | 49  | 31562.551 | 14511.0627 | 2219.3051  | 118154.667 |
|   Europe & Central Asia    | 2009 | 49  | 29949.336 | 13879.4934 | 2261.4089  | 112230.081 |
|   Europe & Central Asia    | 2010 | 49  | 30703.639 | 14037.7255 | 2359.9963  | 114343.988 |
|   Europe & Central Asia    | 2011 | 49  | 31494.952 | 14113.2147 | 2481.5523  | 112998.390 |
|   Europe & Central Asia    | 2012 | 49  | 31590.908 | 13863.0762 | 2610.1459  | 112137.135 |
|   Europe & Central Asia    | 2013 | 49  | 31793.777 | 13669.4830 | 2741.2030  | 113050.663 |
|   Europe & Central Asia    | 2014 | 49  | 32161.442 | 13890.6199 | 2858.2128  | 113313.579 |
|   Europe & Central Asia    | 2015 | 49  | 32577.821 | 14286.2138 | 2959.9707  | 113182.729 |
|   Europe & Central Asia    | 2016 | 49  | 33041.573 | 14486.0197 | 3091.2242  | 116283.700 |
|   Europe & Central Asia    | 2017 | 49  | 33935.275 | 14735.4230 | 3236.4393  | 114985.842 |
|   Europe & Central Asia    | 2018 | 49  | 34652.793 | 14847.6473 | 3405.1136  | 114164.469 |
|   Europe & Central Asia    | 2019 | 49  | 35245.617 | 14959.5767 | 3575.2820  | 114542.497 |
|   Europe & Central Asia    | 2020 | 49  | 33422.674 | 14094.5912 | 3651.9427  | 111751.315 |
| Latin America & Caribbean  | 1990 | 23  | 10174.318 | 3344.3431  | 3409.7943  | 15218.308  |
| Latin America & Caribbean  | 1991 | 23  | 10244.827 | 3447.6751  | 3325.1094  | 15522.920  |
| Latin America & Caribbean  | 1992 | 23  | 10278.807 | 3532.5575  | 3262.6471  | 15757.148  |
| Latin America & Caribbean  | 1993 | 23  | 10527.556 | 3560.4582  | 3107.4559  | 15930.484  |
| Latin America & Caribbean  | 1994 | 23  | 10898.913 | 3669.5905  | 2684.8715  | 16402.258  |
| Latin America & Caribbean  | 1995 | 23  | 10826.131 | 3206.2456  | 2895.9796  | 15087.590  |
| Latin America & Caribbean  | 1996 | 23  | 11063.764 | 3433.7608  | 2960.2874  | 15825.094  |
| Latin America & Caribbean  | 1997 | 23  | 11424.820 | 3657.5370  | 2984.8194  | 16618.913  |
| Latin America & Caribbean  | 1998 | 23  | 11527.649 | 3858.7430  | 2994.8069  | 17184.943  |
| Latin America & Caribbean  | 1999 | 23  | 11462.378 | 3953.3919  | 3020.4996  | 17370.813  |
| Latin America & Caribbean  | 2000 | 23  | 11761.061 | 4134.6587  | 2991.6462  | 17942.780  |
| Latin America & Caribbean  | 2001 | 23  | 11695.835 | 4033.0419  | 2928.3256  | 17596.787  |
| Latin America & Caribbean  | 2002 | 23  | 11761.943 | 3922.4505  | 2907.9426  | 18018.296  |
| Latin America & Caribbean  | 2003 | 23  | 11829.343 | 3923.5637  | 2957.7017  | 20504.780  |
| Latin America & Caribbean  | 2004 | 23  | 12260.125 | 4042.3664  | 2870.1152  | 22010.941  |
| Latin America & Caribbean  | 2005 | 23  | 12529.463 | 4066.7414  | 2909.4573  | 23242.640  |
| Latin America & Caribbean  | 2006 | 23  | 12974.322 | 4204.8978  | 2911.5858  | 26162.644  |
| Latin America & Caribbean  | 2007 | 23  | 13450.135 | 4220.5711  | 2998.6113  | 27249.299  |
| Latin America & Caribbean  | 2008 | 23  | 13800.994 | 4193.9737  | 3028.6710  | 28012.873  |
| Latin America & Caribbean  | 2009 | 23  | 13439.324 | 3881.3451  | 3155.8567  | 26622.294  |
| Latin America & Caribbean  | 2010 | 23  | 14106.761 | 4087.9435  | 2943.5495  | 27329.425  |
| Latin America & Caribbean  | 2011 | 23  | 14571.512 | 4192.0151  | 3058.9869  | 27062.384  |
| Latin America & Caribbean  | 2012 | 23  | 14878.288 | 4342.4854  | 3027.4503  | 28642.335  |
| Latin America & Caribbean  | 2013 | 23  | 15167.510 | 4347.4200  | 3111.3903  | 29529.577  |
| Latin America & Caribbean  | 2014 | 23  | 15329.749 | 4388.5614  | 3154.6840  | 30301.935  |
| Latin America & Caribbean  | 2015 | 22  | 15240.919 | 4470.0472  | 3153.1196  | 29876.981  |
| Latin America & Caribbean  | 2016 | 22  | 15136.001 | 4556.7254  | 3165.2957  | 30481.772  |
| Latin America & Caribbean  | 2017 | 22  | 15259.429 | 4597.1418  | 3200.0422  | 31638.152  |
| Latin America & Caribbean  | 2018 | 22  | 15443.394 | 4683.7922  | 3209.4296  | 32259.221  |
| Latin America & Caribbean  | 2019 | 22  | 15444.231 | 4638.5485  | 3112.2959  | 32788.333  |
| Latin America & Caribbean  | 2020 | 22  | 14374.189 | 4156.9162  | 2970.4628  | 26606.295  |
| Latin America & Caribbean  | 2021 | 22  | 15214.319 | 4463.1589  | 2881.1766  | 30416.793  |
| Middle East & North Africa | 1990 | 15  | 8305.774  | 9770.8237  | 4119.8721  | 105893.626 |
| Middle East & North Africa | 1991 | 15  | 8117.231  | 9599.2904  | 2650.0869  | 101048.696 |
| Middle East & North Africa | 1992 | 15  | 8277.679  | 9508.6316  | 3410.7405  | 99106.792  |
| Middle East & North Africa | 1993 | 15  | 8229.110  | 9246.1084  | 4085.7014  | 95524.457  |
| Middle East & North Africa | 1994 | 15  | 8208.511  | 9491.8009  | 3951.2055  | 97475.981  |
| Middle East & North Africa | 1995 | 15  | 8760.389  | 10114.1164 | 4047.1285  | 99510.351  |
| Middle East & North Africa | 1996 | 15  | 9127.352  | 10291.7115 | 3916.9258  | 99602.617  |
| Middle East & North Africa | 1997 | 15  | 9319.769  | 10604.9917 | 4294.7461  | 100956.706 |
| Middle East & North Africa | 1998 | 15  | 9623.239  | 10154.9576 | 4702.2349  | 95185.306  |
| Middle East & North Africa | 1999 | 15  | 9850.246  | 10001.5005 | 4715.1216  | 92368.813  |
| Middle East & North Africa | 2000 | 15  | 10337.296 | 10680.1279 | 4503.2488  | 96835.889  |
| Middle East & North Africa | 2001 | 15  | 10414.799 | 10386.7493 | 3980.9333  | 93106.202  |
| Middle East & North Africa | 2002 | 15  | 10575.403 | 10274.6501 | 3395.8550  | 90661.725  |
| Middle East & North Africa | 2003 | 15  | 10720.424 | 10903.8330 | 3774.1324  | 93989.960  |
| Middle East & North Africa | 2004 | 15  | 11304.196 | 11425.2797 | 4485.5255  | 98342.237  |
| Middle East & North Africa | 2005 | 15  | 11542.893 | 11438.5805 | 4866.0285  | 96188.128  |
| Middle East & North Africa | 2006 | 15  | 11993.943 | 11545.6428 | 4695.9947  | 92323.563  |
| Middle East & North Africa | 2007 | 15  | 12478.311 | 10738.2845 | 4750.9297  | 79468.995  |
| Middle East & North Africa | 2008 | 15  | 12691.038 | 9906.1009  | 4965.3729  | 68909.610  |
| Middle East & North Africa | 2009 | 15  | 12635.678 | 8621.1259  | 5250.1161  | 57094.786  |
| Middle East & North Africa | 2010 | 15  | 12976.161 | 8475.1437  | 5411.0665  | 54664.612  |
| Middle East & North Africa | 2011 | 15  | 13176.437 | 8936.3492  | 5782.6896  | 57815.170  |
| Middle East & North Africa | 2012 | 15  | 13156.565 | 9128.0901  | 5985.6832  | 59949.245  |
| Middle East & North Africa | 2013 | 15  | 13126.276 | 9428.4348  | 3664.2680  | 62354.823  |
| Middle East & North Africa | 2014 | 15  | 13257.142 | 9700.5041  | 3854.2700  | 64334.092  |
| Middle East & North Africa | 2015 | 15  | 13320.736 | 10138.9497 | 4073.6225  | 68076.636  |
| Middle East & North Africa | 2016 | 15  | 13835.558 | 10563.1952 | 4291.2974  | 71244.586  |
| Middle East & North Africa | 2017 | 15  | 13940.692 | 10574.6247 | 4451.6838  | 71182.371  |
| Middle East & North Africa | 2018 | 15  | 13946.131 | 10603.0886 | 4589.3783  | 71550.554  |
| Middle East & North Africa | 2019 | 15  | 13977.161 | 10616.6315 | 4768.1130  | 71782.154  |
| Middle East & North Africa | 2020 |  3  | 53384.500 | 13956.3728 | 39680.6659 | 67668.287  |
|       North America        | 1990 |  2  | 39863.490 | 1765.4487  | 34562.8663 | 40451.498  |
|       North America        | 1991 |  2  | 39228.075 | 1932.2652  | 33423.8845 | 39871.343  |
|       North America        | 1992 |  2  | 39972.394 | 2209.7472  | 33327.9475 | 40707.291  |
|       North America        | 1993 |  2  | 40540.173 | 2225.5615  | 33840.8183 | 41279.517  |
|       North America        | 1994 |  2  | 41680.336 | 2225.5608  | 34976.5929 | 42419.195  |
|       North America        | 1995 |  2  | 42299.384 | 2239.2667  | 35549.0930 | 43042.214  |
|       North America        | 1996 |  2  | 43317.492 | 2509.1902  | 35749.0453 | 44149.371  |
|       North America        | 1997 |  2  | 44705.893 | 2581.7232  | 36910.4779 | 45560.920  |
|       North America        | 1998 |  2  | 46162.196 | 2688.2047  | 38031.6248 | 47050.995  |
|       North America        | 1999 |  2  | 47852.549 | 2700.3984  | 39671.3846 | 48743.883  |
|       North America        | 2000 |  2  | 49303.647 | 2626.6633  | 41338.6470 | 50169.856  |
|       North America        | 2001 |  2  | 49312.838 | 2536.8336  | 41623.9503 | 50149.829  |
|       North America        | 2002 |  2  | 49731.750 | 2415.5149  | 42416.4088 | 50529.350  |
|       North America        | 2003 |  2  | 50641.639 | 2592.1257  | 42793.0793 | 51497.735  |
|       North America        | 2004 |  2  | 52075.835 | 2764.9139  | 43704.4139 | 52989.031  |
|       North America        | 2005 |  2  | 53382.245 | 2874.2426  | 44680.7965 | 54331.658  |
|       North America        | 2006 |  2  | 54332.321 | 2952.2279  | 45396.8397 | 55307.719  |
|       North America        | 2007 |  2  | 54901.685 | 2977.8485  | 45889.5621 | 55885.646  |
|       North America        | 2008 |  2  | 54483.405 | 2854.2679  | 45851.2019 | 55427.178  |
|       North America        | 2009 |  2  | 52575.357 | 2837.8044  | 44004.3141 | 53514.932  |
|       North America        | 2010 |  2  | 53554.887 | 2882.0715  | 44862.4195 | 54510.466  |
|       North America        | 2011 |  2  | 54048.011 | 2730.4643  | 45823.1642 | 54954.464  |
|       North America        | 2012 |  2  | 54833.948 | 2895.7675  | 46126.5139 | 55796.972  |
|       North America        | 2013 |  2  | 55460.439 | 2917.1119  | 46704.7622 | 56432.328  |
|       North America        | 2014 |  2  | 56326.386 | 2923.1165  | 47564.6091 | 57301.600  |
|       North America        | 2015 |  2  | 57329.051 | 3271.9612  | 47522.1407 | 58420.703  |
|       North America        | 2016 |  2  | 57809.016 | 3460.6807  | 47457.5853 | 58965.987  |
|       North America        | 2017 |  2  | 58736.566 | 3493.2888  | 48317.1746 | 59907.754  |
|       North America        | 2018 |  2  | 60086.893 | 3746.2187  | 48962.4815 | 61348.457  |
|       North America        | 2019 |  2  | 61104.777 | 4036.9508  | 49175.6771 | 62470.930  |
|       North America        | 2020 |  2  | 58721.278 | 4245.8473  | 46181.7576 | 60158.910  |
|         South Asia         | 1990 |  7  | 1967.148  |  461.6678  | 1554.6020  |  4040.028  |
|         South Asia         | 1991 |  7  | 1964.394  |  487.1464  | 1611.3595  |  4174.309  |
|         South Asia         | 1992 |  7  | 2034.785  |  515.5777  | 1631.2307  |  4306.581  |
|         South Asia         | 1993 |  7  | 2078.586  |  511.2163  | 1649.0081  |  4551.786  |
|         South Asia         | 1994 |  7  | 2156.313  |  507.7947  | 1741.3094  |  4757.010  |
|         South Asia         | 1995 |  7  | 2258.037  |  522.5170  | 1760.4450  | 10430.789  |
|         South Asia         | 1996 |  7  | 2361.422  |  518.5007  | 1815.3126  | 11010.309  |
|         South Asia         | 1997 |  7  | 2400.534  |  516.7185  | 1868.7838  | 11704.315  |
|         South Asia         | 1998 |  7  | 2482.261  |  511.7457  | 1888.2875  | 12362.967  |
|         South Asia         | 1999 |  7  | 2618.174  |  503.1438  | 1935.6917  | 12922.800  |
|         South Asia         | 2000 |  7  | 2673.141  |  521.7118  | 2020.8554  | 13210.965  |
|         South Asia         | 2001 |  7  | 2738.642  |  489.2452  | 2084.2000  | 12477.225  |
|         South Asia         | 2002 |  7  | 2784.887  |  497.3913  | 2055.7227  | 13156.992  |
|         South Asia         | 2003 |  7  | 2933.848  |  519.9421  | 2107.6410  | 14717.243  |
|         South Asia         | 2004 |  7  | 3101.477  |  542.7827  | 2179.0930  | 15351.696  |
|         South Asia         | 2005 |  7  | 3279.888  |  553.6644  | 2230.7892  | 13124.057  |
|         South Asia         | 2006 |  7  | 3475.510  |  592.8232  | 2285.5122  | 16162.282  |
|         South Asia         | 2007 |  7  | 3669.903  |  616.4224  | 2346.2592  | 16834.744  |
|         South Asia         | 2008 |  7  | 3734.769  |  634.5454  | 2473.9417  | 17788.326  |
|         South Asia         | 2009 |  7  | 3933.320  |  628.1254  | 2572.1751  | 15927.796  |
|         South Asia         | 2010 |  7  | 4163.199  |  679.3012  | 2682.6987  | 16492.531  |
|         South Asia         | 2011 |  7  | 4314.979  |  739.4649  | 2763.8283  | 17290.209  |
|         South Asia         | 2012 |  7  | 4486.607  |  801.3911  | 2886.0975  | 17126.341  |
|         South Asia         | 2013 |  7  | 4696.010  |  829.7137  | 2982.2870  | 17768.609  |
|         South Asia         | 2014 |  7  | 4961.147  |  886.8154  | 3152.2933  | 18338.323  |
|         South Asia         | 2015 |  7  | 5264.632  |  927.2863  | 3260.0350  | 18051.070  |
|         South Asia         | 2016 |  7  | 5604.403  |  986.8277  | 3244.6743  | 18406.268  |
|         South Asia         | 2017 |  7  | 5901.503  | 1055.2887  | 3495.5288  | 18973.569  |
|         South Asia         | 2018 |  7  | 6206.625  | 1065.1248  | 3719.3078  | 19789.490  |
|         South Asia         | 2019 |  7  | 6381.452  | 1046.2925  | 3922.0813  | 20574.404  |
|         South Asia         | 2020 |  7  | 6021.352  |  914.1209  | 3761.8028  | 13419.335  |
|         South Asia         | 2021 |  7  | 6465.718  |  979.5055  | 3853.6803  | 18765.216  |
|     Sub-Saharan Africa     | 1990 | 44  | 2963.420  | 2632.0828  |  460.1237  | 17559.006  |
|     Sub-Saharan Africa     | 1991 | 44  | 2891.724  | 2563.3365  |  473.6120  | 18134.816  |
|     Sub-Saharan Africa     | 1992 | 44  | 2804.268  | 2468.0469  |  436.3764  | 17109.402  |
|     Sub-Saharan Africa     | 1993 | 44  | 2710.943  | 2428.3590  |  469.3978  | 17665.057  |
|     Sub-Saharan Africa     | 1994 | 44  | 2669.551  | 2456.8584  |  476.3708  | 17497.650  |
|     Sub-Saharan Africa     | 1995 | 44  | 2693.126  | 2492.2617  |  465.8333  | 17903.782  |
|     Sub-Saharan Africa     | 1996 | 44  | 2757.525  | 2554.6233  |  502.4025  | 18091.047  |
|     Sub-Saharan Africa     | 1997 | 44  | 2796.189  | 2598.3732  |  544.2141  | 19972.203  |
|     Sub-Saharan Africa     | 1998 | 44  | 2793.386  | 2581.0519  |  584.0687  | 20068.663  |
|     Sub-Saharan Africa     | 1999 | 44  | 2784.571  | 2576.1519  |  636.7917  | 20046.700  |
|     Sub-Saharan Africa     | 2000 | 44  | 2800.877  | 2629.9417  |  628.6933  | 20713.597  |
|     Sub-Saharan Africa     | 2001 | 44  | 2838.174  | 2649.9517  |  687.1935  | 20225.457  |
|     Sub-Saharan Africa     | 2002 | 44  | 2929.292  | 2716.0867  |  690.6596  | 19854.359  |
|     Sub-Saharan Africa     | 2003 | 44  | 2969.078  | 2758.8326  |  708.0288  | 18898.138  |
|     Sub-Saharan Africa     | 2004 | 44  | 3075.495  | 2837.8525  |  733.5419  | 18427.573  |
|     Sub-Saharan Africa     | 2005 | 44  | 3173.734  | 2941.5643  |  754.6644  | 19994.266  |
|     Sub-Saharan Africa     | 2006 | 44  | 3278.571  | 3057.4220  |  769.8871  | 21424.525  |
|     Sub-Saharan Africa     | 2007 | 44  | 3385.715  | 3180.2716  |  792.1909  | 23207.400  |
|     Sub-Saharan Africa     | 2008 | 45  | 3463.003  | 3231.4392  |  812.5922  | 22075.545  |
|     Sub-Saharan Africa     | 2009 | 45  | 3469.040  | 3120.5385  |  801.8034  | 21443.870  |
|     Sub-Saharan Africa     | 2010 | 45  | 3580.228  | 3166.5750  |  804.3549  | 21792.841  |
|     Sub-Saharan Africa     | 2011 | 45  | 3662.092  | 3210.7041  |  807.6650  | 24500.948  |
|     Sub-Saharan Africa     | 2012 | 45  | 3718.517  | 3228.3307  |  814.3208  | 25016.479  |
|     Sub-Saharan Africa     | 2013 | 45  | 3803.773  | 3263.3001  |  764.0515  | 24866.597  |
|     Sub-Saharan Africa     | 2014 | 45  | 3883.981  | 3261.0893  |  765.2596  | 25477.722  |
|     Sub-Saharan Africa     | 2015 | 45  | 3895.780  | 3212.8042  |  781.5793  | 25961.027  |
|     Sub-Saharan Africa     | 2016 | 45  | 3856.604  | 3151.9393  |  764.3366  | 26923.732  |
|     Sub-Saharan Africa     | 2017 | 45  | 3857.984  | 3128.5854  |  750.7876  | 27336.608  |
|     Sub-Saharan Africa     | 2018 | 45  | 3864.929  | 3104.8021  |  740.4482  | 28081.381  |
|     Sub-Saharan Africa     | 2019 | 45  | 3870.249  | 3058.6141  |  729.6585  | 29190.550  |

comparing datasets:

``` r
# loading data to replicate for comparison: 
wdi_summ_out <- readr::read_rds(paste0(data_url, "wdi_summ_out.Rds"))

waldo::compare(wdi_summ_out, wdi_summ_out_repl)
```

    `class(old)`: "data.table"                "data.frame"
    `class(new)`: "grouped_df" "tbl_df" "tbl" "data.frame"

    `attr(old, 'groups')` is absent
    `attr(new, 'groups')` is an S3 object of class <tbl_df/tbl/data.frame>, a list

    `attr(old$region, 'label')` is absent
    `attr(new$region, 'label')` is a character vector ('region')

    `old$year` is a character vector ('1990', '1991', '1992', '1993', '1994', ...)
    `new$year` is a double vector (1990, 1991, 1992, 1993, 1994, ...)

    `old$N` is a double vector (22, 22, 22, 22, 22, ...)
    `new$N` is an integer vector (22, 22, 22, 22, 22, ...)

         old$Mean           | new$Mean                           
     [1] 4913.1033824116075 - 4913.1033824116084 [1]             
     [2] 5105.0104171493358 | 5105.0104171493358 [2]             
     [3] 5290.8095613208006 | 5290.8095613208006 [3]             
     [4] 5482.7896793386963 - 5482.7896793386972 [4]             
     [5] 5740.0878796092338 - 5740.0878796092356 [5]             
     [6] 6037.2194463295655 | 6037.2194463295655 [6]             
     [7] 6339.7772987430008 - 6339.7772987429998 [7]             
     [8] 6537.7012496267316 - 6537.7012496267325 [8]             
     [9] 6449.2714744924242 | 6449.2714744924242 [9]             
    [10] 6634.8428345702769 - 6634.8428345702760 [10]            
     ... ...                  ...                and 116 more ...

          old$Mean            | new$Mean                 
    [128] 39972.3937454740808 | 39972.3937454740808 [128]
    [129] 40540.1730477565070 | 40540.1730477565070 [129]
    [130] 41680.3363897968156 | 41680.3363897968156 [130]
    [131] 42299.3843855870655 - 42299.3843855870582 [131]
    [132] 43317.4916249835878 | 43317.4916249835878 [132]
    [133] 44705.8925252675472 | 44705.8925252675472 [133]
    [134] 46162.1959999669634 | 46162.1959999669634 [134]

          old$Mean            | new$Mean                           
    [139] 50641.6393301463177 | 50641.6393301463177 [139]          
    [140] 52075.8345827743615 | 52075.8345827743615 [140]          
    [141] 53382.2451330527474 | 53382.2451330527474 [141]          
    [142] 54332.3212907509442 - 54332.3212907509514 [142]          
    [143] 54901.6846339972544 - 54901.6846339972617 [143]          
    [144] 54483.4046545501769 | 54483.4046545501769 [144]          
    [145] 52575.3572801488699 - 52575.3572801488772 [145]          
    [146] 53554.8870340037683 - 53554.8870340037756 [146]          
    [147] 54048.0111263539293 | 54048.0111263539293 [147]          
    [148] 54833.9476994414872 | 54833.9476994414872 [148]          
      ... ...                   ...                 and 27 more ...

          old$Mean           | new$Mean                          
    [176] 3933.3196505927381 | 3933.3196505927381 [176]          
    [177] 4163.1986496962954 | 4163.1986496962954 [177]          
    [178] 4314.9787515613989 | 4314.9787515613989 [178]          
    [179] 4486.6074089603635 - 4486.6074089603626 [179]          
    [180] 4696.0101367664583 | 4696.0101367664583 [180]          
    [181] 4961.1473706195020 | 4961.1473706195020 [181]          
    [182] 5264.6321169718831 | 5264.6321169718831 [182]          
    [183] 5604.4025093391583 | 5604.4025093391583 [183]          
    [184] 5901.5029808441432 - 5901.5029808441441 [184]          
    [185] 6206.6246759065252 - 6206.6246759065261 [185]          
      ... ...                  ...                and 33 more ...

         old$SD              | new$SD                             
     [1] 8496.53633718822675 | 8496.53633718822675 [1]            
     [2] 8690.75813054334685 - 8690.75813054334503 [2]            
     [3] 8666.69255281417645 | 8666.69255281417645 [3]            
     [4] 8576.39688546040816 | 8576.39688546040816 [4]            
     [5] 8619.32053242204711 | 8619.32053242204711 [5]            
     [6] 8796.34791297051925 | 8796.34791297051925 [6]            
     [7] 9011.02696463313441 | 9011.02696463313441 [7]            
     [8] 9045.03610218434005 - 9045.03610218434187 [8]            
     [9] 8823.33658869879218 - 8823.33658869879400 [9]            
    [10] 8835.69325873518210 | 8835.69325873518210 [10]           
     ... ...                   ...                 and 16 more ...

    And 1 more differences ...

The two datasets are similar. Differences pointed out by compare()
function come down to minor differences after several decimal places.

## 2. Replicate Aggregate Stats:

``` r
# for loop for each variable? 

wdi_agg <- wdi %>% 
  group_by(region, date) %>% # grouping by region and year
  summarise(
            # Life Expectancy
            mean_lifeex = stats::weighted.mean(lifeex, pop, na.rm = TRUE),
            sd_lifeex =  matrixStats::weightedSd(x = lifeex, w = pop, na.rm = TRUE), 
            min_lifeex = min(lifeex, na.rm = TRUE), 
            max_lifeex = max(lifeex, na.rm = TRUE),
            median_lifeex = matrixStats::weightedMedian(x = lifeex, w = pop, na.rm = TRUE),
            # GDP 
            mean_gdp = stats::weighted.mean(gdp, pop, na.rm = TRUE),
            sd_gdp =  matrixStats::weightedSd(x = gdp, w = pop, na.rm = TRUE), 
            min_gdp = min(gdp, na.rm = TRUE), 
            max_gdp = max(gdp, na.rm = TRUE),
            median_gdp = matrixStats::weightedMedian(x = gdp, w = pop, na.rm = TRUE), 
            # Poverty
            mean_povintl = stats::weighted.mean(pov_intl, pop, na.rm = TRUE),
            sd_povintl =  matrixStats::weightedSd(x = pov_intl, w = pop, na.rm = TRUE), 
            min_povintl = min(pov_intl, na.rm = TRUE), 
            max_povintl = max(pov_intl, na.rm = TRUE),
            median_povintl = matrixStats::weightedMedian(x = pov_intl, w = pop, na.rm = TRUE),
            # Total Population in Each Region by Year:
            tot_pop = sum(pop, na.rm = TRUE)
            )
```

    `summarise()` has grouped output by 'region'. You can override using the
    `.groups` argument.

``` r
# Now we use pivot_longer() and pivot_wider() functions to transform the data from wide to long format: 

wdi_agg_out_repl <- wdi_agg %>% 
  
  pivot_longer(-c(region, date, tot_pop))  %>% 
  # we first gather all aggregate statistics into one "value" column with their corresponding 'name" variable taking values such as "mean_gdp" or median_lifeex". 
  
  separate(name, into = c("estimate", "variable"), sep = "_") %>% 
  # using the "_" separator, we break two the character variable "name" into two components: the estimate component (mean, sd, min, max, median) and the "variable" component corresponding to the specific variable in consideration: gdp, lifeex, povintl
  
   pivot_wider(names_from = "variable", values_from = "value")
 # finally we use pivot_wider() spreading the "variable" column into three columns: gdp, lifeex and povintl


# renaming variables and ordering dataset
wdi_agg_out_repl <- wdi_agg_out_repl %>% 
  relocate(estimate, .before = region) %>% 
  rename(pop = tot_pop, 
         pov_intl = povintl) %>% 
  mutate(estimate = factor(estimate, levels = (c("mean", "sd", "min", "max", "median")))) %>%  
  arrange(estimate)
  
# labeling variables 
labelled::var_label(wdi_agg_out_repl) <- 
  list(estimate = "estimate",
       region = "region",
       date = "year", 
       pop = "Population, total", 
       lifeex = "Life expectancy at birth, total (years)", 
       gdp = "GDP per capita, PPP (constant 2017 international $)", 
       pov_intl = "Poverty headcount at $2.15 (2017 prices)")
```

**Displaying dataset**:

``` r
head(wdi_agg_out_repl, 100) %>% 
  kable(align = "c", format = "markdown") %>% 
  kable_styling(bootstrap_options = c("striped", "hover"), 
                full_width = TRUE) %>%
  scroll_box(width = "600px", height = "600px")
```

    Warning in kable_styling(., bootstrap_options = c("striped", "hover"),
    full_width = TRUE): Please specify format in kable. kableExtra can customize
    either HTML or LaTeX outputs. See https://haozhu233.github.io/kableExtra/ for
    details.

| estimate |           region           | date |    pop     |  lifeex  |    gdp    | pov_intl  |
|:--------:|:--------------------------:|:----:|:----------:|:--------:|:---------:|:---------:|
|   mean   |    East Asia & Pacific     | 1990 | 1754166013 | 68.19770 | 4913.103  | 0.5897045 |
|   mean   |    East Asia & Pacific     | 1991 | 1779284317 | 68.41732 | 5105.010  | 0.5731783 |
|   mean   |    East Asia & Pacific     | 1992 | 1802946756 | 68.89536 | 5290.810  | 0.5495899 |
|   mean   |    East Asia & Pacific     | 1993 | 1825777375 | 69.34064 | 5482.790  | 0.5234072 |
|   mean   |    East Asia & Pacific     | 1994 | 1848480100 | 69.62833 | 5740.088  | 0.4830632 |
|   mean   |    East Asia & Pacific     | 1995 | 1870755748 | 70.00733 | 6037.219  | 0.4504016 |
|   mean   |    East Asia & Pacific     | 1996 | 1892721009 | 70.29316 | 6339.777  | 0.4099235 |
|   mean   |    East Asia & Pacific     | 1997 | 1914534267 | 70.67039 | 6537.701  | 0.4077139 |
|   mean   |    East Asia & Pacific     | 1998 | 1935514675 | 71.06589 | 6449.271  | 0.4168976 |
|   mean   |    East Asia & Pacific     | 1999 | 1955084080 | 71.26554 | 6634.843  | 0.3861952 |
|   mean   |    East Asia & Pacific     | 2000 | 1974380842 | 71.64023 | 6942.470  | 0.3569507 |
|   mean   |    East Asia & Pacific     | 2001 | 1992025200 | 72.20763 | 7157.720  | 0.3363615 |
|   mean   |    East Asia & Pacific     | 2002 | 2008837833 | 72.54372 | 7449.350  | 0.3004620 |
|   mean   |    East Asia & Pacific     | 2003 | 2024982628 | 72.86038 | 7802.672  | 0.2707910 |
|   mean   |    East Asia & Pacific     | 2004 | 2040362927 | 72.95297 | 8225.192  | 0.2375685 |
|   mean   |    East Asia & Pacific     | 2005 | 2055565588 | 73.44410 | 8690.507  | 0.1957607 |
|   mean   |    East Asia & Pacific     | 2006 | 2070742069 | 73.79872 | 9237.262  | 0.1908682 |
|   mean   |    East Asia & Pacific     | 2007 | 2085738331 | 74.04260 | 9920.135  | 0.1701479 |
|   mean   |    East Asia & Pacific     | 2008 | 2100713254 | 74.00235 | 10367.217 | 0.1600824 |
|   mean   |    East Asia & Pacific     | 2009 | 2115342751 | 74.52669 | 10668.371 | 0.1412060 |
|   mean   |    East Asia & Pacific     | 2010 | 2129784509 | 74.72302 | 11431.945 | 0.1209233 |
|   mean   |    East Asia & Pacific     | 2011 | 2145011812 | 74.95594 | 12031.219 | 0.0943918 |
|   mean   |    East Asia & Pacific     | 2012 | 2162144561 | 75.20823 | 12627.012 | 0.0805549 |
|   mean   |    East Asia & Pacific     | 2013 | 2179061870 | 75.44920 | 13242.288 | 0.0415154 |
|   mean   |    East Asia & Pacific     | 2014 | 2195419421 | 75.70163 | 13828.938 | 0.0332039 |
|   mean   |    East Asia & Pacific     | 2015 | 2211047641 | 75.92251 | 14454.771 | 0.0252666 |
|   mean   |    East Asia & Pacific     | 2016 | 2226637138 | 76.11806 | 15096.085 | 0.0211252 |
|   mean   |    East Asia & Pacific     | 2017 | 2242494267 | 76.18488 | 15806.715 | 0.0181355 |
|   mean   |    East Asia & Pacific     | 2018 | 2256218228 | 76.57566 | 16530.946 | 0.0146757 |
|   mean   |    East Asia & Pacific     | 2019 | 2268160993 | 76.78012 | 17185.302 | 0.0111565 |
|   mean   |    East Asia & Pacific     | 2020 | 2277861231 | 76.73622 | 17115.830 | 0.0118253 |
|   mean   |   Europe & Central Asia    | 1990 | 839692852  | 72.22811 | 24285.554 | 0.0185888 |
|   mean   |   Europe & Central Asia    | 1991 | 843987268  | 72.17067 | 23886.687 | 0.0218004 |
|   mean   |   Europe & Central Asia    | 1992 | 847402678  | 71.89052 | 23171.591 | 0.0307886 |
|   mean   |   Europe & Central Asia    | 1993 | 850285670  | 71.53081 | 22680.486 | 0.0373551 |
|   mean   |   Europe & Central Asia    | 1994 | 852407912  | 71.70095 | 22383.985 | 0.0487208 |
|   mean   |   Europe & Central Asia    | 1995 | 853991333  | 71.79243 | 22355.489 | 0.0474017 |
|   mean   |   Europe & Central Asia    | 1996 | 855655379  | 72.22251 | 22630.425 | 0.0485134 |
|   mean   |   Europe & Central Asia    | 1997 | 857127065  | 72.62816 | 23228.872 | 0.0443249 |
|   mean   |   Europe & Central Asia    | 1998 | 858408005  | 72.94249 | 23679.543 | 0.0432629 |
|   mean   |   Europe & Central Asia    | 1999 | 859581049  | 72.89559 | 24311.996 | 0.0553249 |
|   mean   |   Europe & Central Asia    | 2000 | 862277331  | 73.13071 | 25431.049 | 0.0504540 |
|   mean   |   Europe & Central Asia    | 2001 | 863395400  | 73.39110 | 26009.219 | 0.0462838 |
|   mean   |   Europe & Central Asia    | 2002 | 864953609  | 73.50148 | 26498.888 | 0.0413026 |
|   mean   |   Europe & Central Asia    | 2003 | 867173880  | 73.61183 | 27077.313 | 0.0406583 |
|   mean   |   Europe & Central Asia    | 2004 | 869736445  | 74.05278 | 28046.647 | 0.0351221 |
|   mean   |   Europe & Central Asia    | 2005 | 872373068  | 74.19285 | 28859.935 | 0.0346710 |
|   mean   |   Europe & Central Asia    | 2006 | 875083967  | 74.69151 | 30063.582 | 0.0307062 |
|   mean   |   Europe & Central Asia    | 2007 | 878222275  | 75.04942 | 31244.859 | 0.0273696 |
|   mean   |   Europe & Central Asia    | 2008 | 881811825  | 75.31083 | 31562.551 | 0.0244849 |
|   mean   |   Europe & Central Asia    | 2009 | 885299042  | 75.69995 | 29949.336 | 0.0238689 |
|   mean   |   Europe & Central Asia    | 2010 | 888673255  | 75.98485 | 30703.639 | 0.0236580 |
|   mean   |   Europe & Central Asia    | 2011 | 890727299  | 76.45714 | 31494.952 | 0.0221896 |
|   mean   |   Europe & Central Asia    | 2012 | 894206408  | 76.64178 | 31590.908 | 0.0212646 |
|   mean   |   Europe & Central Asia    | 2013 | 898477792  | 76.98477 | 31793.777 | 0.0196729 |
|   mean   |   Europe & Central Asia    | 2014 | 903040813  | 77.29512 | 32161.442 | 0.0200955 |
|   mean   |   Europe & Central Asia    | 2015 | 907560479  | 77.23016 | 32577.821 | 0.0189863 |
|   mean   |   Europe & Central Asia    | 2016 | 911808229  | 77.56398 | 33041.573 | 0.0169483 |
|   mean   |   Europe & Central Asia    | 2017 | 915284792  | 77.77748 | 33935.275 | 0.0166147 |
|   mean   |   Europe & Central Asia    | 2018 | 918207514  | 77.91032 | 34652.793 | 0.0142877 |
|   mean   |   Europe & Central Asia    | 2019 | 920599754  | 78.18514 | 35245.617 | 0.0137498 |
|   mean   |   Europe & Central Asia    | 2020 | 922520078  | 76.95765 | 33422.674 | 0.0139143 |
|   mean   | Latin America & Caribbean  | 1990 | 393044509  | 67.26420 | 10174.318 | 0.1790119 |
|   mean   | Latin America & Caribbean  | 1991 | 400697265  | 67.61880 | 10244.827 | 0.1722628 |
|   mean   | Latin America & Caribbean  | 1992 | 408281864  | 67.97899 | 10278.807 | 0.1692876 |
|   mean   | Latin America & Caribbean  | 1993 | 415846710  | 68.33266 | 10527.556 | 0.1665414 |
|   mean   | Latin America & Caribbean  | 1994 | 423405156  | 68.70423 | 10898.913 | 0.1532183 |
|   mean   | Latin America & Caribbean  | 1995 | 430924030  | 69.05043 | 10826.131 | 0.1518330 |
|   mean   | Latin America & Caribbean  | 1996 | 438351501  | 69.44836 | 11063.764 | 0.1700529 |
|   mean   | Latin America & Caribbean  | 1997 | 445727440  | 69.86073 | 11424.820 | 0.1614918 |
|   mean   | Latin America & Caribbean  | 1998 | 453044647  | 70.10561 | 11527.649 | 0.1524628 |
|   mean   | Latin America & Caribbean  | 1999 | 460257200  | 70.52955 | 11462.378 | 0.1540536 |
|   mean   | Latin America & Caribbean  | 2000 | 467344965  | 70.85768 | 11761.061 | 0.1411750 |
|   mean   | Latin America & Caribbean  | 2001 | 474284993  | 71.11817 | 11695.835 | 0.1358004 |
|   mean   | Latin America & Caribbean  | 2002 | 481071467  | 71.48573 | 11761.943 | 0.1243007 |
|   mean   | Latin America & Caribbean  | 2003 | 487664429  | 71.71450 | 11829.343 | 0.1260394 |
|   mean   | Latin America & Caribbean  | 2004 | 494146032  | 72.00821 | 12260.125 | 0.1154744 |
|   mean   | Latin America & Caribbean  | 2005 | 500549213  | 72.39529 | 12529.463 | 0.1099350 |
|   mean   | Latin America & Caribbean  | 2006 | 506806920  | 72.60159 | 12974.322 | 0.0895372 |
|   mean   | Latin America & Caribbean  | 2007 | 512946835  | 72.78894 | 13450.135 | 0.0849168 |
|   mean   | Latin America & Caribbean  | 2008 | 518957147  | 72.98284 | 13800.994 | 0.0790659 |
|   mean   | Latin America & Caribbean  | 2009 | 524891569  | 73.18434 | 13439.324 | 0.0749427 |
|   mean   | Latin America & Caribbean  | 2010 | 530914933  | 73.08207 | 14106.761 | 0.0681789 |
|   mean   | Latin America & Caribbean  | 2011 | 537094357  | 73.58716 | 14571.512 | 0.0632448 |
|   mean   | Latin America & Caribbean  | 2012 | 543267278  | 73.80994 | 14878.288 | 0.0536770 |
|   mean   | Latin America & Caribbean  | 2013 | 549311259  | 74.08259 | 15167.510 | 0.0479592 |
|   mean   | Latin America & Caribbean  | 2014 | 555278923  | 74.28838 | 15329.749 | 0.0451974 |
|   mean   | Latin America & Caribbean  | 2015 | 530665237  | 74.43084 | 15240.919 | 0.0444136 |
|   mean   | Latin America & Caribbean  | 2016 | 536320446  | 74.48264 | 15136.001 | 0.0467282 |
|   mean   | Latin America & Caribbean  | 2017 | 542244262  | 74.63438 | 15259.429 | 0.0465596 |
|   mean   | Latin America & Caribbean  | 2018 | 548490083  | 74.75066 | 15443.394 | 0.0453020 |
|   mean   | Latin America & Caribbean  | 2019 | 554565970  | 74.93454 | 15444.231 | 0.0457786 |
|   mean   | Latin America & Caribbean  | 2020 | 559765508  | 72.78012 | 14374.189 | 0.0414769 |
|   mean   | Latin America & Caribbean  | 2021 | 564124917  | 71.88239 | 15214.319 | 0.0484864 |
|   mean   | Middle East & North Africa | 1990 | 231523697  | 64.58471 | 8305.774  | 0.0609996 |
|   mean   | Middle East & North Africa | 1991 | 238222973  | 65.80871 | 8117.231  | 0.0667547 |
|   mean   | Middle East & North Africa | 1992 | 244294458  | 66.36705 | 8277.679  | 0.0548171 |
|   mean   | Middle East & North Africa | 1993 | 249720886  | 66.69205 | 8229.110  | 0.0501787 |
|   mean   | Middle East & North Africa | 1994 | 255026070  | 66.87078 | 8208.511  | 0.0473128 |
|   mean   | Middle East & North Africa | 1995 | 260577063  | 67.18288 | 8760.389  | 0.0506417 |

**Comparing Datasets:**

``` r
# loading dataset to replication for comparison: 
wdi_agg_out <- readr::read_rds(paste0(data_url, "wdi_agg_out.Rds"))


waldo::compare(wdi_agg_out, wdi_agg_out_repl)
```

    `class(old)`: "data.table"                "data.frame"
    `class(new)`: "grouped_df" "tbl_df" "tbl" "data.frame"

    `attr(old, 'sorted')` is a character vector ('iso3c', 'date')
    `attr(new, 'sorted')` is absent

    `attr(old, 'groups')` is absent
    `attr(new, 'groups')` is an S3 object of class <tbl_df/tbl/data.frame>, a list

    `old$estimate` is a character vector ('mean', 'mean', 'mean', 'mean', 'mean', ...)
    `new$estimate` is an S3 object of class <factor>, an integer vector

         old$lifeex          | new$lifeex                         
     [1] 68.1976973388072594 - 68.1976973388072736 [1]            
     [2] 68.4173163239407671 | 68.4173163239407671 [2]            
     [3] 68.8953557627816053 - 68.8953557627815911 [3]            
     [4] 69.3406373629141655 | 69.3406373629141655 [4]            
     [5] 69.6283283110117850 - 69.6283283110117708 [5]            
     [6] 70.0073311790400794 - 70.0073311790400652 [6]            
     [7] 70.2931615690908558 | 70.2931615690908558 [7]            
     [8] 70.6703932154161976 | 70.6703932154161976 [8]            
     [9] 71.0658878891729984 - 71.0658878891730126 [9]            
    [10] 71.2655382349961712 | 71.2655382349961712 [10]           
     ... ...                   ...                 and 30 more ...

         old$lifeex          | new$lifeex                         
    [48] 74.6915096858219698 | 74.6915096858219698 [48]           
    [49] 75.0494176630977137 | 75.0494176630977137 [49]           
    [50] 75.3108306962506475 | 75.3108306962506475 [50]           
    [51] 75.6999476866083540 - 75.6999476866083398 [51]           
    [52] 75.9848478169965631 | 75.9848478169965631 [52]           
    [53] 76.4571405400979245 - 76.4571405400979103 [53]           
    [54] 76.6417777359762482 | 76.6417777359762482 [54]           
    [55] 76.9847663139150740 - 76.9847663139150455 [55]           
    [56] 77.2951182257871778 - 77.2951182257872063 [56]           
    [57] 77.2301599055240189 | 77.2301599055240189 [57]           
     ... ...                   ...                 and 70 more ...

          old$lifeex          | new$lifeex                          
    [160] 59.8072268048575211 | 59.8072268048575211 [160]           
    [161] 60.2118258351601341 | 60.2118258351601341 [161]           
    [162] 60.5611054112612592 | 60.5611054112612592 [162]           
    [163] 60.9263817466174089 - 60.9263817466173947 [163]           
    [164] 61.4488284167672489 - 61.4488284167672347 [164]           
    [165] 62.0537947207797274 | 62.0537947207797274 [165]           
    [166] 62.6663194716481442 | 62.6663194716481442 [166]           
    [167] 62.9999490050389568 | 62.9999490050389568 [167]           
    [168] 63.4081710825230473 - 63.4081710825230402 [168]           
    [169] 63.8781656847296659 - 63.8781656847296730 [169]           
      ... ...                   ...                 and 270 more ...

          old$lifeex          | new$lifeex                          
    [870] 76.5930000000000035 | 76.5930000000000035 [870]           
    [871] 75.7330000000000041 | 75.7330000000000041 [871]           
    [872] 76.0040000000000049 | 76.0040000000000049 [872]           
    [873] 68.0049999999999955 - 68.0322933204002567 [873]           
    [874] 68.1689999999999969 - 68.2041884246291090 [874]           
    [875] 68.7339999999999947 - 68.7576277045883444 [875]           
    [876] 69.2159999999999940 - 69.2407183039542389 [876]           
    [877] 69.5199999999999960 - 69.5396036446154113 [877]           
    [878] 70.0079999999999956 - 70.0212943845404965 [878]           
    [879] 70.2660000000000053 - 70.2809521770972054 [879]           
      ... ...                   ...                 and 211 more ...

         old$gdp             | new$gdp                             
     [1] 4913.10338241160844 | 4913.10338241160844 [1]             
     [2] 5105.01041714933672 - 5105.01041714933581 [2]             
     [3] 5290.80956132079882 - 5290.80956132080064 [3]             
     [4] 5482.78967933869717 | 5482.78967933869717 [4]             
     [5] 5740.08787960923655 - 5740.08787960923564 [5]             
     [6] 6037.21944632956365 - 6037.21944632956547 [6]             
     [7] 6339.77729874299894 - 6339.77729874299985 [7]             
     [8] 6537.70124962673435 - 6537.70124962673253 [8]             
     [9] 6449.27147449242420 | 6449.27147449242420 [9]             
    [10] 6634.84283457027595 | 6634.84283457027595 [10]            
     ... ...                   ...                 and 117 more ...

          old$gdp             | new$gdp                  
    [158] 1964.39372849255915 | 1964.39372849255915 [158]
    [159] 2034.78526972962072 | 2034.78526972962072 [159]
    [160] 2078.58596366213260 | 2078.58596366213260 [160]
    [161] 2156.31304223615189 - 2156.31304223615234 [161]
    [162] 2258.03651787769968 - 2258.03651787769923 [162]
    [163] 2361.42236380576242 - 2361.42236380576196 [163]
    [164] 2400.53393916362802 | 2400.53393916362802 [164]
    [165] 2482.26085567857263 | 2482.26085567857263 [165]
    [166] 2618.17371133556981 | 2618.17371133556981 [166]

    And 11 more differences ...

Minor differences due to decimal places once again.

## 3. Find Outliers:

``` r
wdi_outliers_thresholds <- wdi %>%  
  
  group_by(date) %>% 
  
  summarise(
            # Life Expectancy
            mean_lifeex = weighted.mean(x = lifeex, w = pop, na.rm = TRUE), 
            sd_lifeex = matrixStats::weightedSd(x = lifeex, w = pop, na.rm = TRUE), 
            # lower threshold below which value is an outlier
            lower_thresh_lifeex = mean_lifeex - 2.5 * sd_lifeex,
            # upper threshold above which value is an outlier 
            upper_thresh_lifeex = mean_lifeex + 2.5 * sd_lifeex, 
            
            # GDP
            mean_gdp = weighted.mean(x = gdp, w = pop, na.rm = TRUE), 
            sd_gdp = matrixStats::weightedSd(x = gdp, w = pop, na.rm = TRUE), 
            # lower threshold below which value is an outlier
            lower_thresh_gdp = mean_gdp - 2.5 * sd_gdp,
            # upper threshold above which value is an outlier 
            upper_thresh_gdp = mean_gdp + 2.5 * sd_gdp, 
            
            # Gini 
            mean_gini = weighted.mean(x = gini, w = pop, na.rm = TRUE), 
            sd_gini = matrixStats::weightedSd(x = gini, w = pop, na.rm = TRUE), 
            # lower threshold below which value is an outlier
            lower_thresh_gini = mean_gini - 2.5 * sd_gini,
            # upper threshold above which value is an outlier
            upper_thresh_gini = mean_gini + 2.5 * sd_gini
            )


# now we can merge the aggregate statistics and outlier thresholds by year with the original wdi datasets to determine which observations are outliers according to the criteria given in the exercise: 
wdi_outliers_out_repl <- merge(x = wdi, y = wdi_outliers_thresholds, by = "date")

# creating boolean variables determining whether an observation is an outlier (low or high)
wdi_outliers_out_repl <- wdi_outliers_out_repl %>% 
  
  mutate(
         # Life Expectancy 
         ll_lifeex = lifeex < lower_thresh_lifeex, 
         hl_lifeex = lifeex > upper_thresh_lifeex, 
         # GDP
         ll_gdp = gdp < lower_thresh_gdp, 
         hl_gdp = gdp > upper_thresh_gdp,
         # Gini
         ll_gini = gini < lower_thresh_gini,
         hl_gini = gini > upper_thresh_gini)


# ordering variables and observations 
wdi_outliers_out_repl <- wdi_outliers_out_repl %>% 
  relocate(ll_lifeex, .before = mean_gdp) %>% 
  relocate(hl_lifeex, .after = ll_lifeex) %>% 
  relocate(ll_gdp, .before = mean_gini) %>% 
  relocate(hl_gdp, .after = ll_gdp) %>% 
  arrange(country)
```

**Displaying dataset**:

``` r
head(wdi_outliers_out_repl, 200) %>% 
  kable(align = "c", format = "markdown") %>% 
  kable_styling(bootstrap_options = c("striped", "hover"), 
                full_width = TRUE) %>%
  scroll_box(width = "600px", height = "600px")
```

    Warning in kable_styling(., bootstrap_options = c("striped", "hover"),
    full_width = TRUE): Please specify format in kable. kableExtra can customize
    either HTML or LaTeX outputs. See https://haozhu233.github.io/kableExtra/ for
    details.

| date |           region           | iso3c |  country   | pov_ofcl |    gdp    | gini |  lifeex  |   pop    | pov_intl  | pov_lmic  | pov_umic  | mean_lifeex | sd_lifeex | lower_thresh_lifeex | upper_thresh_lifeex | ll_lifeex | hl_lifeex | mean_gdp  |  sd_gdp  | lower_thresh_gdp | upper_thresh_gdp | ll_gdp | hl_gdp | mean_gini | sd_gini  | lower_thresh_gini | upper_thresh_gini | ll_gini | hl_gini |
|:----:|:--------------------------:|:-----:|:----------:|:--------:|:---------:|:----:|:--------:|:--------:|:---------:|:---------:|:---------:|:-----------:|:---------:|:-------------------:|:-------------------:|:---------:|:---------:|:---------:|:--------:|:----------------:|:----------------:|:------:|:------:|:---------:|:--------:|:-----------------:|:-----------------:|:-------:|:-------:|
| 1990 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 4827.028  |  NA  | 73.14400 | 3286542  | 0.0069000 | 0.1008397 | 0.4964028 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 3496.370  |  NA  | 73.37800 | 3266790  | 0.0406326 | 0.2564547 | 0.7342321 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 3264.821  |  NA  | 73.71500 | 3247039  | 0.0522766 | 0.3131766 | 0.7745579 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 3598.810  |  NA  | 73.93900 | 3227287  | 0.0353470 | 0.2438462 | 0.7149127 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 3921.615  |  NA  | 74.13100 | 3207536  | 0.0231216 | 0.1908777 | 0.6490172 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 4471.602  |  NA  | 74.36200 | 3187784  | 0.0096059 | 0.1271147 | 0.5630165 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |   NA    |   NA    |
| 1996 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 4908.932  | 27.0 | 74.59200 | 3168033  | 0.0053485 | 0.0889844 | 0.4787914 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |  FALSE  |  FALSE  |
| 1997 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 4400.313  |  NA  | 73.90400 | 3148281  | 0.0204468 | 0.1808016 | 0.6268566 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 4819.068  |  NA  | 74.99000 | 3128530  | 0.0278254 | 0.2063644 | 0.6647328 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 5474.850  |  NA  | 75.18300 | 3108778  | 0.0247257 | 0.1942499 | 0.6373375 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |   NA    |   NA    |
| 2000 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 5892.582  |  NA  | 75.40400 | 3089027  | 0.0180182 | 0.1639937 | 0.5986779 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |   NA    |   NA    |
| 2001 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 6441.441  |  NA  | 75.63900 | 3060173  | 0.0146913 | 0.1438653 | 0.5652597 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |   NA    |   NA    |
| 2002 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 6753.881  | 31.7 | 75.89000 | 3051010  | 0.0109265 | 0.1138832 | 0.5209097 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |  FALSE  |  FALSE  |
| 2003 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 7153.995  |  NA  | 76.14200 | 3039616  | 0.0081524 | 0.0917289 | 0.4698318 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |   NA    |   NA    |
| 2004 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 7580.128  |  NA  | 76.37600 | 3026939  | 0.0076319 | 0.0835264 | 0.4443899 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |   NA    |   NA    |
| 2005 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 8040.081  | 30.6 | 76.62100 | 3011487  | 0.0059109 | 0.0728930 | 0.4042843 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |  FALSE  |  FALSE  |
| 2006 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 8568.550  |  NA  | 76.81600 | 2992547  | 0.0039996 | 0.0658647 | 0.3901018 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |   NA    |   NA    |
| 2007 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 9150.117  |  NA  | 77.54900 | 2970017  | 0.0028749 | 0.0512457 | 0.3613156 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  | FALSE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |   NA    |   NA    |
| 2008 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 9912.148  | 30.0 | 77.65300 | 2947314  | 0.0019993 | 0.0390261 | 0.3343185 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  | FALSE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |  FALSE  |  FALSE  |
| 2009 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 10313.902 |  NA  | 77.78100 | 2927519  | 0.0034688 | 0.0408429 | 0.3438401 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  | FALSE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |   NA    |   NA    |
| 2010 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 10749.466 |  NA  | 77.93600 | 2913021  | 0.0045130 | 0.0395439 | 0.3455992 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  | FALSE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |   NA    |   NA    |
| 2011 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 11052.778 |  NA  | 78.09200 | 2905195  | 0.0051550 | 0.0425226 | 0.3492214 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  | FALSE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |   NA    |   NA    |
| 2012 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 11227.950 | 29.0 | 78.06400 | 2900401  | 0.0062069 | 0.0480544 | 0.3666775 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  | FALSE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |  FALSE  |  FALSE  |
| 2013 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 11361.252 |  NA  | 78.12300 | 2895092  | 0.0092673 | 0.0771574 | 0.3776502 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  | FALSE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |   NA    |   NA    |
| 2014 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 11586.817 | 34.6 | 78.40700 | 2889104  | 0.0102310 | 0.0967597 | 0.3874383 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  | FALSE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |  FALSE  |  FALSE  |
| 2015 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 11878.438 | 32.8 | 78.64400 | 2880703  | 0.0012062 | 0.0369900 | 0.2615431 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |  FALSE  |  FALSE  |
| 2016 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 12291.842 | 33.7 | 78.86000 | 2876101  | 0.0013930 | 0.0466664 | 0.2541383 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |  FALSE  |  FALSE  |
| 2017 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 12770.992 | 33.1 | 79.04700 | 2873457  | 0.0039257 | 0.0362668 | 0.2554100 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |  FALSE  |  FALSE  |
| 2018 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 13317.119 | 30.1 | 79.18400 | 2866376  | 0.0004811 | 0.0273037 | 0.1816096 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |  FALSE  |  FALSE  |
| 2019 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 13653.182 | 30.1 | 79.28200 | 2854191  | 0.0000000 | 0.0115013 | 0.1490186 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |  FALSE  |  FALSE  |
| 2020 |   Europe & Central Asia    |  ALB  |  Albania   |    NA    | 13278.370 | 29.4 | 76.98900 | 2837849  | 0.0002128 | 0.0147796 | 0.1367199 |  74.35534   | 4.717182  |      62.56238       |      86.14829       |   FALSE   |   FALSE   | 18637.652 | 15789.62 |    -20836.40     |     58111.70     | FALSE  | FALSE  | 37.08401  | 4.392626 |     26.10245      |     48.06558      |  FALSE  |  FALSE  |
| 1990 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8828.874  |  NA  | 67.41600 | 25518074 | 0.0517968 | 0.2237311 | 0.5936019 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8517.377  |  NA  | 67.68800 | 26133905 | 0.0641722 | 0.2529208 | 0.6316003 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8471.528  |  NA  | 67.75700 | 26748303 | 0.0597211 | 0.2440524 | 0.6182341 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8109.884  |  NA  | 67.71900 | 27354327 | 0.0610516 | 0.2476665 | 0.6211920 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 7869.270  |  NA  | 67.36100 | 27937006 | 0.0639814 | 0.2544147 | 0.6265836 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8013.123  | 35.3 | 67.45400 | 28478022 | 0.0577057 | 0.2424832 | 0.6110342 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |  FALSE  |  FALSE  |
| 1996 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8195.860  |  NA  | 68.74900 | 28984634 | 0.0640979 | 0.2583526 | 0.6396594 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |   NA    |   NA    |
| 1997 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8147.878  |  NA  | 69.17100 | 29476031 | 0.0700399 | 0.2738193 | 0.6670192 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8435.036  |  NA  | 69.45100 | 29924668 | 0.0655035 | 0.2664069 | 0.6670157 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8584.071  |  NA  | 70.03200 | 30346083 | 0.0591778 | 0.2538724 | 0.6595860 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |   NA    |   NA    |
| 2000 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8786.190  |  NA  | 70.47800 | 30774621 | 0.0497705 | 0.2315517 | 0.6406370 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |   NA    |   NA    |
| 2001 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 8926.110  |  NA  | 70.82300 | 31200985 | 0.0452364 | 0.2217048 | 0.6365480 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |   NA    |   NA    |
| 2002 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 9299.682  |  NA  | 71.23000 | 31624696 | 0.0369304 | 0.1871504 | 0.5942148 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |   NA    |   NA    |
| 2003 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 9835.162  |  NA  | 71.28700 | 32055883 | 0.0314687 | 0.1698188 | 0.5764566 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |   NA    |   NA    |
| 2004 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10114.726 |  NA  | 71.76200 | 32510186 | 0.0251839 | 0.1473159 | 0.5480747 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |   NA    |   NA    |
| 2005 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10566.373 |  NA  | 72.06100 | 32956690 | 0.0210105 | 0.1309001 | 0.5279457 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |   NA    |   NA    |
| 2006 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10592.247 |  NA  | 72.33400 | 33435080 | 0.0179835 | 0.1188781 | 0.5149538 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |   NA    |   NA    |
| 2007 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10775.532 |  NA  | 72.60200 | 33983827 | 0.0144314 | 0.1020670 | 0.4905065 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  | FALSE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |   NA    |   NA    |
| 2008 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10847.177 |  NA  | 72.94100 | 34569592 | 0.0110151 | 0.0832022 | 0.4568717 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  | FALSE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |   NA    |   NA    |
| 2009 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 10824.576 |  NA  | 73.62000 | 35196037 | 0.0083965 | 0.0676891 | 0.4270769 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  | FALSE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |   NA    |   NA    |
| 2010 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11007.747 |  NA  | 73.80800 | 35856344 | 0.0064586 | 0.0544546 | 0.3998560 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  | FALSE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |   NA    |   NA    |
| 2011 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11113.969 | 27.6 | 74.12300 | 36543541 | 0.0048384 | 0.0419772 | 0.3699987 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  | FALSE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |  FALSE  |  FALSE  |
| 2012 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11270.701 |  NA  | 74.20200 | 37260563 | 0.0042496 | 0.0359205 | 0.3462222 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  | FALSE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |   NA    |   NA    |
| 2013 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11360.638 |  NA  | 74.61500 | 38000626 | 0.0038834 | 0.0313437 | 0.3235216 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  | FALSE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |   NA    |   NA    |
| 2014 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11561.260 |  NA  | 75.11000 | 38760168 | 0.0035783 | 0.0281706 | 0.3055809 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  | FALSE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |   NA    |   NA    |
| 2015 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11751.634 |  NA  | 75.62200 | 39543154 | 0.0034563 | 0.0258517 | 0.2917897 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |   NA    |   NA    |
| 2016 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11888.323 |  NA  | 75.73200 | 40339329 | 0.0033342 | 0.0242041 | 0.2812938 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |   NA    |   NA    |
| 2017 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11809.483 |  NA  | 75.74300 | 41136546 | 0.0033342 | 0.0243871 | 0.2822701 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |   NA    |   NA    |
| 2018 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11725.878 |  NA  | 76.06600 | 41927007 | 0.0032122 | 0.0232887 | 0.2749474 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |   NA    |   NA    |
| 2019 | Middle East & North Africa |  DZA  |  Algeria   |    NA    | 11627.280 |  NA  | 76.47400 | 42705368 | 0.0032122 | 0.0231057 | 0.2734828 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |   NA    |   NA    |
| 1990 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5793.085  |  NA  | 41.89300 | 11828638 | 0.1652797 | 0.3093024 | 0.5843191 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   TRUE    |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5659.119  |  NA  | 43.81300 | 12228691 | 0.1680163 | 0.3142586 | 0.5963407 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   TRUE    |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5158.384  |  NA  | 42.20900 | 12632507 | 0.1919029 | 0.3537655 | 0.6382768 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   TRUE    |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 3799.195  |  NA  | 42.10100 | 13038270 | 0.2736178 | 0.4874785 | 0.7609357 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   TRUE    |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 3728.886  |  NA  | 43.42200 | 13462031 | 0.2789797 | 0.4950096 | 0.7659570 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   TRUE    |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4149.446  |  NA  | 45.84900 | 13912253 | 0.2487543 | 0.4519831 | 0.7248044 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   TRUE    |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |   NA    |   NA    |
| 1996 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4557.148  |  NA  | 46.03300 | 14383350 | 0.2255422 | 0.4108742 | 0.6872821 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   TRUE    |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |   NA    |   NA    |
| 1997 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4728.292  |  NA  | 46.30600 | 14871146 | 0.2149350 | 0.3966135 | 0.6721527 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   TRUE    |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4790.419  |  NA  | 45.05700 | 15366864 | 0.2115163 | 0.3902037 | 0.6674954 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   TRUE    |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4739.510  |  NA  | 45.38600 | 15870753 | 0.2137960 | 0.3952961 | 0.6719825 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   TRUE    |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |   NA    |   NA    |
| 2000 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4728.374  | 52.0 | 46.02400 | 16394062 | 0.2149350 | 0.3966135 | 0.6721527 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   TRUE    |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |  FALSE  |  FALSE  |
| 2001 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 4768.009  |  NA  | 46.59000 | 16941587 | 0.2261470 | 0.4133390 | 0.6887413 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   TRUE    |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |   NA    |   NA    |
| 2002 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5241.821  |  NA  | 47.38600 | 17516139 | 0.2140402 | 0.3952978 | 0.6770913 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   TRUE    |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |   NA    |   NA    |
| 2003 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5217.391  |  NA  | 49.61700 | 18124342 | 0.2314895 | 0.4243199 | 0.7043781 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |   NA    |   NA    |
| 2004 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 5589.238  |  NA  | 50.59200 | 18771125 | 0.2223429 | 0.4217418 | 0.7011801 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |   NA    |   NA    |
| 2005 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 6204.589  |  NA  | 51.57000 | 19450959 | 0.1972601 | 0.4026204 | 0.6812658 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |   NA    |   NA    |
| 2006 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 6677.020  |  NA  | 52.36900 | 20162340 | 0.1809167 | 0.3927536 | 0.6812652 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |   NA    |   NA    |
| 2007 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7340.389  |  NA  | 53.64200 | 20909684 | 0.1520389 | 0.3717998 | 0.6704136 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  | FALSE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |   NA    |   NA    |
| 2008 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7866.184  | 42.7 | 54.63300 | 21691522 | 0.1401263 | 0.3598728 | 0.6692544 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  | FALSE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |  FALSE  |  FALSE  |
| 2009 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7646.144  |  NA  | 55.75200 | 22507674 | 0.1560865 | 0.3884827 | 0.7008793 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  | FALSE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |   NA    |   NA    |
| 2010 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7689.821  |  NA  | 56.72600 | 23364185 | 0.1649020 | 0.3934708 | 0.6996463 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  | FALSE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |   NA    |   NA    |
| 2011 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7663.286  |  NA  | 57.59600 | 24259111 | 0.1769505 | 0.4020998 | 0.7047534 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  | FALSE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |   NA    |   NA    |
| 2012 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 8011.050  |  NA  | 58.62300 | 25188292 | 0.1785449 | 0.3945625 | 0.6923446 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  | FALSE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |   NA    |   NA    |
| 2013 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 8099.679  |  NA  | 59.30700 | 26147002 | 0.1868837 | 0.3992543 | 0.6927341 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  | FALSE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |   NA    |   NA    |
| 2014 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 8183.165  |  NA  | 60.04000 | 27128337 | 0.1963579 | 0.4046610 | 0.6928854 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  | FALSE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |   NA    |   NA    |
| 2015 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7966.886  |  NA  | 60.65500 | 28127721 | 0.2157624 | 0.4273161 | 0.7088338 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |   NA    |   NA    |
| 2016 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7487.925  |  NA  | 61.09200 | 29154746 | 0.2404678 | 0.4560104 | 0.7287458 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |   NA    |   NA    |
| 2017 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 7216.061  |  NA  | 61.68000 | 30208628 | 0.2704345 | 0.4834415 | 0.7500700 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |   NA    |   NA    |
| 2018 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 6878.590  | 51.3 | 62.14400 | 31273533 | 0.3039496 | 0.5243433 | 0.7759013 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |  FALSE  |  TRUE   |
| 2019 |     Sub-Saharan Africa     |  AGO  |   Angola   |    NA    | 6602.269  |  NA  | 62.44800 | 32353588 | 0.3226889 | 0.5461035 | 0.7914211 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |   NA    |   NA    |
| 1990 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 5153.298  |  NA  | 68.82100 | 3556539  | 0.0284146 | 0.1465551 | 0.4304338 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 4473.519  |  NA  | 68.64300 | 3617631  | 0.0479420 | 0.1987907 | 0.5103406 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 2634.963  |  NA  | 68.62700 | 3574555  | 0.1987907 | 0.4524021 | 0.7688520 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 2484.552  |  NA  | 68.84100 | 3457349  | 0.2227117 | 0.4838899 | 0.7912065 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 2683.638  |  NA  | 69.07300 | 3373713  | 0.1948853 | 0.4470321 | 0.7649755 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 2912.781  |  NA  | 69.31100 | 3322782  | 0.1550983 | 0.3906469 | 0.7196308 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |   NA    |   NA    |
| 1996 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 3105.953  |  NA  | 69.78300 | 3298898  | 0.1389882 | 0.3662377 | 0.6980951 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |   NA    |   NA    |
| 1997 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 3236.061  |  NA  | 69.91000 | 3271418  | 0.1123822 | 0.3235216 | 0.6570878 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 3505.368  |  NA  | 70.51400 | 3240550  | 0.1254040 | 0.4604723 | 0.8323015 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 3660.034  | 36.2 | 70.25700 | 3206030  | 0.1189941 | 0.4476524 | 0.8230976 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |  FALSE  |  FALSE  |
| 2000 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 3921.858  |  NA  | 70.62400 | 3168523  | 0.1331953 | 0.4640874 | 0.8211738 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |   NA    |   NA    |
| 2001 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 4346.908  | 35.4 | 70.93200 | 3133133  | 0.1397475 | 0.4723064 | 0.8379924 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |  FALSE  |  FALSE  |
| 2002 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 4965.224  | 34.8 | 71.01800 | 3105037  | 0.0996660 | 0.4368608 | 0.8289501 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |  FALSE  |  FALSE  |
| 2003 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 5698.778  | 33.0 | 71.43600 | 3084102  | 0.0753952 | 0.3965786 | 0.8257592 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |  FALSE  |  FALSE  |
| 2004 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 6334.856  | 37.5 | 71.42100 | 3065745  | 0.0531781 | 0.2892521 | 0.7348814 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |  FALSE  |  FALSE  |
| 2005 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 7259.204  | 36.0 | 71.79200 | 3047246  | 0.0262957 | 0.1936986 | 0.6746392 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |  FALSE  |  FALSE  |
| 2006 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 8273.786  | 29.7 | 71.98700 | 3026486  | 0.0199859 | 0.1546493 | 0.6280611 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |  FALSE  |  FALSE  |
| 2007 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 9476.471  | 31.2 | 72.32700 | 3004393  | 0.0153620 | 0.1369555 | 0.5597316 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  | FALSE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |  FALSE  |  FALSE  |
| 2008 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 10201.559 | 29.2 | 72.39800 | 2983421  | 0.0087969 | 0.0922336 | 0.5214972 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  | FALSE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |  FALSE  |  FALSE  |
| 2009 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 8819.677  | 28.0 | 72.81300 | 2964296  | 0.0124766 | 0.1379577 | 0.6074754 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  | FALSE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |  FALSE  |  FALSE  |
| 2010 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 9068.788  | 30.0 | 73.16000 | 2946293  | 0.0095147 | 0.1402412 | 0.6210717 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  | FALSE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |  FALSE  |  FALSE  |
| 2011 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 9551.158  | 29.4 | 73.30500 | 2928976  | 0.0118507 | 0.1313001 | 0.5925432 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  | FALSE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |  FALSE  |  FALSE  |
| 2012 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 10289.976 | 29.6 | 73.45400 | 2914421  | 0.0083222 | 0.1120397 | 0.5524800 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  | FALSE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |  FALSE  |  FALSE  |
| 2013 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 10677.304 | 30.6 | 73.67600 | 2901385  | 0.0173827 | 0.1132802 | 0.5308642 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  | FALSE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |  FALSE  |  FALSE  |
| 2014 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 11105.532 | 31.5 | 74.05800 | 2889930  | 0.0143914 | 0.1135778 | 0.5105128 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  | FALSE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |  FALSE  |  FALSE  |
| 2015 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 11506.039 | 32.4 | 74.43600 | 2878595  | 0.0114636 | 0.0943295 | 0.4667745 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |  FALSE  |  FALSE  |
| 2016 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 11580.384 | 32.5 | 74.66400 | 2865835  | 0.0110795 | 0.0952222 | 0.4242208 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |  FALSE  |  FALSE  |
| 2017 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 12509.640 | 33.6 | 74.90600 | 2851923  | 0.0078565 | 0.0807934 | 0.4865330 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |  FALSE  |  FALSE  |
| 2018 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 13231.431 | 34.4 | 75.06400 | 2836557  | 0.0133982 | 0.0941740 | 0.4885919 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |  FALSE  |  FALSE  |
| 2019 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 14317.553 | 30.0 | 75.43900 | 2820602  | 0.0103007 | 0.0978054 | 0.5210210 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |  FALSE  |  FALSE  |
| 2020 |   Europe & Central Asia    |  ARM  |  Armenia   |    NA    | 13357.697 | 25.1 | 72.17300 | 2805608  | 0.0035201 | 0.0672420 | 0.5323614 |  74.35534   | 4.717182  |      62.56238       |      86.14829       |   FALSE   |   FALSE   | 18637.652 | 15789.62 |    -20836.40     |     58111.70     | FALSE  | FALSE  | 37.08401  | 4.392626 |     26.10245      |     48.06558      |  TRUE   |  FALSE  |
| 1990 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 31006.100 |  NA  | 76.99463 | 17065128 | 0.0078568 | 0.0103287 | 0.0178229 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 30496.126 |  NA  | 77.27561 | 17284036 | 0.0082709 | 0.0107062 | 0.0182415 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 30285.854 |  NA  | 77.37805 | 17478635 | 0.0086850 | 0.0110836 | 0.0186602 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 31232.585 |  NA  | 77.87805 | 17634808 | 0.0090991 | 0.0114611 | 0.0174665 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 32164.659 |  NA  | 77.87805 | 17805468 | 0.0095132 | 0.0118386 | 0.0174821 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 33044.844 | 32.6 | 77.82927 | 18004882 | 0.0074978 | 0.0122160 | 0.0174977 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |  FALSE  |  FALSE  |
| 1996 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 33905.263 |  NA  | 78.07805 | 18224767 | 0.0074939 | 0.0122146 | 0.0174493 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |   NA    |   NA    |
| 1997 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 34852.752 |  NA  | 78.48049 | 18423037 | 0.0074899 | 0.0114509 | 0.0174010 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 36099.023 |  NA  | 78.63171 | 18607584 | 0.0074860 | 0.0099240 | 0.0173526 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 37475.979 |  NA  | 78.93171 | 18812264 | 0.0074820 | 0.0099229 | 0.0156781 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |   NA    |   NA    |
| 2000 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 38494.887 |  NA  | 79.23415 | 19028802 | 0.0074781 | 0.0099218 | 0.0152232 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |   NA    |   NA    |
| 2001 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 38779.600 | 33.5 | 79.63415 | 19274701 | 0.0074741 | 0.0099206 | 0.0147683 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |  FALSE  |  FALSE  |
| 2002 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 39872.148 |  NA  | 79.93659 | 19495210 | 0.0087237 | 0.0111667 | 0.0161221 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |   NA    |   NA    |
| 2003 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 40642.562 | 33.5 | 80.23902 | 19720737 | 0.0099733 | 0.0124127 | 0.0174758 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |  FALSE  |  FALSE  |
| 2004 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 41905.850 | 33.1 | 80.49024 | 19932722 | 0.0071254 | 0.0071254 | 0.0124786 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |  FALSE  |  FALSE  |
| 2005 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 42704.442 |  NA  | 80.84146 | 20176844 | 0.0046477 | 0.0070177 | 0.0099856 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |   NA    |   NA    |
| 2006 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 43286.724 |  NA  | 81.04146 | 20450966 | 0.0043025 | 0.0069100 | 0.0100122 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |   NA    |   NA    |
| 2007 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 44109.669 |  NA  | 81.29268 | 20827622 | 0.0039574 | 0.0068022 | 0.0075106 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  | FALSE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |   NA    |   NA    |
| 2008 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 44777.274 | 35.4 | 81.39512 | 21249199 | 0.0036123 | 0.0066945 | 0.0066945 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  | FALSE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |  FALSE  |  FALSE  |
| 2009 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 44684.401 |  NA  | 81.54390 | 21691653 | 0.0035520 | 0.0067355 | 0.0083878 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  | FALSE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |   NA    |   NA    |
| 2010 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 44965.393 | 34.7 | 81.69512 | 22031750 | 0.0034917 | 0.0067765 | 0.0100811 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  | FALSE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |  FALSE  |  FALSE  |
| 2011 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 45405.365 |  NA  | 81.89512 | 22340024 | 0.0038650 | 0.0069434 | 0.0100533 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  | FALSE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |   NA    |   NA    |
| 2012 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 46360.607 |  NA  | 82.04634 | 22733465 | 0.0042383 | 0.0071103 | 0.0100255 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  | FALSE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |   NA    |   NA    |
| 2013 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 46744.624 |  NA  | 82.14878 | 23128129 | 0.0046115 | 0.0072772 | 0.0099977 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  | FALSE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |   NA    |   NA    |
| 2014 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 47240.274 | 34.4 | 82.30000 | 23475686 | 0.0049848 | 0.0074441 | 0.0099699 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  | FALSE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |  FALSE  |  FALSE  |
| 2015 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 47567.681 |  NA  | 82.40000 | 23815995 | 0.0049861 | 0.0062158 | 0.0099304 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |   NA    |   NA    |
| 2016 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 48109.203 | 33.7 | 82.44878 | 24190907 | 0.0049874 | 0.0049874 | 0.0098910 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |  FALSE  |  FALSE  |
| 2017 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 48400.246 |  NA  | 82.50000 | 24594202 | 0.0049792 | 0.0061849 | 0.0098669 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |   NA    |   NA    |
| 2018 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 49052.818 | 34.3 | 82.74878 | 24966643 | 0.0049709 | 0.0073823 | 0.0098427 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |  FALSE  |  FALSE  |
| 2019 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 49379.093 |  NA  | 82.90000 | 25340217 | 0.0049709 | 0.0073823 | 0.0098427 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |   NA    |   NA    |
| 2020 |    East Asia & Pacific     |  AUS  | Australia  |    NA    | 48747.852 |  NA  | 83.20000 | 25655289 | 0.0049709 | 0.0073823 | 0.0098427 |  74.35534   | 4.717182  |      62.56238       |      86.14829       |   FALSE   |   FALSE   | 18637.652 | 15789.62 |    -20836.40     |     58111.70     | FALSE  | FALSE  | 37.08401  | 4.392626 |     26.10245      |     48.06558      |   NA    |   NA    |
| 1990 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 37494.560 |  NA  | 75.56829 | 7677850  | 0.0023091 | 0.0023091 | 0.0099951 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 38399.674 |  NA  | 75.61707 | 7754891  | 0.0023091 | 0.0023091 | 0.0099951 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 38774.490 |  NA  | 75.81707 | 7840709  | 0.0023091 | 0.0023091 | 0.0099951 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 38658.650 |  NA  | 76.06829 | 7905633  | 0.0023091 | 0.0023091 | 0.0099951 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 39435.210 | 30.8 | 76.41951 | 7936118  | 0.0023091 | 0.0023091 | 0.0099951 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |  FALSE  |  FALSE  |
| 1995 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 40425.394 | 29.9 | 76.66829 | 7948278  | 0.0000000 | 0.0023517 | 0.0099465 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |  FALSE  |  FALSE  |
| 1996 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 41319.375 | 29.3 | 76.87073 | 7959017  | 0.0000000 | 0.0023667 | 0.0049273 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |  FALSE  |  FALSE  |
| 1997 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 42136.662 | 29.1 | 77.31951 | 7968041  | 0.0024598 | 0.0024598 | 0.0072784 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |  FALSE  |  FALSE  |
| 1998 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 43597.890 | 31.3 | 77.67073 | 7976789  | 0.0023945 | 0.0049140 | 0.0099648 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |  FALSE  |  FALSE  |
| 1999 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 45060.619 | 29.7 | 77.87561 | 7992324  | 0.0022601 | 0.0022601 | 0.0049731 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |  FALSE  |  FALSE  |
| 2000 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 46469.861 | 29.0 | 78.12683 | 8011566  | 0.0000000 | 0.0024463 | 0.0064526 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |  FALSE  |  FALSE  |
| 2001 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 46878.916 |  NA  | 78.57561 | 8042293  | 0.0011384 | 0.0029703 | 0.0049904 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |   NA    |   NA    |
| 2002 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 47419.278 |  NA  | 78.67805 | 8081957  | 0.0022768 | 0.0034943 | 0.0059552 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  |  TRUE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |   NA    |   NA    |
| 2003 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 47633.114 | 29.5 | 78.63171 | 8121423  | 0.0033220 | 0.0040182 | 0.0065194 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |  FALSE  |  FALSE  |
| 2004 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 48633.272 | 29.8 | 79.18049 | 8171966  | 0.0008165 | 0.0014263 | 0.0042374 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |  FALSE  |  FALSE  |
| 2005 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 49387.028 | 28.7 | 79.33171 | 8227829  | 0.0015369 | 0.0019562 | 0.0045073 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |  FALSE  |  FALSE  |
| 2006 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 50840.694 | 29.6 | 79.88049 | 8268641  | 0.0009011 | 0.0023041 | 0.0047615 |  69.53963   | 7.617820  |      50.49508       |      88.58418       |   FALSE   |   FALSE   | 12744.601 | 15319.27 |    -25553.57     |     51042.78     | FALSE  | FALSE  | 40.11037  | 7.770399 |     20.68437      |     59.53637      |  FALSE  |  FALSE  |
| 2007 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 52565.074 | 30.6 | 80.18049 | 8295487  | 0.0037229 | 0.0059876 | 0.0077530 |  69.80928   | 7.534696  |      50.97254       |      88.64602       |   FALSE   |   FALSE   | 13231.451 | 15460.12 |    -25418.84     |     51881.75     | FALSE  |  TRUE  | 38.29966  | 7.630524 |     19.22335      |     57.37597      |  FALSE  |  FALSE  |
| 2008 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 53166.054 | 30.4 | 80.43171 | 8321496  | 0.0047117 | 0.0058202 | 0.0098540 |  69.95129   | 7.496866  |      51.20912       |      88.69345       |   FALSE   |   FALSE   | 13419.788 | 15289.32 |    -24803.52     |     51643.10     | FALSE  |  TRUE  | 40.81463  | 6.784719 |     23.85283      |     57.77643      |  FALSE  |  FALSE  |
| 2009 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 51030.725 | 31.5 | 80.33171 | 8343323  | 0.0065623 | 0.0073256 | 0.0100748 |  70.34661   | 7.351659  |      51.96746       |      88.72576       |   FALSE   |   FALSE   | 13174.357 | 14455.90 |    -22965.41     |     49314.12     | FALSE  |  TRUE  | 37.95138  | 6.171712 |     22.52210      |     53.38066      |  FALSE  |  FALSE  |
| 2010 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 51843.428 | 30.3 | 80.58049 | 8363404  | 0.0049070 | 0.0057520 | 0.0081402 |  70.62082   | 7.289478  |      52.39713       |      88.84452       |   FALSE   |   FALSE   | 13660.794 | 14671.11 |    -23016.98     |     50338.57     | FALSE  |  TRUE  | 38.84934  | 6.549801 |     22.47484      |     55.22384      |  FALSE  |  FALSE  |
| 2011 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 53179.147 | 30.8 | 80.98293 | 8391643  | 0.0041540 | 0.0057502 | 0.0103259 |  70.95878   | 7.129749  |      53.13440       |      88.78315       |   FALSE   |   FALSE   | 14021.017 | 14787.15 |    -22946.86     |     50988.89     | FALSE  |  TRUE  | 38.78036  | 5.617123 |     24.73755      |     52.82317      |  FALSE  |  FALSE  |
| 2012 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 53297.445 | 30.5 | 80.93659 | 8429991  | 0.0061692 | 0.0071656 | 0.0106965 |  71.22893   | 7.038891  |      53.63170       |      88.82616       |   FALSE   |   FALSE   | 14284.920 | 14818.21 |    -22760.60     |     51330.44     | FALSE  |  TRUE  | 40.21631  | 5.962506 |     25.31005      |     55.12258      |  FALSE  |  FALSE  |
| 2013 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 52997.754 | 30.8 | 81.13659 | 8479823  | 0.0033800 | 0.0037661 | 0.0066714 |  71.52264   | 6.965711  |      54.10837       |      88.93692       |   FALSE   |   FALSE   | 14572.444 | 14850.15 |    -22552.94     |     51697.82     | FALSE  |  TRUE  | 38.77854  | 5.663425 |     24.61998      |     52.93711      |  FALSE  |  FALSE  |
| 2014 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 52932.900 | 30.5 | 81.49024 | 8546356  | 0.0022426 | 0.0033589 | 0.0073160 |  71.83224   | 6.927617  |      54.51320       |      89.15128       |   FALSE   |   FALSE   | 14895.884 | 14966.62 |    -22520.67     |     52312.44     | FALSE  |  TRUE  | 39.66306  | 6.260616 |     24.01152      |     55.31460      |  FALSE  |  FALSE  |
| 2015 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 52873.859 | 30.5 | 81.19024 | 8642699  | 0.0067719 | 0.0073144 | 0.0101275 |  72.04576   | 6.819779  |      54.99631       |      89.09520       |   FALSE   |   FALSE   | 15220.731 | 15157.66 |    -22673.43     |     53114.89     | FALSE  | FALSE  | 37.56216  | 4.923590 |     25.25318      |     49.87113      |  FALSE  |  FALSE  |
| 2016 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 53345.742 | 30.8 | 81.64146 | 8736668  | 0.0066406 | 0.0089280 | 0.0132684 |  72.30952   | 6.753262  |      55.42636       |      89.19267       |   FALSE   |   FALSE   | 15550.686 | 15250.47 |    -22575.49     |     53676.86     | FALSE  | FALSE  | 37.71794  | 5.119278 |     24.91975      |     50.51614      |  FALSE  |  FALSE  |
| 2017 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 54172.987 | 29.7 | 81.64390 | 8797566  | 0.0031809 | 0.0039829 | 0.0068288 |  72.50898   | 6.649733  |      55.88465       |      89.13332       |   FALSE   |   FALSE   | 15965.417 | 15476.97 |    -22727.02     |     54657.85     | FALSE  | FALSE  | 37.92075  | 4.860761 |     25.76885      |     50.07265      |  FALSE  |  FALSE  |
| 2018 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 55217.287 | 30.8 | 81.69268 | 8840521  | 0.0056836 | 0.0070349 | 0.0115174 |  72.75578   | 6.643446  |      56.14716       |      89.36439       |   FALSE   |   FALSE   | 16379.375 | 15713.16 |    -22903.53     |     55662.28     | FALSE  | FALSE  | 37.56201  | 5.213651 |     24.52789      |     50.59614      |  FALSE  |  FALSE  |
| 2019 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 55806.438 | 30.2 | 81.89512 | 8879920  | 0.0064064 | 0.0071255 | 0.0084971 |  72.95224   | 6.623025  |      56.39468       |      89.50980       |   FALSE   |   FALSE   | 16689.970 | 15891.68 |    -23039.23     |     56419.17     | FALSE  | FALSE  | 37.57519  | 5.062162 |     24.91978      |     50.23059      |  FALSE  |  FALSE  |
| 2020 |   Europe & Central Asia    |  AUT  |  Austria   |    NA    | 51988.416 | 29.8 | 81.19268 | 8916864  | 0.0068358 | 0.0088753 | 0.0104661 |  74.35534   | 4.717182  |      62.56238       |      86.14829       |   FALSE   |   FALSE   | 18637.652 | 15789.62 |    -20836.40     |     58111.70     | FALSE  | FALSE  | 37.08401  | 4.392626 |     26.10245      |     48.06558      |  FALSE  |  FALSE  |
| 1990 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 7616.910  |  NA  | 62.35200 | 7175200  | 0.0112965 | 0.0418743 | 0.2116386 |  65.13871   | 7.941912  |      45.28393       |      84.99349       |   FALSE   |   FALSE   | 9566.977  | 12598.52 |    -21929.31     |     41063.27     | FALSE  | FALSE  | 35.80927  | 7.621505 |     16.75551      |     54.86303      |   NA    |   NA    |
| 1991 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 7463.629  |  NA  | 62.05200 | 7271300  | 0.0122052 | 0.0442620 | 0.2226407 |  65.30392   | 7.937230  |      45.46084       |      85.14699       |   FALSE   |   FALSE   | 9510.650  | 12531.81 |    -21818.87     |     40840.16     | FALSE  | FALSE  | 34.84978  | 6.331182 |     19.02182      |     50.67773      |   NA    |   NA    |
| 1992 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 5690.181  |  NA  | 61.29600 | 7382050  | 0.0259037 | 0.0884409 | 0.3705139 |  65.57109   | 7.953732  |      45.68676       |      85.45542       |   FALSE   |   FALSE   | 9492.389  | 12537.99 |    -21852.60     |     40837.37     | FALSE  | FALSE  | 41.83976  | 8.899886 |     19.59005      |     64.08948      |   NA    |   NA    |
| 1993 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 4309.921  |  NA  | 61.28600 | 7494800  | 0.0461577 | 0.1825276 | 0.5606488 |  65.72071   | 7.985901  |      45.75596       |      85.68546       |   FALSE   |   FALSE   | 9489.380  | 12479.77 |    -21710.05     |     40688.81     | FALSE  | FALSE  | 35.40868  | 7.161314 |     17.50540      |     53.31196      |   NA    |   NA    |
| 1994 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 3414.511  |  NA  | 61.59400 | 7596550  | 0.0835268 | 0.3000910 | 0.7066591 |  65.95488   | 8.075165  |      45.76697       |      86.14279       |   FALSE   |   FALSE   | 9599.831  | 12690.65 |    -22126.81     |     41326.47     | FALSE  | FALSE  | 40.98136  | 6.867103 |     23.81360      |     58.14912      |   NA    |   NA    |
| 1995 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 2976.995  | 34.7 | 62.30500 | 7684850  | 0.1242059 | 0.3871354 | 0.7774079 |  66.20071   | 7.920361  |      46.39981       |      86.00161       |   FALSE   |   FALSE   | 9785.852  | 12838.57 |    -22310.57     |     41882.27     | FALSE  | FALSE  | 39.66104  | 9.451560 |     16.03213      |     63.28994      |  FALSE  |  FALSE  |
| 1996 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 2985.337  |  NA  | 62.62200 | 7763000  | 0.1233337 | 0.3929936 | 0.7814603 |  66.46636   | 7.996269  |      46.47569       |      86.45703       |   FALSE   |   FALSE   | 10004.477 | 13046.36 |    -22611.43     |     42620.38     | FALSE  | FALSE  | 38.74123  | 8.250476 |     18.11504      |     59.36742      |   NA    |   NA    |
| 1997 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 3128.164  |  NA  | 62.97300 | 7838250  | 0.1148307 | 0.3809989 | 0.7717834 |  66.81484   | 8.004939  |      46.80249       |      86.82719       |   FALSE   |   FALSE   | 10242.625 | 13356.79 |    -23149.35     |     43634.60     | FALSE  | FALSE  | 41.70397  | 9.811026 |     17.17641      |     66.23154      |   NA    |   NA    |
| 1998 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 3408.475  |  NA  | 63.56100 | 7913000  | 0.0997014 | 0.3596868 | 0.7572108 |  67.11284   | 8.057041  |      46.97024       |      87.25544       |   FALSE   |   FALSE   | 10352.427 | 13618.77 |    -23694.50     |     44399.35     | FALSE  | FALSE  | 40.47361  | 8.946692 |     18.10688      |     62.84034      |   NA    |   NA    |
| 1999 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 3628.717  |  NA  | 64.18900 | 7982750  | 0.0881595 | 0.3432444 | 0.7473449 |  67.38320   | 7.934088  |      47.54798       |      87.21842       |   FALSE   |   FALSE   | 10589.516 | 13944.33 |    -24271.32     |     45450.35     | FALSE  | FALSE  | 39.38094  | 7.191094 |     21.40320      |     57.35867      |   NA    |   NA    |
| 2000 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 3998.520  |  NA  | 64.89100 | 8048600  | 0.0741362 | 0.3109957 | 0.7273083 |  67.68763   | 7.876091  |      47.99740       |      87.37785       |   FALSE   |   FALSE   | 10940.954 | 14371.81 |    -24988.58     |     46870.49     | FALSE  | FALSE  | 39.14598  | 8.497427 |     17.90241      |     60.38955      |   NA    |   NA    |
| 2001 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 4360.459  | 36.5 | 65.51000 | 8111200  | 0.0615766 | 0.2833437 | 0.7099665 |  68.03955   | 7.926232  |      48.22397       |      87.85513       |   FALSE   |   FALSE   | 11065.131 | 14400.14 |    -24935.22     |     47065.48     | FALSE  | FALSE  | 39.28925  | 9.928771 |     14.46732      |     64.11117      |  FALSE  |  FALSE  |
| 2002 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 4736.564  | 25.3 | 66.23900 | 8171950  | 0.0007950 | 0.0256339 | 0.4845349 |  68.32245   | 7.876650  |      48.63082       |      88.01407       |   FALSE   |   FALSE   | 11239.108 | 14433.26 |    -24844.04     |     47322.26     | FALSE  | FALSE  | 41.25667  | 6.857553 |     24.11279      |     58.40055      |  FALSE  |  FALSE  |
| 2003 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 5180.686  | 26.8 | 66.75600 | 8234100  | 0.0005058 | 0.0327477 | 0.4457882 |  68.56633   | 7.798309  |      49.07056       |      88.06211       |   FALSE   |   FALSE   | 11490.301 | 14558.40 |    -24905.71     |     47886.31     | FALSE  | FALSE  | 40.20792  | 8.461240 |     19.05482      |     61.36102      |  FALSE  |  FALSE  |
| 2004 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 5610.763  | 26.6 | 67.34600 | 8306500  | 0.0000000 | 0.0103227 | 0.4232238 |  68.81916   | 7.813363  |      49.28575       |      88.35257       |   FALSE   |   FALSE   | 11906.504 | 14858.01 |    -25238.52     |     49051.53     | FALSE  | FALSE  | 37.36054  | 7.184098 |     19.40029      |     55.32079      |  FALSE  |  FALSE  |
| 2005 |   Europe & Central Asia    |  AZE  | Azerbaijan |    NA    | 7106.598  | 26.6 | 67.56200 | 8391850  | 0.0000000 | 0.0031486 | 0.3134640 |  69.16151   | 7.714661  |      49.87485       |      88.44816       |   FALSE   |   FALSE   | 12280.878 | 15077.46 |    -25412.77     |     49974.52     | FALSE  | FALSE  | 40.36008  | 7.201896 |     22.35534      |     58.36482      |  FALSE  |  FALSE  |

Comparison with data to replicate:

``` r
# data to replicate 
wdi_outliers_out <- readr::read_rds(paste0(data_url, "wdi_outliers_out.Rds"))

# arrranging by country for comparison purposes:
wdi_outliers_out <- wdi_outliers_out %>% 
  arrange(country)

# asserting outlier detection is the same in both datasets: 
# if not, then an error message will appear

# for Life Expectancy
message_l <- "Lower outlier detection for life expectancy is not the same in both datasets"
message_h <- "Higher outlier detection for life expectancy is not the same in both datasets"

testit::assert(message_l, 
               sum(wdi_outliers_out_repl$ll_lifeex != wdi_outliers_out$ll_lifeex, na.rm = TRUE) == 0) 

testit::assert(message_h, 
               sum(wdi_outliers_out_repl$hl_lifeex != wdi_outliers_out$hl_lifeex, na.rm = TRUE) == 0) 

# For GDP
testit::assert(message_l, 
               sum(wdi_outliers_out_repl$ll_gdp != wdi_outliers_out$ll_gdp, na.rm = TRUE) == 0) 

testit::assert(message_h, 
               sum(wdi_outliers_out_repl$hl_gdp != wdi_outliers_out$hl_gdp, na.rm = TRUE) == 0) 

# For Gini coefficient: 
testit::assert(message_l, 
               sum(wdi_outliers_out_repl$ll_gini != wdi_outliers_out$ll_gini, na.rm = TRUE) == 0) 

testit::assert(message_h, 
               sum(wdi_outliers_out_repl$hl_gini != wdi_outliers_out$hl_gini, na.rm = TRUE) == 0) 
```

Replicating outlier figure using ggplot package:

``` r
ggplot(
      # dataset and x and y variables for line representing weighted mean life expectancy
      wdi_outliers_out_repl, aes(x = date, y = mean_lifeex)) + 
      # titles and aesthetics
      ggtitle("Life Expectancy 1990-2021 - Visualizing Trend and Outliers") +
      theme_pander() +
      scale_x_continuous(name = "Year", breaks = seq(1990, 2021, 5)) +
      scale_y_continuous(name = "Life Expectancy", breaks = seq(20, 80, 20)) +

      # adding data points colored by region
      geom_point(data = wdi_outliers_out_repl,
                 aes(x  = date, y = lifeex, color = factor(region)), 
                 size = 0.6) +
      # adding line representing the population-weighted mean over the years
      geom_line(linewidth = 0.4, color = "blue") + 
      # adding 2.5sd confidence interval
      geom_ribbon(aes(ymin = lower_thresh_lifeex, ymax = upper_thresh_lifeex),
                  alpha = 0.2) +
      
  # formatting title, axis titles and legend
   theme(plot.title = element_text(color = "dark blue", size = 13, face = "bold", hjust = 0.5),
         axis.title.x = element_text(color = "midnightblue", size = 10, face = "bold", hjust = 0.5), 
         axis.title.y = element_text(color = "midnightblue", size = 10, face = "bold", vjust = 0.5), 
         axis.text.x = element_text(color = "midnightblue", size = 9), 
         axis.text.y = element_text(color = "midnightblue", size = 9), 
         legend.position = c(0.5, 0.2), 
         legend.direction = "horizontal",
         legend.background=element_blank(),
         legend.title = element_blank(),
         legend.text = element_text(color = "midnightblue", size = 8))
```

![](R-Test_files/figure-commonmark/unnamed-chunk-24-1.png)

# Simulated Data

## 4. Poverty Measures:

Loading data as instructed

``` r
l_svy <-readr::read_rds(paste0(data_url, "svy_sim_in1.Rds"))

glimpse(l_svy)
```

    List of 10
     $ Y2001:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0 0 0 0 0 0 0 0 0 0 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 2 1 1 1 1 1 1 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2002:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0 0 0 0 0 0 0 0 0 0 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 2 1 1 1 1 1 1 2 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2003:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0 0 0 0 0 0 0 0 0 0 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 2 1 1 1 1 1 2 1 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2004:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0 0 0 0 0 0 0 0 0 0 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 2 1 1 2 1 2 2 2 2 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2005:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0 0 0 0 0 0 0 0 0 0 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 1 1 1 1 1 2 1 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2006:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0.294 0.302 0.307 0.307 0.308 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 2 1 2 1 1 1 1 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2007:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 0.911 0.912 0.913 0.913 0.914 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 1 2 1 1 1 1 1 2 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2008:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 1.52 1.52 1.52 1.52 1.52 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 2 1 1 1 1 2 1 1 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2009:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 2.14 2.14 2.14 2.14 2.14 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 2 2 1 1 1 1 1 2 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 
     $ Y2010:Classes 'data.table' and 'data.frame': 100000 obs. of  3 variables:
      ..$ income: num [1:100000] 2.72 2.72 2.72 2.72 2.72 ...
      ..$ weight: int [1:100000] 1 1 1 1 1 1 1 1 1 1 ...
      ..$ area  : Factor w/ 2 levels "urban","rural": 1 1 2 1 1 1 2 2 1 1 ...
      ..- attr(*, ".internal.selfref")=<externalptr> 

``` r
# this R object is a list with each element corresponding to a simulated household dataset for each year from 2001 to 2010
# each dataset contains the same variables
```

:

``` r
# Let's bind all the dataframe elements of the list into one dataframe using the bind_rows() function from dplyr: 
l_svy_df <- bind_rows(l_svy, .id = "year")

# cleaning up year variable: 
l_svy_df$year <- gsub('Y','', l_svy_df$year) 
l_svy_df$year <- as.numeric(l_svy_df$year)


# calculating povery headcount, by year and for each poverty line: 
pov_out <- l_svy_df %>% 
  
  group_by(year) %>% 
  
  mutate(
        # defining poverty dummy according to poverty lines
         poor215  = income < 2.15, 
         poor365 = income < 3.65, 
         poor685 = income < 6.85, 
         
         # poverty headcount:
         headcount_215 = sum(poor215 * weight) / sum(weight), 
         headcount_365 = sum(poor365 * weight) / sum(weight), 
         headcount_685 = sum(poor685 * weight) / sum(weight), 
         
         # poverty gap: 
           # first calculating "gap" part of the eqution
         gap215 = ((2.15-income) * poor215) / 2.15,
         gap365 = ((3.65-income) * poor365) / 3.65,
         gap685 = ((6.85-income) * poor685) / 6.85,
           # aggregating with weights: 
         povgap_215 = sum(gap215 * weight) / sum(weight), 
         povgap_365 = sum(gap365 * weight) / sum(weight),
         povgap_685 = sum(gap685 * weight) / sum(weight),
         
         # poverty severity: similar formula as poverty gap with gap^2 
         povseverity_215 = sum(gap215^2 * weight)/sum(weight), 
         povseverity_365 = sum(gap365^2 * weight)/sum(weight),
         povseverity_685 = sum(gap685^2 * weight)/sum(weight)
         ) %>%  
  
  # keeping variables of interest only
  select(year, headcount_215, headcount_365, headcount_685, 
         povgap_215, povgap_365, povgap_685, povseverity_215, 
         povseverity_365, povseverity_685) %>%  
  
  # removing duplicates: 
  distinct()

# now transforming dataset from wide to long: 
pov_out_repl <- pov_out %>% 
      # first gather aggregate statistic into one "value" column 
      pivot_longer(-year) %>% 
      # separating name variable to facilitate pivot_wider() operation below
      separate(name, into = c("measure", "pov_line"), sep = "_") %>% 
      # reshaping from long to wide for desired dataset format
      pivot_wider(names_from = "measure", values_from = "value") %>% 
      # cleaning pov_line variable
      mutate(pov_line = as.numeric(pov_line)/100)
```

**Displaying dataset**:

``` r
pov_out_repl %>% 
  kable(align = "c", format = "markdown") %>% 
  kable_styling(bootstrap_options = c("striped", "hover"), 
                full_width = TRUE) %>%
  scroll_box(width = "600px", height = "600px")
```

    Warning in kable_styling(., bootstrap_options = c("striped", "hover"),
    full_width = TRUE): Please specify format in kable. kableExtra can customize
    either HTML or LaTeX outputs. See https://haozhu233.github.io/kableExtra/ for
    details.

| year | pov_line | headcount |  povgap   | povseverity |
|:----:|:--------:|:---------:|:---------:|:-----------:|
| 2001 |   2.15   | 0.5422254 | 0.4228365 |  0.3798612  |
| 2001 |   3.65   | 0.6611328 | 0.4975328 |  0.4352643  |
| 2001 |   6.85   | 0.8138747 | 0.6139778 |  0.5287430  |
| 2002 |   2.15   | 0.4978546 | 0.3613057 |  0.3129150  |
| 2002 |   3.65   | 0.6326504 | 0.4470285 |  0.3759358  |
| 2002 |   6.85   | 0.7985686 | 0.5774394 |  0.4818298  |
| 2003 |   2.15   | 0.4495065 | 0.2949849 |  0.2407590  |
| 2003 |   3.65   | 0.6061122 | 0.3927487 |  0.3119177  |
| 2003 |   6.85   | 0.7951019 | 0.5422696 |  0.4328688  |
| 2004 |   2.15   | 0.3891313 | 0.2162681 |  0.1575609  |
| 2004 |   3.65   | 0.5713907 | 0.3271271 |  0.2363397  |
| 2004 |   6.85   | 0.7863333 | 0.4981569 |  0.3735382  |
| 2005 |   2.15   | 0.3191814 | 0.1342803 |  0.0773006  |
| 2005 |   3.65   | 0.5355700 | 0.2577392 |  0.1591762  |
| 2005 |   6.85   | 0.7810200 | 0.4536021 |  0.3124131  |
| 2006 |   2.15   | 0.2269120 | 0.0655324 |  0.0266009  |
| 2006 |   3.65   | 0.4819491 | 0.1870087 |  0.0947344  |
| 2006 |   6.85   | 0.7688042 | 0.4029516 |  0.2514004  |
| 2007 |   2.15   | 0.1237929 | 0.0214040 |  0.0053981  |
| 2007 |   3.65   | 0.4177703 | 0.1256204 |  0.0503021  |
| 2007 |   6.85   | 0.7572493 | 0.3545981 |  0.1990952  |
| 2008 |   2.15   | 0.0273748 | 0.0019883 |  0.0002209  |
| 2008 |   3.65   | 0.3316249 | 0.0713925 |  0.0210064  |
| 2008 |   6.85   | 0.7392448 | 0.3050429 |  0.1516430  |
| 2009 |   2.15   | 0.0000003 | 0.0000000 |  0.0000000  |
| 2009 |   3.65   | 0.2252440 | 0.0313220 |  0.0061128  |
| 2009 |   6.85   | 0.7176459 | 0.2579189 |  0.1122589  |
| 2010 |   2.15   | 0.0000000 | 0.0000000 |  0.0000000  |
| 2010 |   3.65   | 0.1003947 | 0.0074464 |  0.0008116  |
| 2010 |   6.85   | 0.6858685 | 0.2091219 |  0.0782869  |

**Loading data to replicate for comparisons:**

``` r
pov_out <- readr::read_rds(paste0(data_url, "dt_pov_out.Rds"))

# comparison using compare() function from waldo package: 
waldo::compare(pov_out, pov_out_repl)
```

    `class(old)`: "data.table"                "data.frame"
    `class(new)`: "grouped_df" "tbl_df" "tbl" "data.frame"

    `attr(old, 'groups')` is absent
    `attr(new, 'groups')` is an S3 object of class <tbl_df/tbl/data.frame>, a list

         old$povgap                 | new$povgap                                
     [1] 0.422836512449614676523169 - 0.422836512449617951681091 [1]            
     [2] 0.497532840998252179343098 - 0.497532840998253678144181 [2]            
     [3] 0.613977765272257114403942 - 0.613977765272253117601053 [3]            
     [4] 0.361305710877410646286734 - 0.361305710877410202197524 [4]            
     [5] 0.447028472149521127754213 - 0.447028472149524958023648 [5]            
     [6] 0.577439398880055909657472 - 0.577439398880055354545959 [6]            
     [7] 0.294984949621574454869943 - 0.294984949621574343847641 [7]            
     [8] 0.392748686486455333977119 - 0.392748686486454945399061 [8]            
     [9] 0.542269561896721707938696 - 0.542269561896722707139418 [9]            
    [10] 0.216268123697459208054639 - 0.216268123697456959853014 [10]           
     ... ...                          ...                        and 20 more ...

    old$povseverity vs new$povseverity
    - 0.379861221972007556679784557
    + 0.379861221972000562274729418
    - 0.435264272597242984907950358
    + 0.435264272597246149043570540
    - 0.528742991700463238480267592
    + 0.528742991700460240878101104
    - 0.312915046937871699217481591
    + 0.312915046937869811838339729
    - 0.375935841309772311724657357
    + 0.375935841309770479856666725
    - 0.481829789411867270843004007
    + 0.481829789411867104309550314
    - 0.240758984465688163911778474
    + 0.240758984465686914910875771
    - 0.311917732119078250363486404
    + 0.311917732119078361385788867
    - 0.432868772584650729484678777
    + 0.432868772584652061752308327
    - 0.157560940016208794745011801
    + 0.157560940016207073899323632
    and 20 more ...

Once again insignificant differences after many decimal places

``` r
# rounding measures for comparison purposes

decimal <- 10

pov_out$headcount <- round(pov_out$headcount, decimal)
pov_out_repl$headcount <- round(pov_out_repl$headcount, decimal)

pov_out$povgap <- round(pov_out$povgap, decimal)
pov_out_repl$povgap <- round(pov_out_repl$povgap, decimal)

pov_out$povseverity <- round(pov_out$povseverity, decimal)
pov_out_repl$povseverity <- round(pov_out_repl$povseverity, decimal)

# The following three statements should be true
sum(pov_out$headcount != pov_out_repl$headcount) == 0
```

    [1] TRUE

``` r
sum(pov_out$povgap != pov_out_repl$povgap) == 0
```

    [1] TRUE

``` r
sum(pov_out$povseverity != pov_out_repl$povseverity) == 0
```

    [1] TRUE

**Replicating Poverty Headcount Figure:**

``` r
ggplot(
      # dataset and x and y variables for line and data point
      pov_out_repl, aes(x = year, y = headcount, color = factor(pov_line))) + 
      # titles and aesthetics
      ggtitle("Poverty Headcount 2001-2010 - 3 Poverty Lines") +
      theme_pander() +
      scale_x_continuous(name = "Year", breaks = seq(2001, 2010, 1)) +
      scale_y_continuous(name = "Poverty Headcount", breaks = seq(0, 0.8, 0.2)) +

      # adding data points colored by poverty line
      geom_point(size = 0.7) +
      # adding line colored by poverty line 
      geom_line(linewidth = 0.5) +

  # formatting title, axis titles and legend
   theme(plot.title = element_text(color = "dark blue", size = 13, face = "bold", hjust = 0.5),
         axis.title.x = element_text(color = "midnightblue", size = 10, face = "bold", hjust = 0.5), 
         axis.title.y = element_text(color = "midnightblue", size = 10, face = "bold", vjust = 0.5), 
         axis.text.x = element_text(color = "midnightblue", size = 9), 
         axis.text.y = element_text(color = "midnightblue", size = 9), 
         legend.position = "bottom", 
         legend.direction = "horizontal",
         legend.background=element_blank(),
         legend.title = element_blank(),
         legend.text = element_text(color = "midnightblue", size = 8))
```

![](R-Test_files/figure-commonmark/unnamed-chunk-36-1.png)

## 5. Lorenz Curve:

**Function:**

``` r
# Creating function to calculate Lorenz curve variables: 

lorenz_function <- function(data, date_year) {

# we need to calculate the cumulative income and the cumulative population for each percentile bins 
# we can use a for loop over each observation to calculate the cumulated income and population

# filtering year in which to calculate Lorenz curve variable: 
data <- data %>% 
  filter(year == date_year)


# defining variables

# 1. number of observations:
N <- length(data$weight)

# 2. weighted income
income_w <- data$income * data$weight 
# this will be added cumulatively to each observation

# 3. weight 
weight <- data$weight

# 4. number representing the weighted population in each of the 100 percentiles
pop_p_threshold <- sum(data$weight) / 100
# create a second object that will be updated at each percentile throughout loop: 
pop_p_threshold_update <- pop_p_threshold 
# this number will be updated by increments of itself at each percentile

# 5. starting points: 

  # we start at 0 for cumulative weighted population and culumative income 
  cum_weighted_pop <- 0
  cum_income <- 0
  
  # we first at the first percentile p 
  p <- 1 

# 6. empty objects in which cumulative values will be calculated: 
# they should have 100 elements representing each percentile
  population_cumulative <- vector(mode = "numeric", length = 100)
  income_cumulative <- vector(mode = "numeric", length = 100)

  
# start of for-loop: 
# looping over each observation in dataset
  
for (i in 1:N) {
  # we add weight and weighted income values step by step (observation by observation)
  cum_weighted_pop <- cum_weighted_pop + weight[i] 
  cum_income <- cum_income + income_w[i]

# while loop: 2 conditions
  
  while ((p <= 100) & (cum_weighted_pop >= pop_p_threshold_update)) {
    
    # we specify that the loop just run until the percentile p=100 is reached. 
    # keep going as long as cumulative weighted pop is still less or equal to the weighted 
    # population in each percentile 
    # at some point, we will reach the threshold where we switch from one percentile to the        next
    # we update this variable 
    # we record the cumulative income and population expressed as shares: 
    
     income_cumulative[p] = cum_income / sum(income_w)
     population_cumulative[p] = cum_weighted_pop / sum(weight)

     # we switch to the next percentile 
     p = p + 1
     
    if (p <= 100) {
    # if we haven't reached the last percentile
      
    # update the threshold value between percentile 
        pop_p_threshold_update <- pop_p_threshold * p
      } # close if loop
     
 } # close while loop 

} # close for loop 
  
    lorenz_vars <- data.frame(cum_population = population_cumulative,
                              cum_welfare = income_cumulative, 
                              year = date_year, 
                              bin = 1:100) 
    
    
  return(lorenz_vars)
    
  } # close function
```

**Applying Lorenz Function created:**

``` r
# applying function
lorenz_list <- vector(mode='list', length = 10)

  for (i in 1:10){
 lorenz_list[[i]] <- lorenz_function(data = l_svy_df, date_year = 2000 + i)
}

lorenz_out_repl <- bind_rows(lorenz_list, .id = "year")

lorenz_out_repl <- lorenz_out_repl %>% 
  mutate(year = 2000 + as.numeric(year)) %>% # cleaning year variable
  relocate(cum_welfare, .before = cum_population)
```

**Displaying dataset**:

``` r
head(lorenz_out_repl, 200) %>% 
  kable(align = "c", format = "markdown") %>% 
  kable_styling(bootstrap_options = c("striped", "hover"), 
                full_width = TRUE) %>%
  scroll_box(width = "600px", height = "600px")
```

    Warning in kable_styling(., bootstrap_options = c("striped", "hover"),
    full_width = TRUE): Please specify format in kable. kableExtra can customize
    either HTML or LaTeX outputs. See https://haozhu233.github.io/kableExtra/ for
    details.

| cum_welfare | cum_population | year | bin |
|:-----------:|:--------------:|:----:|:---:|
|  0.0000000  |   0.0100011    | 2001 |  1  |
|  0.0000000  |   0.0200025    | 2001 |  2  |
|  0.0000000  |   0.0300031    | 2001 |  3  |
|  0.0000000  |   0.0400025    | 2001 |  4  |
|  0.0000000  |   0.0500003    | 2001 |  5  |
|  0.0000000  |   0.0600018    | 2001 |  6  |
|  0.0000000  |   0.0700031    | 2001 |  7  |
|  0.0000000  |   0.0800023    | 2001 |  8  |
|  0.0000000  |   0.0900010    | 2001 |  9  |
|  0.0000000  |   0.1000008    | 2001 | 10  |
|  0.0000000  |   0.1100001    | 2001 | 11  |
|  0.0000000  |   0.1200041    | 2001 | 12  |
|  0.0000000  |   0.1300020    | 2001 | 13  |
|  0.0000000  |   0.1400012    | 2001 | 14  |
|  0.0000000  |   0.1500029    | 2001 | 15  |
|  0.0000000  |   0.1600031    | 2001 | 16  |
|  0.0000000  |   0.1700050    | 2001 | 17  |
|  0.0000000  |   0.1800075    | 2001 | 18  |
|  0.0000000  |   0.1900030    | 2001 | 19  |
|  0.0000000  |   0.2000001    | 2001 | 20  |
|  0.0000000  |   0.2100058    | 2001 | 21  |
|  0.0000000  |   0.2200087    | 2001 | 22  |
|  0.0000000  |   0.2300047    | 2001 | 23  |
|  0.0000000  |   0.2400073    | 2001 | 24  |
|  0.0000000  |   0.2500009    | 2001 | 25  |
|  0.0000000  |   0.2600015    | 2001 | 26  |
|  0.0000000  |   0.2700021    | 2001 | 27  |
|  0.0000000  |   0.2800000    | 2001 | 28  |
|  0.0000248  |   0.2900054    | 2001 | 29  |
|  0.0001992  |   0.3000006    | 2001 | 30  |
|  0.0005424  |   0.3100111    | 2001 | 31  |
|  0.0010673  |   0.3200073    | 2001 | 32  |
|  0.0017762  |   0.3300028    | 2001 | 33  |
|  0.0026600  |   0.3400081    | 2001 | 34  |
|  0.0037225  |   0.3500042    | 2001 | 35  |
|  0.0049598  |   0.3600004    | 2001 | 36  |
|  0.0063828  |   0.3700064    | 2001 | 37  |
|  0.0079881  |   0.3800015    | 2001 | 38  |
|  0.0097839  |   0.3900120    | 2001 | 39  |
|  0.0117757  |   0.4000052    | 2001 | 40  |
|  0.0139657  |   0.4100027    | 2001 | 41  |
|  0.0163642  |   0.4200115    | 2001 | 42  |
|  0.0189507  |   0.4300025    | 2001 | 43  |
|  0.0217306  |   0.4400050    | 2001 | 44  |
|  0.0247119  |   0.4500010    | 2001 | 45  |
|  0.0279101  |   0.4600060    | 2001 | 46  |
|  0.0313314  |   0.4700044    | 2001 | 47  |
|  0.0349779  |   0.4800104    | 2001 | 48  |
|  0.0388514  |   0.4900026    | 2001 | 49  |
|  0.0429612  |   0.5000132    | 2001 | 50  |
|  0.0473049  |   0.5100076    | 2001 | 51  |
|  0.0518970  |   0.5200001    | 2001 | 52  |
|  0.0567583  |   0.5300142    | 2001 | 53  |
|  0.0618519  |   0.5400017    | 2001 | 54  |
|  0.0672019  |   0.5500001    | 2001 | 55  |
|  0.0728233  |   0.5600059    | 2001 | 56  |
|  0.0787333  |   0.5700069    | 2001 | 57  |
|  0.0849381  |   0.5800163    | 2001 | 58  |
|  0.0914176  |   0.5900018    | 2001 | 59  |
|  0.0982251  |   0.6000159    | 2001 | 60  |
|  0.1053180  |   0.6100142    | 2001 | 61  |
|  0.1127278  |   0.6200059    | 2001 | 62  |
|  0.1204559  |   0.6300048    | 2001 | 63  |
|  0.1285091  |   0.6400125    | 2001 | 64  |
|  0.1368729  |   0.6500047    | 2001 | 65  |
|  0.1455987  |   0.6600066    | 2001 | 66  |
|  0.1547071  |   0.6700149    | 2001 | 67  |
|  0.1641931  |   0.6800106    | 2001 | 68  |
|  0.1740870  |   0.6900155    | 2001 | 69  |
|  0.1843392  |   0.7000068    | 2001 | 70  |
|  0.1950198  |   0.7100168    | 2001 | 71  |
|  0.2061262  |   0.7200071    | 2001 | 72  |
|  0.2177236  |   0.7300167    | 2001 | 73  |
|  0.2297927  |   0.7400196    | 2001 | 74  |
|  0.2423591  |   0.7500192    | 2001 | 75  |
|  0.2554389  |   0.7600156    | 2001 | 76  |
|  0.2690397  |   0.7700046    | 2001 | 77  |
|  0.2832584  |   0.7800035    | 2001 | 78  |
|  0.2981274  |   0.7900124    | 2001 | 79  |
|  0.3136042  |   0.8000112    | 2001 | 80  |
|  0.3297479  |   0.8100190    | 2001 | 81  |
|  0.3466652  |   0.8200190    | 2001 | 82  |
|  0.3643468  |   0.8300012    | 2001 | 83  |
|  0.3829506  |   0.8400031    | 2001 | 84  |
|  0.4024694  |   0.8500077    | 2001 | 85  |
|  0.4229712  |   0.8600196    | 2001 | 86  |
|  0.4444870  |   0.8700093    | 2001 | 87  |
|  0.4672427  |   0.8800007    | 2001 | 88  |
|  0.4913725  |   0.8900169    | 2001 | 89  |
|  0.5169706  |   0.9000200    | 2001 | 90  |
|  0.5442030  |   0.9100103    | 2001 | 91  |
|  0.5733999  |   0.9200181    | 2001 | 92  |
|  0.6048746  |   0.9300197    | 2001 | 93  |
|  0.6389425  |   0.9400179    | 2001 | 94  |
|  0.6763207  |   0.9500206    | 2001 | 95  |
|  0.7178816  |   0.9600244    | 2001 | 96  |
|  0.7649522  |   0.9700213    | 2001 | 97  |
|  0.8196462  |   0.9800174    | 2001 | 98  |
|  0.8880564  |   0.9900063    | 2001 | 99  |
|  1.0000000  |   1.0000000    | 2001 | 100 |
|  0.0000000  |   0.0100003    | 2002 |  1  |
|  0.0000000  |   0.0200011    | 2002 |  2  |
|  0.0000000  |   0.0300024    | 2002 |  3  |
|  0.0000000  |   0.0400033    | 2002 |  4  |
|  0.0000000  |   0.0500003    | 2002 |  5  |
|  0.0000000  |   0.0600044    | 2002 |  6  |
|  0.0000000  |   0.0700042    | 2002 |  7  |
|  0.0000000  |   0.0800036    | 2002 |  8  |
|  0.0000000  |   0.0900015    | 2002 |  9  |
|  0.0000000  |   0.1000014    | 2002 | 10  |
|  0.0000000  |   0.1100032    | 2002 | 11  |
|  0.0000000  |   0.1200056    | 2002 | 12  |
|  0.0000000  |   0.1300061    | 2002 | 13  |
|  0.0000000  |   0.1400038    | 2002 | 14  |
|  0.0000000  |   0.1500066    | 2002 | 15  |
|  0.0000000  |   0.1600069    | 2002 | 16  |
|  0.0000000  |   0.1700033    | 2002 | 17  |
|  0.0000000  |   0.1800027    | 2002 | 18  |
|  0.0000000  |   0.1900021    | 2002 | 19  |
|  0.0000000  |   0.2000065    | 2002 | 20  |
|  0.0000024  |   0.2100078    | 2002 | 21  |
|  0.0001043  |   0.2200077    | 2002 | 22  |
|  0.0003600  |   0.2300014    | 2002 | 23  |
|  0.0007652  |   0.2400076    | 2002 | 24  |
|  0.0013202  |   0.2500028    | 2002 | 25  |
|  0.0020226  |   0.2600019    | 2002 | 26  |
|  0.0028795  |   0.2700051    | 2002 | 27  |
|  0.0038930  |   0.2800020    | 2002 | 28  |
|  0.0050665  |   0.2900021    | 2002 | 29  |
|  0.0063951  |   0.3000011    | 2002 | 30  |
|  0.0078810  |   0.3100101    | 2002 | 31  |
|  0.0095286  |   0.3200046    | 2002 | 32  |
|  0.0113413  |   0.3300001    | 2002 | 33  |
|  0.0133225  |   0.3400027    | 2002 | 34  |
|  0.0154743  |   0.3500005    | 2002 | 35  |
|  0.0178028  |   0.3600064    | 2002 | 36  |
|  0.0202978  |   0.3700029    | 2002 | 37  |
|  0.0229670  |   0.3800117    | 2002 | 38  |
|  0.0258044  |   0.3900042    | 2002 | 39  |
|  0.0288133  |   0.4000065    | 2002 | 40  |
|  0.0320049  |   0.4100120    | 2002 | 41  |
|  0.0353669  |   0.4200082    | 2002 | 42  |
|  0.0389145  |   0.4300073    | 2002 | 43  |
|  0.0426440  |   0.4400027    | 2002 | 44  |
|  0.0465697  |   0.4500075    | 2002 | 45  |
|  0.0506866  |   0.4600098    | 2002 | 46  |
|  0.0550020  |   0.4700012    | 2002 | 47  |
|  0.0595254  |   0.4800116    | 2002 | 48  |
|  0.0642481  |   0.4900138    | 2002 | 49  |
|  0.0691707  |   0.5000053    | 2002 | 50  |
|  0.0743088  |   0.5100096    | 2002 | 51  |
|  0.0796711  |   0.5200048    | 2002 | 52  |
|  0.0852720  |   0.5300141    | 2002 | 53  |
|  0.0910979  |   0.5400091    | 2002 | 54  |
|  0.0971596  |   0.5500111    | 2002 | 55  |
|  0.1034515  |   0.5600075    | 2002 | 56  |
|  0.1099915  |   0.5700038    | 2002 | 57  |
|  0.1167902  |   0.5800023    | 2002 | 58  |
|  0.1238391  |   0.5900058    | 2002 | 59  |
|  0.1311748  |   0.6000156    | 2002 | 60  |
|  0.1387911  |   0.6100101    | 2002 | 61  |
|  0.1467027  |   0.6200033    | 2002 | 62  |
|  0.1549219  |   0.6300106    | 2002 | 63  |
|  0.1634603  |   0.6400119    | 2002 | 64  |
|  0.1722890  |   0.6500001    | 2002 | 65  |
|  0.1814535  |   0.6600104    | 2002 | 66  |
|  0.1909511  |   0.6700123    | 2002 | 67  |
|  0.2008060  |   0.6800175    | 2002 | 68  |
|  0.2110203  |   0.6900117    | 2002 | 69  |
|  0.2216164  |   0.7000010    | 2002 | 70  |
|  0.2326567  |   0.7100032    | 2002 | 71  |
|  0.2441449  |   0.7200049    | 2002 | 72  |
|  0.2560908  |   0.7300185    | 2002 | 73  |
|  0.2684509  |   0.7400038    | 2002 | 74  |
|  0.2813155  |   0.7500047    | 2002 | 75  |
|  0.2946926  |   0.7600138    | 2002 | 76  |
|  0.3085520  |   0.7700111    | 2002 | 77  |
|  0.3229457  |   0.7800019    | 2002 | 78  |
|  0.3379855  |   0.7900207    | 2002 | 79  |
|  0.3536169  |   0.8000049    | 2002 | 80  |
|  0.3699226  |   0.8100141    | 2002 | 81  |
|  0.3869019  |   0.8200071    | 2002 | 82  |
|  0.4046363  |   0.8300067    | 2002 | 83  |
|  0.4231595  |   0.8400029    | 2002 | 84  |
|  0.4425411  |   0.8500076    | 2002 | 85  |
|  0.4628032  |   0.8600207    | 2002 | 86  |
|  0.4839450  |   0.8700105    | 2002 | 87  |
|  0.5061206  |   0.8800135    | 2002 | 88  |
|  0.5294553  |   0.8900067    | 2002 | 89  |
|  0.5541103  |   0.9000032    | 2002 | 90  |
|  0.5802292  |   0.9100073    | 2002 | 91  |
|  0.6080557  |   0.9200204    | 2002 | 92  |
|  0.6376815  |   0.9300045    | 2002 | 93  |
|  0.6696191  |   0.9400087    | 2002 | 94  |
|  0.7042157  |   0.9500146    | 2002 | 95  |
|  0.7425515  |   0.9600001    | 2002 | 96  |
|  0.7856647  |   0.9700023    | 2002 | 97  |
|  0.8357304  |   0.9800218    | 2002 | 98  |
|  0.8972822  |   0.9900015    | 2002 | 99  |
|  0.0000000  |   0.0000000    | 2002 | 100 |

**Comparing Datasets:**

``` r
lorenz_out <- readr::read_rds(paste0(data_url, "dt_lorenz_out.Rds"))

waldo::compare(lorenz_out, lorenz_out_repl)
```

    `old` is length 5
    `new` is length 4

    `names(old)[1:4]`: "welfare" "cum_welfare" "cum_population" "year"
    `names(new)[1:3]`:           "cum_welfare" "cum_population" "year"

    `old$welfare` is a double vector (0, 0, 0, 0, 0, ...)
    `new$welfare` is absent

         old$cum_welfare         | new$cum_welfare                        
    [26] 0.000000000000000000000 | 0.000000000000000000000 [26]           
    [27] 0.000000000000000000000 | 0.000000000000000000000 [27]           
    [28] 0.000000000000000000000 | 0.000000000000000000000 [28]           
    [29] 0.000024825296692951159 - 0.000024825296692951230 [29]           
    [30] 0.000199228746626629660 - 0.000199228746626630229 [30]           
    [31] 0.000542433230480700106 - 0.000542433230480701624 [31]           
    [32] 0.001067268004930284168 - 0.001067268004930287204 [32]           
    [33] 0.001776238552210863053 - 0.001776238552210868257 [33]           
    [34] 0.002660013779649268023 - 0.002660013779649275829 [34]           
    [35] 0.003722537696480630268 - 0.003722537696480640677 [35]           
     ... ...                       ...                     and 68 more ...

          old$cum_welfare         | new$cum_welfare                        
    [118] 0.000000000000000000000 | 0.000000000000000000000 [118]          
    [119] 0.000000000000000000000 | 0.000000000000000000000 [119]          
    [120] 0.000000000000000000000 | 0.000000000000000000000 [120]          
    [121] 0.000002378575468880935 - 0.000002378575468880926 [121]          
    [122] 0.000104338919871559339 - 0.000104338919871558973 [122]          
    [123] 0.000360045542253494900 - 0.000360045542253493653 [123]          
    [124] 0.000765204509703419026 - 0.000765204509703416315 [124]          
    [125] 0.001320167329623054241 - 0.001320167329623049687 [125]          
    [126] 0.002022647251165708704 - 0.002022647251165701331 [126]          
    [127] 0.002879525607532907195 - 0.002879525607532897220 [127]          
      ... ...                       ...                     and 76 more ...

          old$cum_welfare         | new$cum_welfare                         
    [210] 0.000000000000000000000 | 0.000000000000000000000 [210]           
    [211] 0.000000000000000000000 | 0.000000000000000000000 [211]           
    [212] 0.000000000000000000000 | 0.000000000000000000000 [212]           
    [213] 0.000004880601370743974 - 0.000004880601370743965 [213]           
    [214] 0.000113035929207294040 - 0.000113035929207293823 [214]           
    [215] 0.000361346344522625834 - 0.000361346344522625129 [215]           
    [216] 0.000748619550541399967 - 0.000748619550541398449 [216]           
    [217] 0.001279343439640182826 - 0.001279343439640180441 [217]           
    [218] 0.001946640983456262523 - 0.001946640983456258836 [218]           
    [219] 0.002752615740364624565 - 0.002752615740364618927 [219]           
      ... ...                       ...                     and 584 more ...

          old$cum_welfare         | new$cum_welfare                        
    [898] 0.912947233249493339535 | 0.912947233249493339535 [898]          
    [899] 0.946959157593749045745 | 0.946959157593749045745 [899]          
    [900] 1.000000000000000888178 | 1.000000000000000888178 [900]          
    [901] 0.004522043454981021504 - 0.004522043454981031045 [901]          
    [902] 0.009253848965270362187 - 0.009253848965270381269 [902]          
    [903] 0.014114309032891084184 - 0.014114309032891115409 [903]          
    [904] 0.019079962585637693512 - 0.019079962585637735145 [904]          
    [905] 0.024133949113674623560 - 0.024133949113674675602 [905]          
    [906] 0.029270601077160653125 - 0.029270601077160715575 [906]          
    [907] 0.034482828761518882765 - 0.034482828761518952154 [907]          
      ... ...                       ...                     and 93 more ...

    `old$cum_population[197:203]`: 1 1 1 1 0 0 0
    `new$cum_population[197:203]`: 1 1 1 0 0 0 0

Some minor differences after several decimal places

**Replicating Lorenz Curve** Graph

``` r
ggplot(
      # dataset and x and y variables for line and data point
      lorenz_out_repl, aes(x = cum_population, y = cum_welfare, color = factor(year))) + 
      # titles and aesthetics
      ggtitle("Lorenz Curve 2001-2010") +
      theme_pander() +
      scale_x_continuous(name = "Cumulative Population", breaks = seq(0, 1, 0.25)) +
      scale_y_continuous(name = "Cumulative Welfare", breaks = seq(0, 1, 0.25)) +

      # adding line colored by poverty line 
      geom_line(linewidth = 0.5) +

  # formatting title, axis titles and legend
   theme(plot.title = element_text(color = "dark blue", size = 13, face = "bold", hjust = 0.5),
         axis.title.x = element_text(color = "midnightblue", size = 10, face = "bold", hjust = 0.5), 
         axis.title.y = element_text(color = "midnightblue", size = 10, face = "bold", vjust = 0.5), 
         axis.text.x = element_text(color = "midnightblue", size = 9), 
         axis.text.y = element_text(color = "midnightblue", size = 9), 
         legend.position = "bottom", 
         legend.direction = "horizontal",
         legend.background=element_blank(),
         legend.title = element_blank(),
         legend.text = element_text(color = "midnightblue", size = 8))
```

![](R-Test_files/figure-commonmark/unnamed-chunk-46-1.png)

## 6. Gini Coefficient:

**Create function to calculate Gini coefficient:**

``` r
# Making use of formula provided on wikipedia page: 
# a = area above the curve 
# b = area under the curve 

# Gini = a / (a + b) 
# (0,1) axis on both sides so areas a + b form triangle area = 1/2

# gini formila = 1 - 2b
# need to calculate area under the curve b


gini_function <- function(data, date_year) {
  
  data <- data %>% 
    filter(year == date_year)


income_w <- data$weight * data$income
weight <- data$weight 

 # attempting to compute area under the curve: 
# formula for calculating the area: base x height: 
area_b <- sum((cumsum(income_w) + (income_w / 2)) * weight) 
gini <- 1 - (2 * area_b)

gini_df <- data.frame(year = date_year, 
                     gini = gini)

return(gini_df)

}
```

``` r
gini_list <- vector(mode='list', length = 10)

  for (i in 1:10){
 gini_list[[i]] <- gini_function(data = l_svy_df, date_year = 2000 + i)
}

gini_out_repl <- bind_rows(gini_list, .id = "year")

gini_out_repl <- gini_out_repl %>% 
  mutate(year = 2000 + as.numeric(year))  # cleaning year variable
```

``` r
gini_out <- readr::read_rds(paste0(data_url, "dt_gini_out.Rds"))

waldo::compare(gini_out, gini_out_repl)
```

    `class(old)`: "data.table" "data.frame"
    `class(new)`:              "data.frame"

    old vs new
                         gini
    - old[1, ]   6.829707e-01
    + new[1, ]  -1.445209e+15
    - old[2, ]   6.421103e-01
    + new[2, ]  -1.722823e+15
    - old[3, ]   5.983021e-01
    + new[3, ]  -2.027573e+15
    - old[4, ]   5.448121e-01
    + new[4, ]  -2.430473e+15
    - old[5, ]   4.890014e-01
    + new[5, ]  -2.895713e+15
    - old[6, ]   4.334923e-01
    + new[6, ]  -3.392194e+15
    - old[7, ]   3.874237e-01
    + new[7, ]  -3.924613e+15
    - old[8, ]   3.431090e-01
    + new[8, ]  -4.407669e+15
    - old[9, ]   3.058074e-01
    + new[9, ]  -4.948356e+15
    - old[10, ]  2.708267e-01
    + new[10, ] -5.465136e+15

         old$gini | new$gini              
     [1] 1        - -1445208563502992 [1] 
     [2] 1        - -1722822993246119 [2] 
     [3] 1        - -2027572801325370 [3] 
     [4] 1        - -2430472807314233 [4] 
     [5] 0        - -2895712590383592 [5] 
     [6] 0        - -3392194382668399 [6] 
     [7] 0        - -3924613249173104 [7] 
     [8] 0        - -4407669471876222 [8] 
     [9] 0        - -4948355880182002 [9] 
    [10] 0        - -5465135659528012 [10]

I didn’t manage to replicate the gini coefficient values.

In fact, the values are negative which indicates the formula is clearly
not right but I couldn’t find the correct solution.

**replicating figure**:

``` r
ggplot(
      # dataset and x and y variables for line and data point
      gini_out, aes(x = year, y = gini)) + 
      # titles and aesthetics
      ggtitle("Gini Coefficient 2001-2010") +
      theme_pander() +
      scale_x_continuous(name = "Year", breaks = seq(2001, 2010, 2)) +
      scale_y_continuous(name = "Cumulative Welfare", breaks = seq(0.3, 0.7, 0.1)) +

      # adding line colored by poverty line 
      geom_line(linewidth = 0.5) +
      # adding data points: 
      geom_point(size = 0.5) + 

  # formatting title, axis titles and legend
   theme(plot.title = element_text(color = "dark blue", size = 13, face = "bold", hjust = 0.5),
         axis.title.x = element_text(color = "midnightblue", size = 10, face = "bold", hjust = 0.5), 
         axis.title.y = element_text(color = "midnightblue", size = 10, face = "bold", vjust = 0.5), 
         axis.text.x = element_text(color = "midnightblue", size = 9), 
         axis.text.y = element_text(color = "midnightblue", size = 9), 
         legend.position = "bottom", 
         legend.direction = "horizontal",
         legend.background=element_blank(),
         legend.title = element_blank(),
         legend.text = element_text(color = "midnightblue", size = 8))
```

![](R-Test_files/figure-commonmark/unnamed-chunk-54-1.png)

Thank you for taking the time to review and assess my code.
