reading\_data
================
Yu
October 31, 2020

Scape a table
-------------

Iwant to the first table from \[this page\] (<http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>)

read in the html

``` r
url = 'http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm'

drug_use_html = read_html(url)
```

extract the table(s); focus on the first one

``` r
tab_mari = 
  drug_use_html %>% 
    html_nodes(css = 'table') %>% 
    first() %>% 
    html_table() %>% 
    slice(-1) %>% 
    as_tibble()
```

Star Wars Movie info
--------------------

``` r
url = 'https://www.imdb.com/list/ls070150896/'

swm_html = read_html(url)
```

Grab elements that I want:

``` r
titles_vec = 
  swm_html %>% 
  html_nodes(css = '.lister-item-header a') %>% 
  html_text()

gross_rev_vec = 
  swm_html %>% 
  html_nodes(css = '.text-muted .ghost~ .text-muted+ span') %>% 
  html_text()

runtime_vec = 
  swm_html %>% 
  html_nodes(css = '.runtime') %>% 
  html_text()

sum_df = 
  tibble(
    title = titles_vec, 
    gross = gross_rev_vec,
    runtime = runtime_vec
  )
```

Get some water data
-------------------

Thsis is coming from on API

``` r
nyc_water = 
  GET('https://data.cityofnewyork.us/resource/ia2d-e54m.csv') %>% 
  content('parse')
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
nyc_water = 
  GET('https://data.cityofnewyork.us/resource/ia2d-e54m.json') %>% 
  content('text') %>%
  jsonlite::fromJSON() %>% 
  as_tibble()
```

BRFSS
-----

same process, different dataset

``` r
brfss_2010 = 
 GET('https://chronicdata.cdc.gov/resource/acme-vg9e.csv',
     query = list('$limit' = 5000)) %>%  ##expamd the row numbers that we need in the dataset
  content('parsed')
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   year = col_double(),
    ##   sample_size = col_double(),
    ##   data_value = col_double(),
    ##   confidence_limit_low = col_double(),
    ##   confidence_limit_high = col_double(),
    ##   display_order = col_double(),
    ##   locationid = col_logical()
    ## )

    ## See spec(...) for full column specifications.
