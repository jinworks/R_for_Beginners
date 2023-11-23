# 数据规整2 {#tidyverse-tidyr2}

接着上一章，罗列一些`tidyr`的函数


```r
library(tidyverse)
```


## `fill()` 缺失值填充

利用**所在列**的上下值进行缺失值填充

```r
sales <- tibble::tribble(
  ~quarter, ~year, ~sales,
      "Q1",  2000,  66013,
      "Q2",    NA,  69182,
      "Q3",    NA,  53175,
      "Q4",    NA,  21001,
      "Q1",  2001,  46036,
      "Q2",    NA,  58842,
      "Q3",    NA,  44568,
      "Q4",    NA,  50197,
      "Q1",  2002,  39113,
      "Q2",    NA,  41668,
      "Q3",    NA,  30144,
      "Q4",    NA,  52897
  )
sales
```

```
## # A tibble: 12 × 3
##    quarter  year sales
##    <chr>   <dbl> <dbl>
##  1 Q1       2000 66013
##  2 Q2         NA 69182
##  3 Q3         NA 53175
##  4 Q4         NA 21001
##  5 Q1       2001 46036
##  6 Q2         NA 58842
##  7 Q3         NA 44568
##  8 Q4         NA 50197
##  9 Q1       2002 39113
## 10 Q2         NA 41668
## 11 Q3         NA 30144
## 12 Q4         NA 52897
```



```r
sales %>% fill(year)
```

```
## # A tibble: 12 × 3
##    quarter  year sales
##    <chr>   <dbl> <dbl>
##  1 Q1       2000 66013
##  2 Q2       2000 69182
##  3 Q3       2000 53175
##  4 Q4       2000 21001
##  5 Q1       2001 46036
##  6 Q2       2001 58842
##  7 Q3       2001 44568
##  8 Q4       2001 50197
##  9 Q1       2002 39113
## 10 Q2       2002 41668
## 11 Q3       2002 30144
## 12 Q4       2002 52897
```
也可以控制填充的方向

```r
sales %>% fill(year, .direction = "up")
```

```
## # A tibble: 12 × 3
##    quarter  year sales
##    <chr>   <dbl> <dbl>
##  1 Q1       2000 66013
##  2 Q2       2001 69182
##  3 Q3       2001 53175
##  4 Q4       2001 21001
##  5 Q1       2001 46036
##  6 Q2       2002 58842
##  7 Q3       2002 44568
##  8 Q4       2002 50197
##  9 Q1       2002 39113
## 10 Q2         NA 41668
## 11 Q3         NA 30144
## 12 Q4         NA 52897
```


## `expand()` 与 `complete()` 

指定数据框的若干列，根据其向量元素值，产生所有可能的交叉组合

```r
df <- tibble::tribble(
  ~x, ~y, ~z,
  1L, 1L, 4L,
  1L, 2L, 5L,
  2L, 1L, NA,
  3L, 2L, 6L
)


df %>% expand(x, y)
```

```
## # A tibble: 6 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     1     2
## 3     2     1
## 4     2     2
## 5     3     1
## 6     3     2
```

`nesting()`用于限定只产生数据框已出现的组合。

```r
df %>% expand(nesting(x, y))
```

```
## # A tibble: 4 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     1     2
## 3     2     1
## 4     3     2
```



```r
df %>% expand(nesting(x, y), z)
```

```
## # A tibble: 16 × 3
##        x     y     z
##    <int> <int> <int>
##  1     1     1     4
##  2     1     1     5
##  3     1     1     6
##  4     1     1    NA
##  5     1     2     4
##  6     1     2     5
##  7     1     2     6
##  8     1     2    NA
##  9     2     1     4
## 10     2     1     5
## 11     2     1     6
## 12     2     1    NA
## 13     3     2     4
## 14     3     2     5
## 15     3     2     6
## 16     3     2    NA
```

 


`complete()` 补全，可以看做是 `expand(nesting()) + fill()`


```r
df %>% complete(x, y)
```

```
## # A tibble: 6 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     1     4
## 2     1     2     5
## 3     2     1    NA
## 4     2     2    NA
## 5     3     1    NA
## 6     3     2     6
```



```r
df %>% complete(x, y, fill = list(z = 0))
```

```
## # A tibble: 6 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     1     4
## 2     1     2     5
## 3     2     1     0
## 4     2     2     0
## 5     3     1     0
## 6     3     2     6
```


数据在complete补全的时候，会面临有两种缺失值：

1. 补位的时候造成的空缺
2. 数据原先就存在缺失值


```r
df %>% complete(x, y)
```

```
## # A tibble: 6 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     1     4
## 2     1     2     5
## 3     2     1    NA
## 4     2     2    NA
## 5     3     1    NA
## 6     3     2     6
```

- 补位的时候造成的空缺，可通过`fill = list(x = 0)` 控制填充


```r
df %>% complete(x, y, fill = list(z = 0))
```

```
## # A tibble: 6 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     1     4
## 2     1     2     5
## 3     2     1     0
## 4     2     2     0
## 5     3     1     0
## 6     3     2     6
```


- 数据原先就存在缺失值，最好通过 `explicit = FALSE`显式地控制是否填充


```r
df %>% complete(x, y, fill = list(z = 0), explicit = FALSE)
```

```
## # A tibble: 6 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     1     4
## 2     1     2     5
## 3     2     1    NA
## 4     2     2     0
## 5     3     1     0
## 6     3     2     6
```





## `expand_grid()` 与 `crossing()`
产生一个新的数据框，每行对应着向量元素的所有交叉组合

```r
expand_grid(x = 1:3, y = 1:2)
```

```
## # A tibble: 6 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     1     2
## 3     2     1
## 4     2     2
## 5     3     1
## 6     3     2
```



```r
crossing(x = 1:3, y = 1:2)
```

```
## # A tibble: 6 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     1     2
## 3     2     1
## 4     2     2
## 5     3     1
## 6     3     2
```

向量换成数据框也可以，其结果就是数据框行与元素的交叉组合

```r
expand_grid(df = data.frame(x = 1:2, y = c(2, 1)), z = 1:3)
```

```
## # A tibble: 6 × 2
##    df$x    $y     z
##   <int> <dbl> <int>
## 1     1     2     1
## 2     1     2     2
## 3     1     2     3
## 4     2     1     1
## 5     2     1     2
## 6     2     1     3
```



```r
crossing(df = data.frame(x = 1:2, y = c(2, 1)), z = 1:3)
```

```
## # A tibble: 6 × 2
##    df$x    $y     z
##   <int> <dbl> <int>
## 1     1     2     1
## 2     1     2     2
## 3     1     2     3
## 4     2     1     1
## 5     2     1     2
## 6     2     1     3
```

`crossing()`可以看作是`expand_grid() + distinct()`， 即`crossing()`在完成交叉组合之后会自动去重，比如


```r
expand_grid(x = c(1, 1), y = c(1:2))  # 不考虑去重
```

```
## # A tibble: 4 × 2
##       x     y
##   <dbl> <int>
## 1     1     1
## 2     1     2
## 3     1     1
## 4     1     2
```



```r
crossing(x = c(1, 1), y = c(1:2))    # 考虑去重 
```

```
## # A tibble: 2 × 2
##       x     y
##   <dbl> <int>
## 1     1     1
## 2     1     2
```



## `separate()` 与 `unite()`


```r
tb <- tibble::tribble(
  ~day, ~price,
  1,   "30-45",
  2,   "40-95",
  3,   "89-65",
  4,   "45-63",
  5,   "52-42"
)
```


```r
tb1 <- tb %>%
  separate(price, into = c("low", "high"), sep = "-")
tb1
```

```
## # A tibble: 5 × 3
##     day low   high 
##   <dbl> <chr> <chr>
## 1     1 30    45   
## 2     2 40    95   
## 3     3 89    65   
## 4     4 45    63   
## 5     5 52    42
```



```r
tb1 %>%
  unite(col = "price", c(low, high), sep = ":", remove = FALSE)
```

```
## # A tibble: 5 × 4
##     day price low   high 
##   <dbl> <chr> <chr> <chr>
## 1     1 30:45 30    45   
## 2     2 40:95 40    95   
## 3     3 89:65 89    65   
## 4     4 45:63 45    63   
## 5     5 52:42 52    42
```



有时候分隔符搞不定的，可以用正则表达式，将捕获的每组弄成一列

```r
dfc <- tibble(x = c("1-12week", "1-10wk", "5-12w", "01-05weeks"))
dfc
```

```
## # A tibble: 4 × 1
##   x         
##   <chr>     
## 1 1-12week  
## 2 1-10wk    
## 3 5-12w     
## 4 01-05weeks
```



```r
dfc %>% tidyr::extract(
  x,
  c("start", "end", "letter"), "(\\d+)-(\\d+)([a-z]+)",
  remove = FALSE
)
```

```
## # A tibble: 4 × 4
##   x          start end   letter
##   <chr>      <chr> <chr> <chr> 
## 1 1-12week   1     12    week  
## 2 1-10wk     1     10    wk    
## 3 5-12w      5     12    w     
## 4 01-05weeks 01    05    weeks
```






## 删除缺失值所在行drop_na()与replace_na() 


```r
df <- tibble::tribble(
    ~name,     ~type, ~score, ~extra,
  "Alice", "english",     80,     10,
  "Alice",    "math",     NA,      5,
    "Bob", "english",     NA,      9,
    "Bob",    "math",     69,     NA,
  "Carol", "english",     80,     10,
  "Carol",    "math",     90,      5
  )

df
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Alice math       NA     5
## 3 Bob   english    NA     9
## 4 Bob   math       69    NA
## 5 Carol english    80    10
## 6 Carol math       90     5
```

如果score列中有缺失值`NA`，就删除所在的row

```r
df %>%
  filter(!is.na(score))
```

```
## # A tibble: 4 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Bob   math       69    NA
## 3 Carol english    80    10
## 4 Carol math       90     5
```

或者用`across()`

```r
df %>%
  filter(
    across(score, ~ !is.na(.x))
  )
```

```
## Warning: Using `across()` in `filter()` was deprecated in dplyr 1.0.8.
## ℹ Please use `if_any()` or `if_all()` instead.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## # A tibble: 4 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Bob   math       69    NA
## 3 Carol english    80    10
## 4 Carol math       90     5
```


所有列，如果有缺失值`NA`，就删除所在的row

```r
df %>%
  filter(
    across(everything(), ~ !is.na(.x))
  )
```

```
## Warning: Using `across()` in `filter()` was deprecated in dplyr 1.0.8.
## ℹ Please use `if_any()` or `if_all()` instead.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## # A tibble: 3 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Carol english    80    10
## 3 Carol math       90     5
```



现在有更简便的方法

```r
df %>%
  drop_na()
```

```
## # A tibble: 3 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Carol english    80    10
## 3 Carol math       90     5
```

也可指定某一列

```r
df %>%
  drop_na(score)
```

```
## # A tibble: 4 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Bob   math       69    NA
## 3 Carol english    80    10
## 4 Carol math       90     5
```


没来参加考试，视为0分，可以用`replace_na()`

```r
df %>% mutate(score = replace_na(score, 0))
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Alice math        0     5
## 3 Bob   english     0     9
## 4 Bob   math       69    NA
## 5 Carol english    80    10
## 6 Carol math       90     5
```

或者使用`coalesce()`


```r
df %>% mutate(score = coalesce(score, 0))
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Alice math        0     5
## 3 Bob   english     0     9
## 4 Bob   math       69    NA
## 5 Carol english    80    10
## 6 Carol math       90     5
```



```r
df %>%
  mutate(
    across(c(score, extra), ~ coalesce(.x, 0))
  )
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english    80    10
## 2 Alice math        0     5
## 3 Bob   english     0     9
## 4 Bob   math       69     0
## 5 Carol english    80    10
## 6 Carol math       90     5
```



没来参加考试，用平均分代替

```r
df %>%
  mutate(
    score = replace_na(score, mean(score, na.rm = TRUE))
  )
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english  80      10
## 2 Alice math     79.8     5
## 3 Bob   english  79.8     9
## 4 Bob   math     69      NA
## 5 Carol english  80      10
## 6 Carol math     90       5
```

当然也可以用`if_else()`来做


```r
df %>%
  mutate(
    score = if_else(is.na(score), mean(score, na.rm = TRUE), score)
  )
```

```
## # A tibble: 6 × 4
##   name  type    score extra
##   <chr> <chr>   <dbl> <dbl>
## 1 Alice english  80      10
## 2 Alice math     79.8     5
## 3 Bob   english  79.8     9
## 4 Bob   math     69      NA
## 5 Carol english  80      10
## 6 Carol math     90       5
```




