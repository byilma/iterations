simulations
================
Binyam Yilma
11/17/2020

## Let’s simulate something

Function

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
      x = rnorm(n = samp_size, mean = mu, sd = sigma)
    )
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
}
```

simultating something

``` r
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.33  2.69

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.89  2.87

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.12  4.14

Let’s simulate a lot a lot Let’s start w a for-loop

``` r
output = vector("list", length = 100)

for (i in 1:100) {
  output[[i]]  = sim_mean_sd(samp_size = 30)
}


bind_rows(output)
```

    ## # A tibble: 100 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ##  1  3.87  3.79
    ##  2  3.00  4.16
    ##  3  2.27  4.06
    ##  4  3.94  3.63
    ##  5  1.80  4.67
    ##  6  3.08  4.21
    ##  7  3.15  5.42
    ##  8  2.68  3.70
    ##  9  2.47  3.64
    ## 10  3.31  4.29
    ## # … with 90 more rows

Let’s use a loop function

``` r
sim_results = rerun(100, sim_mean_sd(samp_size = 30)) %>% 
  bind_rows()
```

Let’s look at results

``` r
sim_results %>% 
  ggplot(aes(x = mean)) +
  geom_density()
```

<img src="simulations_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
sim_results %>% 
  summarize(
    avg_samp_mean = mean(mean),
    sd_samp_mean = sd(mean)
  )
```

    ## # A tibble: 1 x 2
    ##   avg_samp_mean sd_samp_mean
    ##           <dbl>        <dbl>
    ## 1          2.91        0.725

``` r
sim_results %>% 
  ggplot(aes(x = sd)) +
  geom_density()
```

<img src="simulations_files/figure-gfm/unnamed-chunk-5-2.png" width="90%" />

## Let’s try other sample sizes

``` r
n_list = 
  list(
    "n = 30" = 30,
    "n = 60" = 60,
    "n = 120" = 120,
    "n = 240" = 240
  )



output = vector("list", length = 4)

output[[1]] = rerun(100, sim_mean_sd(samp_size = n_list[[1]])) %>% bind_rows()
output[[2]] = rerun(100, sim_mean_sd(samp_size = n_list[[2]])) %>% bind_rows()

for(i in 1:4) {
  output[[i]] = rerun(100, sim_mean_sd(samp_size = n_list[[i]])) %>% 
    bind_rows()
}
```

``` r
sim_results = tibble(
  sample_size = c(30, 60, 120, 240),
  
) %>% mutate(
  output_lists = map(.x = sample_size, ~ rerun(1000, sim_mean_sd(samp_size = .x ))),
  estimate_dfs = map(output_lists, bind_rows)
) %>% 
  select(-output_lists) %>% 
  unnest(estimate_dfs)
```

This `sim_results` is just a data frame ^^^

Let’s do some data science on this Data Frame

``` r
sim_results %>% 
  mutate(
    sample_size = str_c("n = ", sample_size), 
    sample_size = fct_inorder(sample_size)
  ) %>% 
  ggplot(aes(x = sample_size, y = mean, fill = sample_size)) + 
  geom_boxplot()
```

<img src="simulations_files/figure-gfm/unnamed-chunk-8-1.png" width="90%" />

``` r
sim_results %>% 
  mutate(
    sample_size = str_c("n = ", sample_size), 
    sample_size = fct_inorder(sample_size)
  ) %>% 
  ggplot(aes(x = sample_size, y = mean, fill = sample_size)) + 
  geom_violin()
```

<img src="simulations_files/figure-gfm/unnamed-chunk-8-2.png" width="90%" />

``` r
sim_results %>% 
  group_by(sample_size) %>% 
  summarize(
    avg_samp_mean = mean(mean),
    sd_samp_mean = sd(mean)
  )
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 4 x 3
    ##   sample_size avg_samp_mean sd_samp_mean
    ##         <dbl>         <dbl>        <dbl>
    ## 1          30          3.00        0.707
    ## 2          60          3.02        0.516
    ## 3         120          2.99        0.369
    ## 4         240          3.00        0.257
