Lab 02 - Plastic waste
================
Simon Groulx
16/09/2025

## Chargement des packages et des données

``` r
library(tidyverse) 
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.2
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

    ## Rows: 240 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (3): code, entity, continent
    ## dbl (7): year, gdp_per_cap, plastic_waste_per_cap, mismanaged_plastic_waste_...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Commençons par filtrer les données pour retirer le point représenté par
Trinité et Tobago (TTO) qui est un outlier.

``` r
plastic_waste <- plastic_waste %>%
  filter(plastic_waste_per_cap < 3.5)
```

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.2) +
  facet_wrap(~ continent, ncol = 3)
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

### Exercise 2

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap,
                          color = continent,
                          fill = continent)) +
  geom_density(alpha = 0.5)
```

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

“alpha” n’est pas au même endroit que “color” et “fill” puisque,
contrairement au deux autres, je veux que la transparence de chaque
courbe soit identique, alors que je veux qu’elles soient de couleurs
différentes (une couleur pour chacune des variables).

### Exercise 3

Boxplot:

``` r
ggplot(plastic_waste, aes(x = continent,
                          y = plastic_waste_per_cap)) +
  geom_boxplot()
```

![](lab-02_files/figure-gfm/plastic-waste-boxplot-1.png)<!-- -->

Violin plot:

``` r
ggplot(plastic_waste, aes(x = continent,
                          y = plastic_waste_per_cap)) +
  geom_violin()
```

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

Les violins plots nous permettent de voir comment est distribué le 50%
des données qui se trouve dans le violon, alors que les box plots nous
montrent seulement l’étendu de ce 50% et où se trouve la médiane.

### Exercise 4

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap,
                          y = mismanaged_plastic_waste_per_cap, 
                          color = continent)) +
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

J’observe que de manière générale, plus un pays génère de déchets, plus
sa quantité de déchets non gérés augmente.

### Exercise 5

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap,
                          y = total_pop,
                          color = continent)) +
  geom_point()
```

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(plastic_waste, aes(x = plastic_waste_per_cap,
                          y = coastal_pop,
                          color = continent)) +
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

La tendence qu’un pays produise plus de déchets plus sa population est
grande devient plus significative si nous regardons seulement la
population vivant près d’une côte.

## Conclusion

Recréez la visualisation:

``` r
ggplot(plastic_waste, aes(x = coastal_pop / total_pop,
                          y = plastic_waste_per_cap,
                          color = continent)) +
  geom_point() + geom_smooth(aes(group = 1),color = "black",  se = TRUE) +
  labs(title = "Quantité de déchets plastiques vs Proportion de la population côtière",
       subtitle = "Selon le continent",
       x = "Proportion de la population côtière (Coastal / total population)", y = "Nombre de déchets plastiques par habitant",
       color = "Continent")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- -->
