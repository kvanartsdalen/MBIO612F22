
# MBIO 612 Practical 3: Data Visualization

## Kathryn Van Artsdalen

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

**Question 1**

-   Provide at least two methods to view the number of rows and the
    number of columns in the `mpg` dataset

``` r
# Write your code here
data(mpg)
# First way is to use nrow and ncol functions to count the number of rows and columns, respectively
ncol(mpg) 
```

    ## [1] 11

``` r
nrow(mpg)
```

    ## [1] 234

``` r
#Second way by calling dimensions of the mpg dataset
dim(mpg)
```

    ## [1] 234  11

``` r
# Third way is to make a vector of the column names and use the length function to count the number of characters/objects in the vector. Repeat for row names.
mpg_columns <- colnames(mpg)
length(mpg_columns)
```

    ## [1] 11

``` r
mpg_rows <- rownames(mpg)
length(mpg_rows)
```

    ## [1] 234

**Question 2**

-   Run the following code:

``` r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
```

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
\* What is wrong with the code. \* Fix it so that the plot is displayed
correctly.

The code uses the color aesthetic inside the aes() but does not group
based on a category, such as class, so there is no differentiation
between variables. There is not way to scale “blue” since “blue” is not
a variable name. Color = “blue” actually returns data points that are
red, and does not convey any additional information about them. To
change the color of all points to blue, color must be listed outside
aes() so it treats all variables the same.

``` r
# Write your code here
# To color by a variable (such as class)
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
# To color so all points are blue
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

**Question 3**

-   Change the code below so that mpg\$class_numeric will be of type
    integer (int) rather than double (dbl)

``` r
# mpg$class_numeric = sapply(mpg$class, convert_class_int, USE.NAMES = FALSE)
```

-   Inspect the result to make sure your solution worked

``` r
# Write your code here
mpg$class_numeric <- as.integer(factor(
  mpg$class,
  levels = c("2seater", "subcompact", "compact", "midsize", "minivan", "suv", "pickup"),
  ordered = TRUE
))
```

**Question 4**

-   Use a scatter plot to plot the variable `cty` (`x`-axis) and `hwy`
    (`y`-axis) of the `mpg` dataset
-   Add another geom that shows the liner regression between `x` and `y`
    -   The method for computing linear regression should be a linear
        model `lm` instead of the default (`loess`)
    -   Set the line color to `red`
    -   color the points by `class`
    -   set the points size to 6

``` r
# Write your code here
ggplot(data = mpg, mapping = aes(x = cty, y = hwy))+
  geom_point(mapping = aes(color = class), size = 6)+
  geom_smooth(method = "lm", color = "red")+
  labs(title = "City vs Highway Fuel Consumption by Car Class", x = "City Mileage in mpg", y = "Highway Mileage in mpg")+
  theme_classic()
```

    ## `geom_smooth()` using formula 'y ~ x'

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

**Question 5**

-   You noticed in the previous plot that there exist in the dataset
    subcompacts that has a city fuel consumption of 15 miles per gallon.
-   Extract the manufacturer and model of these subcompacts
    -   results should only have these columns

``` r
# Write your code here
mpg[which(mpg$cty == 15 & mpg$class == "subcompact"), c("manufacturer", "model")]
```

    ## # A tibble: 4 × 2
    ##   manufacturer model  
    ##   <chr>        <chr>  
    ## 1 ford         mustang
    ## 2 ford         mustang
    ## 3 ford         mustang
    ## 4 ford         mustang

**Question 6**

-   The following generates a warning

``` r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

    ## Warning: The shape palette can deal with a maximum of 6 discrete values because
    ## more than 6 becomes difficult to discriminate; you have 7. Consider
    ## specifying shapes manually if you must have them.

    ## Warning: Removed 62 rows containing missing values (geom_point).

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

-   How can you fix that the code so all classes are displayed
-   Hint: you can manually specify your own values for the shapes to use
    using `scale_shape_manual` geom. You can select any of the
    [following
    shapes](https://ggplot2.tidyverse.org/articles/ggplot2-specs_files/figure-html/unnamed-chunk-7-1.png)

``` r
# Write your code here
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))+
  scale_shape_manual(values = c(0,1,5,8,15,16,17))+
  labs(title = "Engine Displacement and Highway Mileage by Car Class",
       x = "Engine Displacement in litres",
       y = "Highway Mileage in mpg")+
  theme_classic()
```

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

**Question 7**

-   Given the following two vectors

``` r
x <- c(3.385,0.48,1.35,465,36.33,27.66,14.83,1.04,4.19,0.425,0.101,0.92,1,0.005,0.06,3.5,2,1.7,2547,0.023,187.1,521,0.785,10,3.3,0.2,1.41,529,207,85,0.75,62,6654,3.5,6.8,35,4.05,0.12,0.023,0.01,1.4,250,2.5,55.5,100,52.16,10.55,0.55,60,3.6,4.288,0.28,0.075,0.122,0.048,192,3,160,0.9,1.62,0.104,4.235)
y <- c(44.5,15.5,8.1,423,119.5,115,98.2,5.5,58,6.4,4,5.7,6.6,0.14,1,10.8,12.3,6.3,4603,0.3,419,655,3.5,115,25.6,5,17.5,680,406,325,12.3,1320,5712,3.9,179,56,17,1,0.4,0.25,12.5,490,12.1,175,157,440,179.5,2.4,81,21,39.2,1.9,1.2,3,0.33,180,25,169,2.6,11.4,2.5,50.4)
```

-   Combine the two vectors into a data frame called `sizeMass`
    -   Name the columns containing the vectors `x` and `y` as `size`
        and `mass` respectively
-   Use the code below to plot the data
    -   If you followed the above instructions the code should work
        without modification

``` r
sizeMass <- data.frame(size = x, mass = y)
```

``` r
ggplot(sizeMass, aes(size, mass)) +
  geom_point() +
  geom_smooth(method = "lm", color = "red", se = FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

**Question 8**

-   In the above, the data is “squashed” because the data range varies
    substantially across observations. One way to fix this is by
    computing the log of the data. We will do that using two different
    approaches

1.  add two new columns called them `size_log10` and `mass_log` that
    represent the log 10 values of the `size` and `mass` columns. Then,
    reuse the plotting code above by replacing `size` and `mass` with
    `size_log10` and `mass_log10` respectively

-   Hint `log10()` can take as input a single value or a vector

2.  Use the `scale_x_log10()` and `scale_y_log10()` to set different
    scales for the `x` and `y` aesthetics

-   Simply add these transforms to the initial snippet of code prior to
    plotting `geom_point` or `geom_smooth`

``` r
# Method 1
sizeMass.log10 <- cbind(sizeMass, size_log10 = log10(x), mass_log10 = log10(y))
ggplot(sizeMass.log10, aes(size_log10, mass_log10)) +
  geom_point() +
  geom_smooth(method = "lm", color = "red", se = FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
# Method 2
ggplot(sizeMass, aes(size, mass)) +
  scale_x_log10()+
  scale_y_log10()+
  geom_point() +
  geom_smooth(method = "lm", color = "red", se = FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](data_visualization_practicalKV_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
