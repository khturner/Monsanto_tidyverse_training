Tidy data tutorial - Day 1
========================================================
author: Keith H. Turner (Twitter: @kay_aych)
date: Thursday, May 11, 2017
css: tutorial.css


Tidy data
========================================================
Paradigm for __exploratory data analysis__ defined primarily by Hadley Wickham

![](http://r4ds.had.co.nz/diagrams/data-science.png)

Outline for the next few days
========================================================
* Day 1: Tidy data
  + Importing your data into R
  + Tidying your data
  + Transforming your tidy data
* Day 2: `ggplot` and modeling
  + Visualizing your tidy data
  + Incorporating basic modeling into your tidy workflow

These tutorials are largely based on the book *R for Data Science*, which is also available for free online at [`https://r4ds.had.co.nz`](https://r4ds.had.co.nz)

What is tidy data?
========================================================
class: small-code
* A paradigm for storing your data in a consistently structured form
* Generally, each row is an *observation*, and each column is a *variable*


```r
head(iris)
```

```
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
```

Why tidy?
========================================================
class: small-code
- Standardized framework allows your data to fit nicely into the APIs provided by base R and other key packages


```r
lm(mpg ~ disp + wt + hp + cyl + drat, data = mtcars)
```

```

Call:
lm(formula = mpg ~ disp + wt + hp + cyl + drat, data = mtcars)

Coefficients:
(Intercept)         disp           wt           hp          cyl  
   36.00836      0.01236     -3.67329     -0.02402     -1.10749  
       drat  
    0.95221  
```

The tidyverse
========================================================
class: small-code

- [The tidyverse](http://tidyverse.org/) is a meta-package of common tidy data packages maintained by [Hadley Wickham](http://hadley.nz/), Chief Scientist at RStudio
  * Run `install.packages("tidyverse")` in your R/RStudio environment of choice


```r
## Run me!
library(tidyverse)
```

```
Loading tidyverse: ggplot2
Loading tidyverse: tibble
Loading tidyverse: tidyr
Loading tidyverse: readr
Loading tidyverse: purrr
Loading tidyverse: dplyr
```

```
Conflicts with tidy packages ----------------------------------------------
```

```
filter(): dplyr, stats
lag():    dplyr, stats
```

The tibble - the central tidyverse object
========================================================
class: small-code

<img style="float: right;" src="http://tibble.tidyverse.org/logo.png">

- Wrapper of tabular data
  * data frames (can include complex columns!)
  * `SQL` tables
- From the `tibble` documentation:

```
tibble is a trimmed down version of data.frame that:
 - Never coerces inputs (i.e. strings stay as strings!).
 - Never adds row.names.
 - Never munges column names.
 - Only recycles length 1 inputs.
 - Evaluates its arguments lazily and in order.
 - Adds tbl_df class to output.
 - Automatically adds column names.
```

Create a tibble de novo
========================================================
class: small-code


```r
tibble(treatment = sample(letters, 10),
       expression_value = rnorm(10, 100, 10))
```

```
# A tibble: 10 × 2
   treatment expression_value
       <chr>            <dbl>
1          k        110.15946
2          v        108.75928
3          y         89.50856
4          j        112.67519
5          h        109.78504
6          o        121.23831
7          g        102.84156
8          i         86.93887
9          w        111.46160
10         x        106.98439
```
 - Never coerces inputs (i.e. strings stay as strings!).
   * Remember to coerce to factor if you need it (i.e. for modeling)
 - Adds tbl_df class to output.
   *  Pretty printing!

Examining data frames in base R is cumbersome
========================================================
class: small-code


```r
dim(iris)
```

```
[1] 150   5
```

```r
str(iris)
```

```
'data.frame':	150 obs. of  5 variables:
 $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```r
iris
```

```
    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1            5.1         3.5          1.4         0.2     setosa
2            4.9         3.0          1.4         0.2     setosa
3            4.7         3.2          1.3         0.2     setosa
4            4.6         3.1          1.5         0.2     setosa
5            5.0         3.6          1.4         0.2     setosa
6            5.4         3.9          1.7         0.4     setosa
7            4.6         3.4          1.4         0.3     setosa
8            5.0         3.4          1.5         0.2     setosa
9            4.4         2.9          1.4         0.2     setosa
10           4.9         3.1          1.5         0.1     setosa
11           5.4         3.7          1.5         0.2     setosa
12           4.8         3.4          1.6         0.2     setosa
13           4.8         3.0          1.4         0.1     setosa
14           4.3         3.0          1.1         0.1     setosa
15           5.8         4.0          1.2         0.2     setosa
16           5.7         4.4          1.5         0.4     setosa
17           5.4         3.9          1.3         0.4     setosa
18           5.1         3.5          1.4         0.3     setosa
19           5.7         3.8          1.7         0.3     setosa
20           5.1         3.8          1.5         0.3     setosa
21           5.4         3.4          1.7         0.2     setosa
22           5.1         3.7          1.5         0.4     setosa
23           4.6         3.6          1.0         0.2     setosa
24           5.1         3.3          1.7         0.5     setosa
25           4.8         3.4          1.9         0.2     setosa
26           5.0         3.0          1.6         0.2     setosa
27           5.0         3.4          1.6         0.4     setosa
28           5.2         3.5          1.5         0.2     setosa
29           5.2         3.4          1.4         0.2     setosa
30           4.7         3.2          1.6         0.2     setosa
31           4.8         3.1          1.6         0.2     setosa
32           5.4         3.4          1.5         0.4     setosa
33           5.2         4.1          1.5         0.1     setosa
34           5.5         4.2          1.4         0.2     setosa
35           4.9         3.1          1.5         0.2     setosa
36           5.0         3.2          1.2         0.2     setosa
37           5.5         3.5          1.3         0.2     setosa
38           4.9         3.6          1.4         0.1     setosa
39           4.4         3.0          1.3         0.2     setosa
40           5.1         3.4          1.5         0.2     setosa
41           5.0         3.5          1.3         0.3     setosa
42           4.5         2.3          1.3         0.3     setosa
43           4.4         3.2          1.3         0.2     setosa
44           5.0         3.5          1.6         0.6     setosa
45           5.1         3.8          1.9         0.4     setosa
46           4.8         3.0          1.4         0.3     setosa
47           5.1         3.8          1.6         0.2     setosa
48           4.6         3.2          1.4         0.2     setosa
49           5.3         3.7          1.5         0.2     setosa
50           5.0         3.3          1.4         0.2     setosa
51           7.0         3.2          4.7         1.4 versicolor
52           6.4         3.2          4.5         1.5 versicolor
53           6.9         3.1          4.9         1.5 versicolor
54           5.5         2.3          4.0         1.3 versicolor
55           6.5         2.8          4.6         1.5 versicolor
56           5.7         2.8          4.5         1.3 versicolor
57           6.3         3.3          4.7         1.6 versicolor
58           4.9         2.4          3.3         1.0 versicolor
59           6.6         2.9          4.6         1.3 versicolor
60           5.2         2.7          3.9         1.4 versicolor
61           5.0         2.0          3.5         1.0 versicolor
62           5.9         3.0          4.2         1.5 versicolor
63           6.0         2.2          4.0         1.0 versicolor
64           6.1         2.9          4.7         1.4 versicolor
65           5.6         2.9          3.6         1.3 versicolor
66           6.7         3.1          4.4         1.4 versicolor
67           5.6         3.0          4.5         1.5 versicolor
68           5.8         2.7          4.1         1.0 versicolor
69           6.2         2.2          4.5         1.5 versicolor
70           5.6         2.5          3.9         1.1 versicolor
71           5.9         3.2          4.8         1.8 versicolor
72           6.1         2.8          4.0         1.3 versicolor
73           6.3         2.5          4.9         1.5 versicolor
74           6.1         2.8          4.7         1.2 versicolor
75           6.4         2.9          4.3         1.3 versicolor
76           6.6         3.0          4.4         1.4 versicolor
77           6.8         2.8          4.8         1.4 versicolor
78           6.7         3.0          5.0         1.7 versicolor
79           6.0         2.9          4.5         1.5 versicolor
80           5.7         2.6          3.5         1.0 versicolor
81           5.5         2.4          3.8         1.1 versicolor
82           5.5         2.4          3.7         1.0 versicolor
83           5.8         2.7          3.9         1.2 versicolor
84           6.0         2.7          5.1         1.6 versicolor
85           5.4         3.0          4.5         1.5 versicolor
86           6.0         3.4          4.5         1.6 versicolor
87           6.7         3.1          4.7         1.5 versicolor
88           6.3         2.3          4.4         1.3 versicolor
89           5.6         3.0          4.1         1.3 versicolor
90           5.5         2.5          4.0         1.3 versicolor
91           5.5         2.6          4.4         1.2 versicolor
92           6.1         3.0          4.6         1.4 versicolor
93           5.8         2.6          4.0         1.2 versicolor
94           5.0         2.3          3.3         1.0 versicolor
95           5.6         2.7          4.2         1.3 versicolor
96           5.7         3.0          4.2         1.2 versicolor
97           5.7         2.9          4.2         1.3 versicolor
98           6.2         2.9          4.3         1.3 versicolor
99           5.1         2.5          3.0         1.1 versicolor
100          5.7         2.8          4.1         1.3 versicolor
101          6.3         3.3          6.0         2.5  virginica
102          5.8         2.7          5.1         1.9  virginica
103          7.1         3.0          5.9         2.1  virginica
104          6.3         2.9          5.6         1.8  virginica
105          6.5         3.0          5.8         2.2  virginica
106          7.6         3.0          6.6         2.1  virginica
107          4.9         2.5          4.5         1.7  virginica
108          7.3         2.9          6.3         1.8  virginica
109          6.7         2.5          5.8         1.8  virginica
110          7.2         3.6          6.1         2.5  virginica
111          6.5         3.2          5.1         2.0  virginica
112          6.4         2.7          5.3         1.9  virginica
113          6.8         3.0          5.5         2.1  virginica
114          5.7         2.5          5.0         2.0  virginica
115          5.8         2.8          5.1         2.4  virginica
116          6.4         3.2          5.3         2.3  virginica
117          6.5         3.0          5.5         1.8  virginica
118          7.7         3.8          6.7         2.2  virginica
119          7.7         2.6          6.9         2.3  virginica
120          6.0         2.2          5.0         1.5  virginica
121          6.9         3.2          5.7         2.3  virginica
122          5.6         2.8          4.9         2.0  virginica
123          7.7         2.8          6.7         2.0  virginica
124          6.3         2.7          4.9         1.8  virginica
125          6.7         3.3          5.7         2.1  virginica
126          7.2         3.2          6.0         1.8  virginica
127          6.2         2.8          4.8         1.8  virginica
128          6.1         3.0          4.9         1.8  virginica
129          6.4         2.8          5.6         2.1  virginica
130          7.2         3.0          5.8         1.6  virginica
131          7.4         2.8          6.1         1.9  virginica
132          7.9         3.8          6.4         2.0  virginica
133          6.4         2.8          5.6         2.2  virginica
134          6.3         2.8          5.1         1.5  virginica
135          6.1         2.6          5.6         1.4  virginica
136          7.7         3.0          6.1         2.3  virginica
137          6.3         3.4          5.6         2.4  virginica
138          6.4         3.1          5.5         1.8  virginica
139          6.0         3.0          4.8         1.8  virginica
140          6.9         3.1          5.4         2.1  virginica
141          6.7         3.1          5.6         2.4  virginica
142          6.9         3.1          5.1         2.3  virginica
143          5.8         2.7          5.1         1.9  virginica
144          6.8         3.2          5.9         2.3  virginica
145          6.7         3.3          5.7         2.5  virginica
146          6.7         3.0          5.2         2.3  virginica
147          6.3         2.5          5.0         1.9  virginica
148          6.5         3.0          5.2         2.0  virginica
149          6.2         3.4          5.4         2.3  virginica
150          5.9         3.0          5.1         1.8  virginica
```

Coerce a data frame to a tibble, now it's pretty!
========================================================
class: small-code


```r
tibble_iris <- tbl_df(iris)
tibble_iris
```

```
# A tibble: 150 × 5
   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
          <dbl>       <dbl>        <dbl>       <dbl>  <fctr>
1           5.1         3.5          1.4         0.2  setosa
2           4.9         3.0          1.4         0.2  setosa
3           4.7         3.2          1.3         0.2  setosa
4           4.6         3.1          1.5         0.2  setosa
5           5.0         3.6          1.4         0.2  setosa
6           5.4         3.9          1.7         0.4  setosa
7           4.6         3.4          1.4         0.3  setosa
8           5.0         3.4          1.5         0.2  setosa
9           4.4         2.9          1.4         0.2  setosa
10          4.9         3.1          1.5         0.1  setosa
# ... with 140 more rows
```

 - Note that column types are *maintained* when coercing an already-typed object like a data frame

Outline
========================================================
* Day 1: Tidy data
  + **Importing your data into R**
  + Tidying your data
  + Transforming your tidy data

readr - read tabular data in as a tibble
========================================================
class: small-code

<img style="float: right;" src="http://readr.tidyverse.org/logo.png">

- Main functions are `read_*`
  * `csv`, `tsv`, `delim`, `fwf`, etc.
- Faster than `read.*`, but not `data.table::fread`
- Nice column parsers, reads data as a tibble


```r
read_csv("https://query.data.world/s/v8bzksoqgqyinhqd4u6lhruu") # file or URL
```

```
# A tibble: 157,462 × 13
      id episode_id number
   <int>      <int>  <int>
1   9549         32    209
2   9550         32    210
3   9551         32    211
4   9552         32    212
5   9553         32    213
6   9554         32    214
7   9555         32    215
8   9556         32    216
9   9557         32    217
10  9558         32    218
# ... with 157,452 more rows, and 10 more variables: raw_text <chr>,
#   timestamp_in_ms <int>, speaking_line <chr>, character_id <int>,
#   location_id <int>, raw_character_text <chr>, raw_location_text <chr>,
#   spoken_words <chr>, normalized_text <chr>, word_count <chr>
```

readr - read tabular data in as a tibble
========================================================
class: small-code

- Can also take raw text


```r
read_csv("date,restaurant,num_visitors
         2017-04-01,Yaya's Euro Bistro,100
         2017-04-02,Elaia,50")
```

```
# A tibble: 2 × 3
        date         restaurant num_visitors
      <date>              <chr>        <int>
1 2017-04-01 Yaya's Euro Bistro          100
2 2017-04-02              Elaia           50
```

- *Useful for streaming data files from S3*


```r
library(aws.s3)
df <- read_csv(rawToChar(get_object("kturn1/POC.sourcing.metadata.csv",
                                    bucket = "genome-analytics-scratch-space")))
```

Messy data with readr
========================================================
class: small-code

- Skip a few lines


```r
read_csv("The first line of metadata
  The second line of metadata
  x,y,z
  1,2,3", skip = 2)
```

```
# A tibble: 1 × 3
      x     y     z
  <int> <int> <int>
1     1     2     3
```

- Comments


```r
read_csv("# A comment I want to skip
  x,y,z
  1,2,3", comment = "#")
```

```
# A tibble: 1 × 3
      x     y     z
  <int> <int> <int>
1     1     2     3
```

Column names and types
========================================================

- Names guessed from first line if `col_names = TRUE` (default)
  - Otherwise, `col_names = c("Var1", "Var2")`
- Types guessed from first 1,000 lines of your code
  - See `vignette("column-types")` for more details.
  - Or you can specify, one letter per column: `col_types = "ciddlD"`

Putting it all together - GFF file
========================================================
class: small-code

- `readr` functions can handle gzipped files


```r
sg <- read_tsv("https://ftp.ncbi.nlm.nih.gov/genomes/refseq/bacteria/Streptococcus_gordonii/latest_assembly_versions/GCF_002073435.1_ASM207343v1/GCF_002073435.1_ASM207343v1_genomic.gff.gz",
               comment = "#",
               col_names = c("seqname", "source", "feature", "start", "end", "score", "strand", "frame", "attribute"))
sg
```

```
# A tibble: 4,449 × 9
         seqname           source feature start     end score strand frame
           <chr>            <chr>   <chr> <int>   <int> <chr>  <chr> <chr>
1  NZ_CP020450.1           RefSeq  region     1 2222466     .      +     .
2  NZ_CP020450.1           RefSeq    gene   526    1875     .      +     .
3  NZ_CP020450.1 Protein Homology     CDS   526    1875     .      +     0
4  NZ_CP020450.1           RefSeq    gene  2199    2930     .      -     .
5  NZ_CP020450.1 Protein Homology     CDS  2199    2930     .      -     0
6  NZ_CP020450.1           RefSeq    gene  2948    4089     .      -     .
7  NZ_CP020450.1 Protein Homology     CDS  2948    4089     .      -     0
8  NZ_CP020450.1           RefSeq    gene  4523    5077     .      +     .
9  NZ_CP020450.1 Protein Homology     CDS  4523    5077     .      +     0
10 NZ_CP020450.1           RefSeq    gene  5090    5302     .      -     .
# ... with 4,439 more rows, and 1 more variables: attribute <chr>
```

readxl - similar functionality for Excel files
========================================================
class: small-code

<img style="float: right;" src="http://readxl.tidyverse.org/logo.png">

- Work in progress to make it more similar to `readr`
  - Currently can't work on text streams, URLs
<br>
<br>


```r
## Run me!
library(readxl)
eurovision <- read_excel("data/eurovision_song_contest_1975_2016.xlsx")
eurovision
```

```
# A tibble: 38,181 × 7
    year competition_round edition jury_or_televoting from_country
   <dbl>             <chr>   <chr>              <chr>        <chr>
1   1975                 f   1975f                  J      Belgium
2   1975                 f   1975f                  J      Belgium
3   1975                 f   1975f                  J      Belgium
4   1975                 f   1975f                  J      Belgium
5   1975                 f   1975f                  J      Belgium
6   1975                 f   1975f                  J      Belgium
7   1975                 f   1975f                  J      Belgium
8   1975                 f   1975f                  J      Belgium
9   1975                 f   1975f                  J      Belgium
10  1975                 f   1975f                  J      Belgium
# ... with 38,171 more rows, and 2 more variables: to_country <chr>,
#   points <dbl>
```

Data import exercise 1
========================================================
class: small-code

- Goal: read in a CSV of flights to/from New York City in 2013
  - File location: `data/flights.csv`

<iframe src="//giphy.com/embed/o5oLImoQgGsKY" width="480" height="264" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

Data import exercise 1
========================================================
class: small-code

- Goal: read in a CSV of flights to/from New York City in 2013
  - File location: `data/flights.csv`


```r
## Run me!
library(tidyverse)
flights <- read_csv("data/flights.csv")
flights
```

```
# A tibble: 336,776 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     1      517            515         2      830
2   2013     1     1      533            529         4      850
3   2013     1     1      542            540         2      923
4   2013     1     1      544            545        -1     1004
5   2013     1     1      554            600        -6      812
6   2013     1     1      554            558        -4      740
7   2013     1     1      555            600        -5      913
8   2013     1     1      557            600        -3      709
9   2013     1     1      557            600        -3      838
10  2013     1     1      558            600        -2      753
# ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

Outline
========================================================
* Day 1: Tidy data
  + Importing your data into R
  + **Tidying your data**
  + Transforming your tidy data

Digression - piping
========================================================

<img style="float: right;" src="https://d21ii91i3y6o6h.cloudfront.net/gallery_images/from_proof/3632/small/1419844831/magrittr.png">

* The `%>%` operator, from the `magrittr` package
* *Very* useful in linear tidy data workflows
* Most simply, `x %>% f(y)` means `f(x,y)`
  * **Tip**: Read it like "then"
* Let's illustrate with an example:

```
Little bunny Foo Foo
Went hopping through the forest
Scooping up the field mice
And bopping them on the head
```

Digression - piping
========================================================
class: small-code

* Without the pipe, this is hard to program cleanly


```r
# Every intermediate as a new object
foo_foo <- little_bunny()
foo_foo_1 <- hop(foo_foo, through = forest)
foo_foo_2 <- scoop(foo_foo_1, up = field_mice)
foo_foo_3 <- bop(foo_foo_2, on = head)

# Overwrite the original object
foo_foo <- hop(foo_foo, through = forest)
foo_foo <- scoop(foo_foo, up = field_mice)
foo_foo <- bop(foo_foo, on = head)

# Nested function calls
bop(
  scoop(
    hop(foo_foo, through = forest),
    up = field_mice
  ), 
  on = head
)
```

Digression - piping
========================================================

* With the pipe, process flows logically
* Legible, understandable code


```r
foo_foo %>%
  hop(through = forest) %>%
  scoop(up = field_mouse) %>%
  bop(on = head)
```

* Focus is on verbs, not nouns
  * Hop through forest, **then** scoop up field mouse, **then** bop on head

Tidy data
========================================================
There are three interrelated rules which make a dataset tidy:

1. Each variable must have its own column.
2. Each observation must have its own row.
3. Each value must have its own cell.

![](http://r4ds.had.co.nz/images/tidy-1.png)

Most data comes to you untidy
========================================================
class: small-code




```r
cases
```

```
# A tibble: 3 × 3
      country `1999` `2000`
        <chr>  <dbl>  <dbl>
1 Afghanistan    745   2666
2      Brazil  37737  80488
3       China 212258 213766
```

In this example, `1999` and `2000` represent the number of cases in two years in these three countries

Most common operations in tidying data
========================================================

<img style="float: right;" src="http://tidyr.tidyverse.org/logo.png">

The `tidyr` package handles two common
issues in tidying data:

1. **One variable might be spread across multiple columns.**
2. One observation might be scattered across multiple rows.

Gathering
========================================================
class: small-code

* Use when column names are not names of variables, but *values* of a variable
  * Like `melt`, but more limited use case


```r
cases %>% gather(`1999`, `2000`, key = "year", value = "cases")
```

```
# A tibble: 6 × 3
      country  year  cases
        <chr> <chr>  <dbl>
1 Afghanistan  1999    745
2      Brazil  1999  37737
3       China  1999 212258
4 Afghanistan  2000   2666
5      Brazil  2000  80488
6       China  2000 213766
```

Most common operations in tidying data
========================================================

<img style="float: right;" src="http://tidyr.tidyverse.org/logo.png">

The `tidyr` package handles two common
issues in tidying data:

1. One variable might be spread across multiple columns.
2. **One observation might be scattered across multiple rows.**

Spreading
========================================================
class: small-code

* Opposite of gathering, use when one observation is spread across multiple rows
  * Like `cast`, but more limited use case




```r
to_spread
```

```
# A tibble: 12 × 4
       country  year       type      count
         <chr> <dbl>      <chr>      <dbl>
1  Afghanistan  1999      cases        745
2       Brazil  1999      cases   19987071
3        China  1999      cases       2666
4  Afghanistan  2000      cases   20595360
5       Brazil  2000      cases      37737
6        China  2000      cases  172006362
7  Afghanistan  1999 population      80488
8       Brazil  1999 population  174504898
9        China  1999 population     212258
10 Afghanistan  2000 population 1272915272
11      Brazil  2000 population     213766
12       China  2000 population 1280428583
```

Spreading
========================================================
class: small-code

* Opposite of gathering, use when one observation is spread across multiple rows
  * Like `cast`, but more limited use case
  

```r
to_spread %>% spread(key = type, value = count)
```

```
# A tibble: 6 × 4
      country  year     cases population
*       <chr> <dbl>     <dbl>      <dbl>
1 Afghanistan  1999       745      80488
2 Afghanistan  2000  20595360 1272915272
3      Brazil  1999  19987071  174504898
4      Brazil  2000     37737     213766
5       China  1999      2666     212258
6       China  2000 172006362 1280428583
```

Separating
========================================================
class: small-code

* Multiple values in a single column




```r
rate_as_char
```

```
# A tibble: 6 × 3
      country  year              rate
        <chr> <dbl>             <chr>
1 Afghanistan  1999      745/19987071
2      Brazil  1999     2666/20595360
3       China  1999   37737/172006362
4 Afghanistan  2000   80488/174504898
5      Brazil  2000 212258/1272915272
6       China  2000 213766/1280428583
```

```r
rate_as_char %>% separate(rate, into = c("cases", "population"))
```

```
# A tibble: 6 × 4
      country  year  cases population
*       <chr> <dbl>  <chr>      <chr>
1 Afghanistan  1999    745   19987071
2      Brazil  1999   2666   20595360
3       China  1999  37737  172006362
4 Afghanistan  2000  80488  174504898
5      Brazil  2000 212258 1272915272
6       China  2000 213766 1280428583
```

Separating - default behavior
========================================================
class: small-code

* By default, splits on non-alphanumeric characters


```r
rate_as_char %>% separate(rate, into = c("cases", "population"), sep = "/") # Regex
```

```
# A tibble: 6 × 4
      country  year  cases population
*       <chr> <dbl>  <chr>      <chr>
1 Afghanistan  1999    745   19987071
2      Brazil  1999   2666   20595360
3       China  1999  37737  172006362
4 Afghanistan  2000  80488  174504898
5      Brazil  2000 212258 1272915272
6       China  2000 213766 1280428583
```

Separating - default behavior
========================================================
class: small-code

* And does not convert new columns


```r
rate_as_char %>% separate(rate, into = c("cases", "population"), convert = T)
```

```
# A tibble: 6 × 4
      country  year  cases population
*       <chr> <dbl>  <int>      <int>
1 Afghanistan  1999    745   19987071
2      Brazil  1999   2666   20595360
3       China  1999  37737  172006362
4 Afghanistan  2000  80488  174504898
5      Brazil  2000 212258 1272915272
6       China  2000 213766 1280428583
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Tuberculosis (TB) cases broken down by year, country, age, gender, and diagnosis method


```r
who
```

```
# A tibble: 7,240 × 60
       country  iso2  iso3  year new_sp_m014 new_sp_m1524 new_sp_m2534
         <chr> <chr> <chr> <int>       <int>        <int>        <int>
1  Afghanistan    AF   AFG  1980          NA           NA           NA
2  Afghanistan    AF   AFG  1981          NA           NA           NA
3  Afghanistan    AF   AFG  1982          NA           NA           NA
4  Afghanistan    AF   AFG  1983          NA           NA           NA
5  Afghanistan    AF   AFG  1984          NA           NA           NA
6  Afghanistan    AF   AFG  1985          NA           NA           NA
7  Afghanistan    AF   AFG  1986          NA           NA           NA
8  Afghanistan    AF   AFG  1987          NA           NA           NA
9  Afghanistan    AF   AFG  1988          NA           NA           NA
10 Afghanistan    AF   AFG  1989          NA           NA           NA
# ... with 7,230 more rows, and 53 more variables: new_sp_m3544 <int>,
#   new_sp_m4554 <int>, new_sp_m5564 <int>, new_sp_m65 <int>,
#   new_sp_f014 <int>, new_sp_f1524 <int>, new_sp_f2534 <int>,
#   new_sp_f3544 <int>, new_sp_f4554 <int>, new_sp_f5564 <int>,
#   new_sp_f65 <int>, new_sn_m014 <int>, new_sn_m1524 <int>,
#   new_sn_m2534 <int>, new_sn_m3544 <int>, new_sn_m4554 <int>,
#   new_sn_m5564 <int>, new_sn_m65 <int>, new_sn_f014 <int>,
#   new_sn_f1524 <int>, new_sn_f2534 <int>, new_sn_f3544 <int>,
#   new_sn_f4554 <int>, new_sn_f5564 <int>, new_sn_f65 <int>,
#   new_ep_m014 <int>, new_ep_m1524 <int>, new_ep_m2534 <int>,
#   new_ep_m3544 <int>, new_ep_m4554 <int>, new_ep_m5564 <int>,
#   new_ep_m65 <int>, new_ep_f014 <int>, new_ep_f1524 <int>,
#   new_ep_f2534 <int>, new_ep_f3544 <int>, new_ep_f4554 <int>,
#   new_ep_f5564 <int>, new_ep_f65 <int>, newrel_m014 <int>,
#   newrel_m1524 <int>, newrel_m2534 <int>, newrel_m3544 <int>,
#   newrel_m4554 <int>, newrel_m5564 <int>, newrel_m65 <int>,
#   newrel_f014 <int>, newrel_f1524 <int>, newrel_f2534 <int>,
#   newrel_f3544 <int>, newrel_f4554 <int>, newrel_f5564 <int>,
#   newrel_f65 <int>
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

```
# Column names description
1. The first three letters of each column denote whether the column contains new or old cases of TB. In this dataset, each column contains new cases.
2. The next two letters describe the type of TB:
  - rel stands for cases of relapse
  - ep stands for cases of extrapulmonary TB
  - sn stands for cases of pulmonary TB that could not be diagnosed by a pulmonary smear (smear negative)
  - sp stands for cases of pulmonary TB that could be diagnosed be a pulmonary smear (smear positive)
3. The sixth letter gives the sex of TB patients. The dataset groups cases by males (m) and females (f).
4. The remaining numbers gives the age group. The dataset groups cases into seven age groups:
  - 014 = 0 – 14 years old
  - 1524 = 15 – 24 years old
  - 2534 = 25 – 34 years old
  - 3544 = 35 – 44 years old
  - 4554 = 45 – 54 years old
  - 5564 = 55 – 64 years old
  - 65 = 65 or older
```


```r
colnames(who)
```

```
 [1] "country"      "iso2"         "iso3"         "year"        
 [5] "new_sp_m014"  "new_sp_m1524" "new_sp_m2534" "new_sp_m3544"
 [9] "new_sp_m4554" "new_sp_m5564" "new_sp_m65"   "new_sp_f014" 
[13] "new_sp_f1524" "new_sp_f2534" "new_sp_f3544" "new_sp_f4554"
[17] "new_sp_f5564" "new_sp_f65"   "new_sn_m014"  "new_sn_m1524"
[21] "new_sn_m2534" "new_sn_m3544" "new_sn_m4554" "new_sn_m5564"
[25] "new_sn_m65"   "new_sn_f014"  "new_sn_f1524" "new_sn_f2534"
[29] "new_sn_f3544" "new_sn_f4554" "new_sn_f5564" "new_sn_f65"  
[33] "new_ep_m014"  "new_ep_m1524" "new_ep_m2534" "new_ep_m3544"
[37] "new_ep_m4554" "new_ep_m5564" "new_ep_m65"   "new_ep_f014" 
[41] "new_ep_f1524" "new_ep_f2534" "new_ep_f3544" "new_ep_f4554"
[45] "new_ep_f5564" "new_ep_f65"   "newrel_m014"  "newrel_m1524"
[49] "newrel_m2534" "newrel_m3544" "newrel_m4554" "newrel_m5564"
[53] "newrel_m65"   "newrel_f014"  "newrel_f1524" "newrel_f2534"
[57] "newrel_f3544" "newrel_f4554" "newrel_f5564" "newrel_f65"  
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Step 1: **Gather** non-variable columns


```r
who
```

```
# A tibble: 7,240 × 60
       country  iso2  iso3  year new_sp_m014 new_sp_m1524 new_sp_m2534
         <chr> <chr> <chr> <int>       <int>        <int>        <int>
1  Afghanistan    AF   AFG  1980          NA           NA           NA
2  Afghanistan    AF   AFG  1981          NA           NA           NA
3  Afghanistan    AF   AFG  1982          NA           NA           NA
4  Afghanistan    AF   AFG  1983          NA           NA           NA
5  Afghanistan    AF   AFG  1984          NA           NA           NA
6  Afghanistan    AF   AFG  1985          NA           NA           NA
7  Afghanistan    AF   AFG  1986          NA           NA           NA
8  Afghanistan    AF   AFG  1987          NA           NA           NA
9  Afghanistan    AF   AFG  1988          NA           NA           NA
10 Afghanistan    AF   AFG  1989          NA           NA           NA
# ... with 7,230 more rows, and 53 more variables: new_sp_m3544 <int>,
#   new_sp_m4554 <int>, new_sp_m5564 <int>, new_sp_m65 <int>,
#   new_sp_f014 <int>, new_sp_f1524 <int>, new_sp_f2534 <int>,
#   new_sp_f3544 <int>, new_sp_f4554 <int>, new_sp_f5564 <int>,
#   new_sp_f65 <int>, new_sn_m014 <int>, new_sn_m1524 <int>,
#   new_sn_m2534 <int>, new_sn_m3544 <int>, new_sn_m4554 <int>,
#   new_sn_m5564 <int>, new_sn_m65 <int>, new_sn_f014 <int>,
#   new_sn_f1524 <int>, new_sn_f2534 <int>, new_sn_f3544 <int>,
#   new_sn_f4554 <int>, new_sn_f5564 <int>, new_sn_f65 <int>,
#   new_ep_m014 <int>, new_ep_m1524 <int>, new_ep_m2534 <int>,
#   new_ep_m3544 <int>, new_ep_m4554 <int>, new_ep_m5564 <int>,
#   new_ep_m65 <int>, new_ep_f014 <int>, new_ep_f1524 <int>,
#   new_ep_f2534 <int>, new_ep_f3544 <int>, new_ep_f4554 <int>,
#   new_ep_f5564 <int>, new_ep_f65 <int>, newrel_m014 <int>,
#   newrel_m1524 <int>, newrel_m2534 <int>, newrel_m3544 <int>,
#   newrel_m4554 <int>, newrel_m5564 <int>, newrel_m65 <int>,
#   newrel_f014 <int>, newrel_f1524 <int>, newrel_f2534 <int>,
#   newrel_f3544 <int>, newrel_f4554 <int>, newrel_f5564 <int>,
#   newrel_f65 <int>
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Step 1: **Gather** non-variable columns


```r
who1 <- who %>%
  gather(new_sp_m014:newrel_f65, key = "key", value = "cases", na.rm = T)
who1
```

```
# A tibble: 76,046 × 6
       country  iso2  iso3  year         key cases
*        <chr> <chr> <chr> <int>       <chr> <int>
1  Afghanistan    AF   AFG  1997 new_sp_m014     0
2  Afghanistan    AF   AFG  1998 new_sp_m014    30
3  Afghanistan    AF   AFG  1999 new_sp_m014     8
4  Afghanistan    AF   AFG  2000 new_sp_m014    52
5  Afghanistan    AF   AFG  2001 new_sp_m014   129
6  Afghanistan    AF   AFG  2002 new_sp_m014    90
7  Afghanistan    AF   AFG  2003 new_sp_m014   127
8  Afghanistan    AF   AFG  2004 new_sp_m014   139
9  Afghanistan    AF   AFG  2005 new_sp_m014   151
10 Afghanistan    AF   AFG  2006 new_sp_m014   193
# ... with 76,036 more rows
```

```r
unique(who1$key)
```

```
 [1] "new_sp_m014"  "new_sp_m1524" "new_sp_m2534" "new_sp_m3544"
 [5] "new_sp_m4554" "new_sp_m5564" "new_sp_m65"   "new_sp_f014" 
 [9] "new_sp_f1524" "new_sp_f2534" "new_sp_f3544" "new_sp_f4554"
[13] "new_sp_f5564" "new_sp_f65"   "new_sn_m014"  "new_sn_m1524"
[17] "new_sn_m2534" "new_sn_m3544" "new_sn_m4554" "new_sn_m5564"
[21] "new_sn_m65"   "new_sn_f014"  "new_sn_f1524" "new_sn_f2534"
[25] "new_sn_f3544" "new_sn_f4554" "new_sn_f5564" "new_sn_f65"  
[29] "new_ep_m014"  "new_ep_m1524" "new_ep_m2534" "new_ep_m3544"
[33] "new_ep_m4554" "new_ep_m5564" "new_ep_m65"   "new_ep_f014" 
[37] "new_ep_f1524" "new_ep_f2534" "new_ep_f3544" "new_ep_f4554"
[41] "new_ep_f5564" "new_ep_f65"   "newrel_m014"  "newrel_m1524"
[45] "newrel_m2534" "newrel_m3544" "newrel_m4554" "newrel_m5564"
[49] "newrel_m65"   "newrel_f014"  "newrel_f1524" "newrel_f2534"
[53] "newrel_f3544" "newrel_f4554" "newrel_f5564" "newrel_f65"  
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Step 2: Decode the key...


```r
# Quick fix: "new_rel" instead of "newrel" - see `stringr` package for documentation on str_replace
#   And we'll discuss mutate from dplyr in the next section
who2 <- who1 %>% mutate(key = stringr::str_replace(key, "newrel", "new_rel"))
who3 <- who2 %>% separate(key, c("new", "type", "sexage"))
who3
```

```
# A tibble: 76,046 × 8
       country  iso2  iso3  year   new  type sexage cases
*        <chr> <chr> <chr> <int> <chr> <chr>  <chr> <int>
1  Afghanistan    AF   AFG  1997   new    sp   m014     0
2  Afghanistan    AF   AFG  1998   new    sp   m014    30
3  Afghanistan    AF   AFG  1999   new    sp   m014     8
4  Afghanistan    AF   AFG  2000   new    sp   m014    52
5  Afghanistan    AF   AFG  2001   new    sp   m014   129
6  Afghanistan    AF   AFG  2002   new    sp   m014    90
7  Afghanistan    AF   AFG  2003   new    sp   m014   127
8  Afghanistan    AF   AFG  2004   new    sp   m014   139
9  Afghanistan    AF   AFG  2005   new    sp   m014   151
10 Afghanistan    AF   AFG  2006   new    sp   m014   193
# ... with 76,036 more rows
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Step 2.5: Remove boring columns
  * `new` is uninformative (they're all "new"")
  * `iso2` and `iso3` are redundant


```r
who4 <- who3 %>% select(-new, -iso2, -iso3) # `select` from dplyr, which we'll discuss later
who4
```

```
# A tibble: 76,046 × 5
       country  year  type sexage cases
*        <chr> <int> <chr>  <chr> <int>
1  Afghanistan  1997    sp   m014     0
2  Afghanistan  1998    sp   m014    30
3  Afghanistan  1999    sp   m014     8
4  Afghanistan  2000    sp   m014    52
5  Afghanistan  2001    sp   m014   129
6  Afghanistan  2002    sp   m014    90
7  Afghanistan  2003    sp   m014   127
8  Afghanistan  2004    sp   m014   139
9  Afghanistan  2005    sp   m014   151
10 Afghanistan  2006    sp   m014   193
# ... with 76,036 more rows
```

CASE STUDY: WHO Global TB Report
========================================================
class: small-code

* Step 3: Separate `sex` and `age` from `sexage`


```r
unique(who4$sexage)
```

```
 [1] "m014"  "m1524" "m2534" "m3544" "m4554" "m5564" "m65"   "f014" 
 [9] "f1524" "f2534" "f3544" "f4554" "f5564" "f65"  
```

```r
who5 <- who4 %>% separate(sexage, c("sex", "age"), sep = 1) # You can specify a position for separate instead of a regex
```

CASE STUDY: WHO Global TB Report
========================================================
class:small-code

### Tidy!


```r
who5
```

```
# A tibble: 76,046 × 6
       country  year  type   sex   age cases
*        <chr> <int> <chr> <chr> <chr> <int>
1  Afghanistan  1997    sp     m   014     0
2  Afghanistan  1998    sp     m   014    30
3  Afghanistan  1999    sp     m   014     8
4  Afghanistan  2000    sp     m   014    52
5  Afghanistan  2001    sp     m   014   129
6  Afghanistan  2002    sp     m   014    90
7  Afghanistan  2003    sp     m   014   127
8  Afghanistan  2004    sp     m   014   139
9  Afghanistan  2005    sp     m   014   151
10 Afghanistan  2006    sp     m   014   193
# ... with 76,036 more rows
```

<iframe src="//giphy.com/embed/3o7TKqTOqevMiJAwU0" width="240" height="240" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

CASE STUDY: WHO Global TB Report
========================================================
class:small-code

Now we can do all the great things we wanted to with tidy data


```r
# Summarize it - Next section
# Which country has the most cases in each decade?
who5 %>% mutate(decade = cut(year, breaks = c(1980, 1990, 2000, 2010, Inf),
                             labels = c("80s", "90s", "00s", "10s"),
                             right = F)) %>%
  filter(sex == "f", age == "014") %>%
  group_by(decade, country) %>%
  summarize(cases = sum(cases)) %>% filter(cases == max(cases))

# Plot it - Tomorrow
who5 %>%
  ggplot(aes(year, cases, color = sex, alpha = age)) +
  geom_line() +
  facet_wrap(~country, scales = "free")

# Model it - Tomorrow
tb_model <- lm(cases ~ country + year + type + sex + age, data = who5)
```

CASE STUDY: WHO Global TB Report
========================================================
class:small-code

Bonus - tidy it all in one piped operation


```r
tidy_who <- who %>%
  gather(code, value, new_sp_m014:newrel_f65, na.rm = TRUE) %>% 
  mutate(code = stringr::str_replace(code, "newrel", "new_rel")) %>%
  separate(code, c("new", "var", "sexage")) %>% 
  select(-new, -iso2, -iso3) %>% 
  separate(sexage, c("sex", "age"), sep = 1)
tidy_who
```

```
# A tibble: 76,046 × 6
       country  year   var   sex   age value
*        <chr> <int> <chr> <chr> <chr> <int>
1  Afghanistan  1997    sp     m   014     0
2  Afghanistan  1998    sp     m   014    30
3  Afghanistan  1999    sp     m   014     8
4  Afghanistan  2000    sp     m   014    52
5  Afghanistan  2001    sp     m   014   129
6  Afghanistan  2002    sp     m   014    90
7  Afghanistan  2003    sp     m   014   127
8  Afghanistan  2004    sp     m   014   139
9  Afghanistan  2005    sp     m   014   151
10 Afghanistan  2006    sp     m   014   193
# ... with 76,036 more rows
```

Outline
========================================================
* Day 1: Tidy data
  + Importing your data into R
  + Tidying your data
  + **Transforming your tidy data**

dplyr - data transformation toolbox
========================================================

<img style="float: right;" src="http://dplyr.tidyverse.org/logo.png">

## A grammar of data manipulation

* Consistent set of verbs for common operations:
  + `filter()` picks cases based on their values.
  + `arrange()` changes the ordering of the rows.
  + `select()` picks variables based on their names.
  + `mutate()` adds new variables that are functions of existing variables
  + `summarize()` reduces multiple values down to a single summary.
* All of these can be combined with `group_by()` to perform any operation "by group"

New York City flights - 2013
========================================================
class:small-code

* From the `nycflights13` package


```r
flights
```

```
# A tibble: 336,776 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     1      517            515         2      830
2   2013     1     1      533            529         4      850
3   2013     1     1      542            540         2      923
4   2013     1     1      544            545        -1     1004
5   2013     1     1      554            600        -6      812
6   2013     1     1      554            558        -4      740
7   2013     1     1      555            600        -5      913
8   2013     1     1      557            600        -3      709
9   2013     1     1      557            600        -3      838
10  2013     1     1      558            600        -2      753
# ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

filter - pick observations based on values
========================================================
class:small-code

* Columns are referred to in expressions that filter rows out of the data frame
  * Separate arguments to `filter` are combined like `&`


```r
flights %>% filter(month == 5, day == 11)
```

```
# A tibble: 738 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     5    11        2           2159       123      151
2   2013     5    11        9           2359        10      342
3   2013     5    11       20           2359        21      353
4   2013     5    11       23           2245        98      126
5   2013     5    11       26           1940       286      203
6   2013     5    11       30           2359        31      359
7   2013     5    11       33           1930       303      227
8   2013     5    11      455            500        -5      630
9   2013     5    11      514            515        -1      810
10  2013     5    11      536            540        -4      855
# ... with 728 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

filter - pick observations based on values
========================================================
class:small-code

* Anything that evaluates to a boolean value can be used in a `filter` statement


```r
flights %>%
  filter(sched_dep_time < 1200,
         origin %in% c("JFK", "EWR"),
         !is.na(dep_time))
```

```
# A tibble: 85,532 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     1      517            515         2      830
2   2013     1     1      542            540         2      923
3   2013     1     1      544            545        -1     1004
4   2013     1     1      554            558        -4      740
5   2013     1     1      555            600        -5      913
6   2013     1     1      557            600        -3      838
7   2013     1     1      558            600        -2      849
8   2013     1     1      558            600        -2      853
9   2013     1     1      558            600        -2      924
10  2013     1     1      558            600        -2      923
# ... with 85,522 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

filter - pick observations based on values
========================================================
class:small-code

* Boolean combinations can be used too


```r
flights %>%
  filter((air_time < 120 & dep_delay > 30) |
           (air_time >= 120 & dep_delay > 60))
```

```
# A tibble: 36,583 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     1      811            630       101     1047
2   2013     1     1      826            715        71     1136
3   2013     1     1      848           1835       853     1001
4   2013     1     1      957            733       144     1056
5   2013     1     1     1114            900       134     1447
6   2013     1     1     1120            944        96     1331
7   2013     1     1     1301           1150        71     1518
8   2013     1     1     1304           1227        37     1518
9   2013     1     1     1337           1220        77     1649
10  2013     1     1     1355           1315        40     1538
# ... with 36,573 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

filter - pick observations based on values
========================================================
class:small-code

* Watchout: only returns rows that evaluate to `TRUE`, **not** `NA`
  * Usually what you want anyway, but it's worth checking in on your data frame from time to time


```r
flights %>% filter(dep_time < 1200) %>% nrow
```

```
[1] 131023
```

```r
flights %>% filter(dep_time < 1200 | is.na(dep_time)) %>% nrow
```

```
[1] 139278
```

arrange - sort rows by values
========================================================
class:small-code

* Sort by values in specified columns
  * Leftmost column is the primary sorting key, subsequent columns break ties


```r
flights %>% arrange(year, month, day)
```

```
# A tibble: 336,776 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     1      517            515         2      830
2   2013     1     1      533            529         4      850
3   2013     1     1      542            540         2      923
4   2013     1     1      544            545        -1     1004
5   2013     1     1      554            600        -6      812
6   2013     1     1      554            558        -4      740
7   2013     1     1      555            600        -5      913
8   2013     1     1      557            600        -3      709
9   2013     1     1      557            600        -3      838
10  2013     1     1      558            600        -2      753
# ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

arrange - sort rows by values
========================================================
class:small-code

* Reverse order of a column with `desc`


```r
flights %>% arrange(desc(dep_delay))
```

```
# A tibble: 336,776 × 19
    year month   day dep_time sched_dep_time dep_delay arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>
1   2013     1     9      641            900      1301     1242
2   2013     6    15     1432           1935      1137     1607
3   2013     1    10     1121           1635      1126     1239
4   2013     9    20     1139           1845      1014     1457
5   2013     7    22      845           1600      1005     1044
6   2013     4    10     1100           1900       960     1342
7   2013     3    17     2321            810       911      135
8   2013     6    27      959           1900       899     1236
9   2013     7    22     2257            759       898      121
10  2013    12     5      756           1700       896     1058
# ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
#   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

arrange - sort rows by values
========================================================
class:small-code

* `NA` values always go to end


```r
flights %>% arrange(dep_delay) %>% tail
```

```
# A tibble: 6 × 19
   year month   day dep_time sched_dep_time dep_delay arr_time
  <int> <int> <int>    <int>          <int>     <dbl>    <int>
1  2013     9    30       NA           1842        NA       NA
2  2013     9    30       NA           1455        NA       NA
3  2013     9    30       NA           2200        NA       NA
4  2013     9    30       NA           1210        NA       NA
5  2013     9    30       NA           1159        NA       NA
6  2013     9    30       NA            840        NA       NA
# ... with 12 more variables: sched_arr_time <int>, arr_delay <dbl>,
#   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>,
#   time_hour <dttm>
```

select - remove, reorder, or rename columns
========================================================
class:small-code

* Specify columns by name


```r
flights %>% select(year, month, day, dep_delay)
```

```
# A tibble: 336,776 × 4
    year month   day dep_delay
   <int> <int> <int>     <dbl>
1   2013     1     1         2
2   2013     1     1         4
3   2013     1     1         2
4   2013     1     1        -1
5   2013     1     1        -6
6   2013     1     1        -4
7   2013     1     1        -5
8   2013     1     1        -3
9   2013     1     1        -3
10  2013     1     1        -2
# ... with 336,766 more rows
```

select - remove, reorder, or rename columns
========================================================
class:small-code

* Remove columns with "`-`"


```r
flights %>% select(-sched_dep_time, -sched_arr_time)
```

```
# A tibble: 336,776 × 17
    year month   day dep_time dep_delay arr_time arr_delay carrier flight
   <int> <int> <int>    <int>     <dbl>    <int>     <dbl>   <chr>  <int>
1   2013     1     1      517         2      830        11      UA   1545
2   2013     1     1      533         4      850        20      UA   1714
3   2013     1     1      542         2      923        33      AA   1141
4   2013     1     1      544        -1     1004       -18      B6    725
5   2013     1     1      554        -6      812       -25      DL    461
6   2013     1     1      554        -4      740        12      UA   1696
7   2013     1     1      555        -5      913        19      B6    507
8   2013     1     1      557        -3      709       -14      EV   5708
9   2013     1     1      557        -3      838        -8      B6     79
10  2013     1     1      558        -2      753         8      AA    301
# ... with 336,766 more rows, and 8 more variables: tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

select - remove, reorder, or rename columns
========================================================
class:small-code

* Range of columns with "`:`"


```r
flights %>% select(carrier:tailnum)
```

```
# A tibble: 336,776 × 3
   carrier flight tailnum
     <chr>  <int>   <chr>
1       UA   1545  N14228
2       UA   1714  N24211
3       AA   1141  N619AA
4       B6    725  N804JB
5       DL    461  N668DN
6       UA   1696  N39463
7       B6    507  N516JB
8       EV   5708  N829AS
9       B6     79  N593JB
10      AA    301  N3ALAA
# ... with 336,766 more rows
```

select - remove, reorder, or rename columns
========================================================
class:small-code

* Helper functions for `select`
  + `starts_with("abc")`: matches names that begin with “abc”.
  + `ends_with("xyz")`: matches names that end with “xyz”.
  + `contains("ijk")`: matches names that contain “ijk”.
  + `matches("(.)\\1")`: selects variables that match a regex.
  + `num_range("x", 1:3)`: matches x1, x2 and x3.


```r
flights %>% select(-starts_with("sched"))
```

```
# A tibble: 336,776 × 17
    year month   day dep_time dep_delay arr_time arr_delay carrier flight
   <int> <int> <int>    <int>     <dbl>    <int>     <dbl>   <chr>  <int>
1   2013     1     1      517         2      830        11      UA   1545
2   2013     1     1      533         4      850        20      UA   1714
3   2013     1     1      542         2      923        33      AA   1141
4   2013     1     1      544        -1     1004       -18      B6    725
5   2013     1     1      554        -6      812       -25      DL    461
6   2013     1     1      554        -4      740        12      UA   1696
7   2013     1     1      555        -5      913        19      B6    507
8   2013     1     1      557        -3      709       -14      EV   5708
9   2013     1     1      557        -3      838        -8      B6     79
10  2013     1     1      558        -2      753         8      AA    301
# ... with 336,766 more rows, and 8 more variables: tailnum <chr>,
#   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

select - remove, reorder, or rename columns
========================================================
class:small-code


```r
flights %>% select(year, month, day, tail_number = tailnum)
```

```
# A tibble: 336,776 × 4
    year month   day tail_number
   <int> <int> <int>       <chr>
1   2013     1     1      N14228
2   2013     1     1      N24211
3   2013     1     1      N619AA
4   2013     1     1      N804JB
5   2013     1     1      N668DN
6   2013     1     1      N39463
7   2013     1     1      N516JB
8   2013     1     1      N829AS
9   2013     1     1      N593JB
10  2013     1     1      N3ALAA
# ... with 336,766 more rows
```

select - remove, reorder, or rename columns
========================================================
class:small-code

* Other tricks:
  * `rename()`: like `select()` but keeps all variables not mentioned
  * `everything()`: shortcut for "all other columns"


```r
flights %>% rename(tail_number = tailnum)
flights %>% select(carrier, dep_delay, arr_delay, everything())
```

mutate - calculate new columns from existing ones
========================================================
class:small-code

* Use any arithmetic operations: `+`, `-`, `/`, `*`


```r
flights %>% select(distance, air_time) %>%
  mutate(speed = distance / air_time * 60)
```

```
# A tibble: 336,776 × 3
   distance air_time    speed
      <dbl>    <dbl>    <dbl>
1      1400      227 370.0441
2      1416      227 374.2731
3      1089      160 408.3750
4      1576      183 516.7213
5       762      116 394.1379
6       719      150 287.6000
7      1065      158 404.4304
8       229       53 259.2453
9       944      140 404.5714
10      733      138 318.6957
# ... with 336,766 more rows
```

mutate - calculate new columns from existing ones
========================================================
class:small-code

* Other useful operations:
  + Modular arithmetic: `%/%`, `%%`
  + Logarithmic functions: `log`, `log10`, `log2`, `log1p`


```r
flights %>% select(sched_dep_time, distance) %>%
  transmute(sched_dep_hour = sched_dep_time %/% 100, # transmute drops other columns
            sched_dep_minute = sched_dep_time %% 100,
            log_distance = log10(distance))
```

```
# A tibble: 336,776 × 3
   sched_dep_hour sched_dep_minute log_distance
            <dbl>            <dbl>        <dbl>
1               5               15     3.146128
2               5               29     3.151063
3               5               40     3.037028
4               5               45     3.197556
5               6                0     2.881955
6               5               58     2.856729
7               6                0     3.027350
8               6                0     2.359835
9               6                0     2.974972
10              6                0     2.865104
# ... with 336,766 more rows
```

mutate - calculate new columns from existing ones
========================================================
class:small-code

* Other useful operations:
  + Offset functions: `lead`, `lag`


```r
flights %>% filter(origin == "JFK", !is.na(dep_time)) %>%
  select(year, month, day, dep_time) %>% 
  arrange(year, month, day, dep_time) %>%
  mutate(min_from_last_flight = dep_time - lag(dep_time))
```

```
# A tibble: 109,416 × 5
    year month   day dep_time min_from_last_flight
   <int> <int> <int>    <int>                <int>
1   2013     1     1      542                   NA
2   2013     1     1      544                    2
3   2013     1     1      557                   13
4   2013     1     1      558                    1
5   2013     1     1      558                    0
6   2013     1     1      558                    0
7   2013     1     1      559                    1
8   2013     1     1      606                   47
9   2013     1     1      611                    5
10  2013     1     1      613                    2
# ... with 109,406 more rows
```

mutate - calculate new columns from existing ones
========================================================
class:small-code

* Other useful operations:
  + Cumulative functions: `cumsum`, `cummean`, `cumprod`, etc.


```r
flights %>% filter(carrier == "UA", !is.na(dep_delay)) %>%
  select(year, month, day, sched_dep_time, dep_delay) %>% 
  arrange(year, month, day, sched_dep_time) %>%
  mutate(dep_delay_to_date = cumsum(dep_delay)) %>% tail
```

```
# A tibble: 6 × 6
   year month   day sched_dep_time dep_delay dep_delay_to_date
  <int> <int> <int>          <int>     <dbl>             <dbl>
1  2013    12    31           2017         4            701860
2  2013    12    31           2030       -10            701850
3  2013    12    31           2030        -2            701848
4  2013    12    31           2046        31            701879
5  2013    12    31           2050        25            701904
6  2013    12    31           2109        -6            701898
```

summarize - collapse data frame to a single row
========================================================
class:small-code


```r
flights %>%
  summarize(mean_delay = mean(dep_delay, na.rm = T),
            num_flights = n()) # n() is a special function, basically "length"
```

```
# A tibble: 1 × 2
  mean_delay num_flights
       <dbl>       <int>
1   12.63907      336776
```

* Pretty boring, right?

group_by - subdivide data frame
========================================================
class:small-code

* Makes `summarize` *much* more useful


```r
flights %>%
  group_by(carrier) %>% 
  summarize(mean_delay = mean(dep_delay, na.rm = T)) %>%
  arrange(desc(mean_delay))
```

```
# A tibble: 16 × 2
   carrier mean_delay
     <chr>      <dbl>
1       F9  20.215543
2       EV  19.955390
3       YV  18.996330
4       FL  18.726075
5       WN  17.711744
6       9E  16.725769
7       B6  13.022522
8       VX  12.869421
9       OO  12.586207
10      UA  12.106073
11      MQ  10.552041
12      DL   9.264505
13      AA   8.586016
14      AS   5.804775
15      HA   4.900585
16      US   3.782418
```

group_by - subdivide data frame
========================================================
class:small-code

* Can group by multiple columns
  * Each `summarize` "peels off" rightmost column


```r
flights %>%
  group_by(year, month, day) %>%
  tally # means "summarize(n = n())"
```

```
Source: local data frame [365 x 4]
Groups: year, month [?]

    year month   day     n
   <int> <int> <int> <int>
1   2013     1     1   842
2   2013     1     2   943
3   2013     1     3   914
4   2013     1     4   915
5   2013     1     5   720
6   2013     1     6   832
7   2013     1     7   933
8   2013     1     8   899
9   2013     1     9   902
10  2013     1    10   932
# ... with 355 more rows
```

group_by - subdivide data frame
========================================================
class:small-code

* Can group by multiple columns
  * Each `summarize` "peels off" rightmost column


```r
flights %>%
  group_by(year, month, day) %>%
  tally %>% tally # will sum "n" column
```

```
Source: local data frame [12 x 3]
Groups: year [?]

    year month    nn
   <int> <int> <int>
1   2013     1 27004
2   2013     2 24951
3   2013     3 28834
4   2013     4 28330
5   2013     5 28796
6   2013     6 28243
7   2013     7 29425
8   2013     8 29327
9   2013     9 27574
10  2013    10 28889
11  2013    11 27268
12  2013    12 28135
```

dplyr - data transformation toolbox
========================================================

* The `dplyr` verbs
  + `filter()` picks cases based on their values.
  + `arrange()` changes the ordering of the rows.
  + `select()` picks variables based on their names.
  + `mutate()` adds new variables that are functions of existing variables
  + `summarize()` reduces multiple values down to a single summary.
  + `group_by()` to perform any operation "by group"
* **This vocabulary allows you to do a lot!**

Not covered - joins
========================================================

* `dplyr` has nice syntax for SQL-style joins
  + `inner_join(df1, df2, by = c("keyA", "keyB1" = "keyB2"))`
  + `left_join()`
  + `right_join()`
  + `full_join()`
  + `anti_join()`

Data transforming exercise 1
========================================================
class: small-code

- Goal: What country gave the highest mean points to Belarus in Eurovision?


```r
eurovision
```

```
# A tibble: 38,181 × 7
    year competition_round edition jury_or_televoting from_country
   <dbl>             <chr>   <chr>              <chr>        <chr>
1   1975                 f   1975f                  J      Belgium
2   1975                 f   1975f                  J      Belgium
3   1975                 f   1975f                  J      Belgium
4   1975                 f   1975f                  J      Belgium
5   1975                 f   1975f                  J      Belgium
6   1975                 f   1975f                  J      Belgium
7   1975                 f   1975f                  J      Belgium
8   1975                 f   1975f                  J      Belgium
9   1975                 f   1975f                  J      Belgium
10  1975                 f   1975f                  J      Belgium
# ... with 38,171 more rows, and 2 more variables: to_country <chr>,
#   points <dbl>
```

<iframe src="//giphy.com/embed/xA1PKCt00IDew" width="240" height="175" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

Data transforming exercise 1
========================================================
class: small-code

- Goal: What country gave the highest mean points to Belarus in Eurovision?


```r
mean_points_to_belarus <- eurovision %>%
  filter(to_country == "Belarus") %>%
  group_by(from_country) %>%
  summarize(num_obs = n(), mean_points = mean(points))

mean_points_to_belarus %>% arrange(desc(mean_points))
```

```
# A tibble: 49 × 3
   from_country num_obs mean_points
          <chr>   <int>       <dbl>
1       Ukraine      14    7.928571
2        Russia      10    6.800000
3       Moldova      10    6.500000
4       Georgia      10    5.900000
5       Armenia       7    5.571429
6    Azerbaijan       3    4.666667
7     Lithuania      14    4.642857
8        Greece      10    3.500000
9        Latvia      13    3.461538
10       Cyprus      10    3.400000
# ... with 39 more rows
```

* How does being a former SSR affect Eurovision voting?
  * **Tomorrow!**

Additional Eurovision data exercises
========================================================
class: small-code

* What country has the highest mean score in the finals?
* What country has given the lowest/highest mean vote to any other country in the finals (num years > 20)?
* What country had the steepest decade-on-decade decline in mean score in the finals?
  + Hint: `cut()` splits a continuous variable into interval factors


```r
# Data to start from - only finals, no self rows
eurovision_finals_no_self <- eurovision %>%
  filter(competition_round == "f", from_country != to_country)
```

Additional Eurovision data exercises
========================================================
class: small-code

* What country has the highest mean score?


```r
eurovision_finals_no_self %>%
  group_by(to_country) %>%
  summarize(mean_points = mean(points), n = n_distinct(year)) %>%
  arrange(desc(mean_points))
```

```
# A tibble: 51 × 3
            to_country mean_points     n
                 <chr>       <dbl> <int>
1            Australia    5.842975     2
2  Serbia & Montenegro    5.479452     2
3               Russia    4.145863    20
4              Ukraine    4.139925    13
5             Bulgaria    3.772358     2
6               Monaco    3.707865     5
7               Sweden    3.691674    40
8              Ireland    3.554348    34
9                Italy    3.513029    23
10          Azerbaijan    3.228070     9
# ... with 41 more rows
```


<iframe src="https://giphy.com/embed/JW5cCdKfoLFG8" width="240" height="180" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

Additional Eurovision data exercises
========================================================
class: small-code

* What country has given the lowest mean vote to any other country (num years > 20)


```r
eurovision_finals_no_self %>%
  group_by(from_country, to_country) %>%
  summarize(mean_points = mean(points), n = n_distinct(year)) %>%
  filter(n > 20) %>% 
  arrange(mean_points)
```

```
Source: local data frame [380 x 4]
Groups: from_country [24]

   from_country to_country mean_points     n
          <chr>      <chr>       <dbl> <int>
1        Turkey     Cyprus  0.04166667    24
2        Cyprus     Turkey  0.42857143    28
3       Ireland     Turkey  0.51612903    31
4      Portugal     Turkey  0.51612903    31
5      Portugal    Finland  0.53571429    28
6       Denmark   Portugal  0.56521739    23
7       Estonia      Spain  0.56521739    22
8       Austria     Cyprus  0.57692308    25
9       Belgium    Finland  0.63333333    30
10      Ireland   Portugal  0.65517241    29
# ... with 370 more rows
```

Additional Eurovision data exercises
========================================================
class: small-code

* What country had the steepest decade-on-decade decline in mean score in the finals?


```r
eurovision_finals_no_self %>%
  group_by(to_country, year) %>%
  summarize(total_points = sum(points)) %>%
  mutate(decade = cut(year, breaks = c(1970, 1980, 1990, 2000, 2010, Inf),
                             labels = c("70s", "80s", "90s", "00s", "10s"),
                             right = F)) %>%
  group_by(to_country, decade) %>%
  summarize(mean_score = mean(total_points)) %>%
  mutate(delta_mean_score = mean_score - lag(mean_score)) %>%
  arrange(delta_mean_score)
```

```
Source: local data frame [165 x 4]
Groups: to_country [51]

       to_country decade mean_score delta_mean_score
            <chr> <fctr>      <dbl>            <dbl>
1         Belarus    10s   36.33333       -108.66667
2          Serbia    10s  107.80000       -106.20000
3         Ireland    00s   42.66667        -76.53333
4  United Kingdom    00s   44.50000        -63.90000
5          France    80s   58.22222        -61.57778
6          Greece    10s   89.00000        -53.88889
7         Hungary    10s   71.00000        -41.50000
8         Germany    90s   54.66667        -40.43333
9      Luxembourg    90s   22.00000        -39.20000
10        Estonia    10s   72.25000        -37.75000
# ... with 155 more rows
```
