Data Visualization Examples
================

``` r
#load packages
#install.packages("palmerpenguins")
library(palmerpenguins)

#install.packages("ggthemes")
library(ggthemes)

library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.0     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

## Basic Scatterplot

``` r
ggplot(
  data = penguins,
  mapping = aes(
    x = flipper_length_mm,
    y = body_mass_g,
    color = species
  )
) +
  geom_point()
```

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

\`\` \## Scatterplot with Line of Best Fit and Correlation by Species

``` r
ggplot(
  data = penguins,
  mapping = aes(
    x = flipper_length_mm,
    y = body_mass_g,
    color = species # color here
  )
) +
  geom_point() +
  geom_smooth(method = "lm")
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 2 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Scatterplot with Line of Best Fit and Correlation by Species

``` r
ggplot(
  data = penguins,
  mapping = aes(
    x = bill_length_mm,
    y = bill_depth_mm,
    color = species # color here
  )
) +
  geom_point() +
  geom_smooth(method = "lm")
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 2 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
cor.test(penguins$bill_length_mm, penguins$bill_depth_mm)
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  penguins$bill_length_mm and penguins$bill_depth_mm
    ## t = -4.4591, df = 340, p-value = 1.12e-05
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.3328072 -0.1323004
    ## sample estimates:
    ##        cor 
    ## -0.2350529

``` r
ggplot(
  data = penguins,
  mapping = aes(
    x = flipper_length_mm,
    y = body_mass_g
  )) + 
  geom_point(aes(colour = bill_depth_mm), na.rm = TRUE) + 
  geom_smooth(na.rm = TRUE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Overlayed Density

``` r
ggplot(penguins, aes(
  x = body_mass_g,
  color = species,
  fill = species
)) +
  geom_density(alpha = 0.5)
```

    ## Warning: Removed 2 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Boxplot

``` r
ggplot(penguins, aes(
  y = species,
  x = body_mass_g
)) +
  geom_boxplot() 
```

    ## Warning: Removed 2 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Barcharts

``` r
### Grouped
penguins |>
  ggplot(aes(fill = sex, x = species)) +
  geom_bar(position = "dodge")             # dodge
```

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
### Stacked
penguins |>
  ggplot(aes(fill = sex, x = species)) +
  geom_bar()
```

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

``` r
### Percent Stacked
penguins |>
  ggplot(aes(fill = sex, x = species)) +
  geom_bar(position = "fill")              
```

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->
\## Using facets to view data differently

``` r
penguins |>
  na.omit(sex) |>
  ggplot(aes(
    x = flipper_length_mm,
    y = body_mass_g
  )) +
  geom_point(aes(
    color = species,
    shape = species
  )) +
  facet_wrap(~sex)
```

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
\## Facet Wrap with trend lines for islands

``` r
penguins |>
  na.omit() |>
  ggplot(aes(
    x = flipper_length_mm,
    y = body_mass_g
  )) +
  geom_point(aes(
    color = species,
    shape = species
  )) +
  geom_smooth() +
  facet_wrap(~island)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
\## Filtered with dplyr before graphing
![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
\## MPG filtered by drivetrain and displ

``` r
ggplot(mpg, aes(x = displ, y = hwy, color = drv)) + geom_point() + geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
ggplot(penguins, aes(

  x = bill_length_mm,

  y = bill_depth_mm

)) +

  geom_point() +

  facet_wrap(~species)
```

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Data-Visualizations-in-R_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
