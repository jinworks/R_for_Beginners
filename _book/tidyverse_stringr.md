# 正则表达式 {#tidyverse-stringr}


```r
library(tidyverse)
library(stringr)
```



## 问题

这是一份关于地址信息的数据


```
## # A tibble: 8 × 2
##      No address                      
##   <int> <chr>                        
## 1     1 Sichuan Univ, Coll Chem      
## 2     2 Sichuan Univ, Coll Elect Engn
## 3     3 Sichuan Univ, Dept Phys      
## 4     4 Sichuan Univ, Coll Life Sci  
## 5     6 Sichuan Univ, Food Engn      
## 6     7 Sichuan Univ, Coll Phys      
## 7     8 Sichuan Univ, Sch Business   
## 8     9 Wuhan Univ, Mat Sci
```

**问题**：如何提取`Sichuan Univ`后面的学院？这需要用到正则表达式的知识。




## 什么是正则表达式

我们在word文档或者excel中，经常使用查找和替换, 然而有些情况，word是解决不了的，比如

- 条件搜索
  - 统计文中，前面有 “data”, “computer” or “statistical” 的 “analysis”，这个单词的个数
  - 找出文中重复的单词，比如“we love love you”
- 拼写检查
  - 电话号码（邮件，密码等）是否正确格式
  - 日期书写的规范与统一
- 提取信息
  - 提取文本特定位置的数据 
- 文本挖掘
  - 非结构化的提取成结构化


这个时候就需要用到正则表达式（Regular Expression），这一强大、便捷、高效的文本处理工具。那么，什么是正则表达式呢？简单点说，正则表达式是处理字符串的。复杂点说，正则表达式描述了一种字符串匹配的模式（pattern），通常被用来检索、替换那些符合某个模式(规则)的文本。这种固定的格式的文本，生活中常见的有电话号码、网络地址、邮件地址和日期格式等等。

正则表达式并不是R语言特有的，事实上，几乎所有程序语言都支持正则表达式 (e.g. Perl, Python, Java, Ruby, etc).

R 语言中很多函数都需要使用正则表达式，然而正则表达式不太好学。幸运的是，大神Hadley Wickham开发的stringr包让正则表达式简单易懂，因此今天我们就介绍这个包。本章的内容与《R for data science》第10章基本一致。本章目的教大家写**简单的**正则表示式就行了。




## 字符串基础


### 字符串长度

想获取字符串的长度，可以使用`str_length()`函数

```r
str_length("R for data science")
```

```
## [1] 18
```

字符串向量，也适用

```r
str_length(c("a", "R for data science", NA))
```

```
## [1]  1 18 NA
```

数据框里配合dplyr函数，同样很方便

```r
data.frame(
  x = c("a", "R for data science", NA)
) %>%
  mutate(y = str_length(x))
```

```
##                    x  y
## 1                  a  1
## 2 R for data science 18
## 3               <NA> NA
```







### 字符串组合


把字符串拼接在一起，使用 `str_c()` 函数

```r
str_c("x", "y")
```

```
## [1] "xy"
```


把字符串拼接在一起，可以设置中间的间隔

```r
str_c("x", "y", sep = ", ")
```

```
## [1] "x, y"
```



```r
str_c(c("x", "y", "z"), sep = ", ")
```

```
## [1] "x" "y" "z"
```
是不是和你想象的不一样，那就`?str_c`，或者试试这个


```r
str_c(c("x", "y", "z"), c("x", "y", "z"), sep = ", ")
```

```
## [1] "x, x" "y, y" "z, z"
```

用在数据框里

```r
data.frame(
  x = c("I", "love", "you"),
  y = c("you", "like", "me")
) %>%
  mutate(z = str_c(x, y, sep = "|"))
```

```
##      x    y         z
## 1    I  you     I|you
## 2 love like love|like
## 3  you   me    you|me
```

使用collapse选项，是先组合，然后再转换成单个字符串，大家对比下


```r
str_c(c("x", "y", "z"), c("a", "b", "c"), sep = "|")
```

```
## [1] "x|a" "y|b" "z|c"
```


```r
str_c(c("x", "y", "z"), c("a", "b", "c"), collapse = "|")
```

```
## [1] "xa|yb|zc"
```







### 字符串取子集

截取字符串的一部分，需要指定截取的开始位置和结束位置

```r
x <- c("Apple", "Banana", "Pear")
str_sub(x, 1, 3)
```

```
## [1] "App" "Ban" "Pea"
```

开始位置和结束位置如果是负整数，就表示位置是从后往前数，比如下面这段代码，截取倒数第3个至倒数第1个位置上的字符串

```r
x <- c("Apple", "Banana", "Pear")
str_sub(x, -3, -1)
```

```
## [1] "ple" "ana" "ear"
```


也可以进行赋值，如果该位置上有字符，就用新的字符替换旧的字符


```r
x <- c("Apple", "Banana", "Pear")
x
```

```
## [1] "Apple"  "Banana" "Pear"
```



```r
str_sub(x, 1, 1)
```

```
## [1] "A" "B" "P"
```



```r
str_sub(x, 1, 1) <- "Q"
x
```

```
## [1] "Qpple"  "Qanana" "Qear"
```




## 使用正则表达式进行模式匹配

正则表示式慢慢会呈现了

### 基础匹配

`str_view()` 是查看string是否匹配pattern，如果匹配，就高亮显示

```r
x <- c("apple", "banana", "pear")
str_view(string = x, pattern = "an")
```

```
## [2] │ b<an><an>a
```

有时候，我们希望在字符`a`前后都有字符（即，a处在两字符中间，如rap, bad, sad, wave，spear等等）

```r
x <- c("apple", "banana", "pear")
str_view(x, ".a.")
```

```
## [2] │ <ban>ana
## [3] │ p<ear>
```

这里的`.` 代表任意字符。如果向表达.本身呢？


```r
c("s.d") %>%
  str_view(".")
```

```
## [1] │ <s><.><d>
```


```r
c("s.d") %>%
  str_view("\\.")
```

```
## [1] │ s<.>d
```


### 锚点

希望`a`是字符串的开始

```r
x <- c("apple", "banana", "pear")
str_view(x, "^a")
```

```
## [1] │ <a>pple
```


希望`a`是一字符串的末尾

```r
x <- c("apple", "banana", "pear")
str_view(x, "a$")
```

```
## [2] │ banan<a>
```



```r
x <- c("apple pie", "apple", "apple cake")
str_view(x, "^apple$")
```

```
## [2] │ <apple>
```





### 字符类与字符选项

前面提到，`.`匹配任意字符，事实上还有很多这种**特殊含义**的字符：

* `\d`: matches any digit.
* `\s`: matches any whitespace (e.g. space, tab, newline).
* `[abc]`: matches a, b, or c.
* `[^abc]`: matches anything except a, b, or c.



```r
str_view(c("grey", "gray"), "gr[ea]y")
```

```
## [1] │ <grey>
## [2] │ <gray>
```








### 重复

控制匹配次数:

* `?`: 0 or 1
* `+`: 1 or more
* `*`: 0 or more




```r
x <- "Roman numerals: MDCCCLXXXVIII"
str_view(x, "CC?")
```

```
## [1] │ Roman numerals: MD<CC><C>LXXXVIII
```

```r
str_view(x, "X+")
```

```
## [1] │ Roman numerals: MDCCCL<XXX>VIII
```





控制匹配次数:

* `{n}`: exactly n
* `{n,}`: n or more
* `{,m}`: at most m
* `{n,m}`: between n and m





```r
x <- "Roman numerals: MDCCCLXXXVIII"
str_view(x, "C{2}")
```

```
## [1] │ Roman numerals: MD<CC>CLXXXVIII
```

```r
str_view(x, "C{2,}")
```

```
## [1] │ Roman numerals: MD<CCC>LXXXVIII
```

```r
str_view(x, "C{2,3}")
```

```
## [1] │ Roman numerals: MD<CCC>LXXXVIII
```


- 默认的情况，`*`, `+` 匹配都是**贪婪**的，也就是它会尽可能的匹配更多
- 如果想让它不贪婪，而是变得懒惰起来，可以在`*`, `+` 加个`?`




```r
x <- "Roman numerals: MDCCCLXXXVIII"

str_view(x, "CLX+")
```

```
## [1] │ Roman numerals: MDCC<CLXXX>VIII
```

```r
str_view(x, "CLX+?")
```

```
## [1] │ Roman numerals: MDCC<CLX>XXVIII
```


小结一下呢

<img src="images/regex_repeat.jpg" width="75%" />




### 分组与回溯引用



```r
ft <- fruit %>% head(10)
ft
```

```
##  [1] "apple"        "apricot"      "avocado"      "banana"       "bell pepper" 
##  [6] "bilberry"     "blackberry"   "blackcurrant" "blood orange" "blueberry"
```

我们想看看这些单词里，有哪些字母是重复两次的，比如`aa`, `pp`. 如果用上面学的方法

```r
str_view(ft, ".{2}", match = TRUE)
```

```
##  [1] │ <ap><pl>e
##  [2] │ <ap><ri><co>t
##  [3] │ <av><oc><ad>o
##  [4] │ <ba><na><na>
##  [5] │ <be><ll>< p><ep><pe>r
##  [6] │ <bi><lb><er><ry>
##  [7] │ <bl><ac><kb><er><ry>
##  [8] │ <bl><ac><kc><ur><ra><nt>
##  [9] │ <bl><oo><d ><or><an><ge>
## [10] │ <bl><ue><be><rr>y
```

发现不是和我们的预想不一样呢。

所以需要用到新技术 **分组与回溯引用**，

```r
str_view(ft, "(.)\\1", match = TRUE)
```

```
##  [1] │ a<pp>le
##  [5] │ be<ll> pe<pp>er
##  [6] │ bilbe<rr>y
##  [7] │ blackbe<rr>y
##  [8] │ blackcu<rr>ant
##  [9] │ bl<oo>d orange
## [10] │ bluebe<rr>y
```

- `.` 是匹配任何字符
- `(.)` 将匹配项括起来，它就用了一个名字，叫`\\1`； 如果有两个括号，就叫`\\1`和`\\2`
- `\\1` 表示回溯引用，表示引用`\\1`对于的`(.)`

所以`(.)\\1`的意思就是，匹配到了字符，后面还希望有个**同样的字符**


如果是匹配`abab`, `wcwc`

```r
str_view(ft, "(..)\\1", match = TRUE)
```

```
## [4] │ b<anan>a
```

如果是匹配`abba`, `wccw`呢？


```r
str_view(ft, "(.)(.)\\2\\1", match = TRUE)
```

```
## [5] │ bell p<eppe>r
```

是不是很神奇？

### 大小写敏感

希望找出线上平台是google和meet的记录，显然Google和google是一个意思，都应该被筛选出

```r
df <- tibble::tribble(
  ~tch_id,  ~online_platform,
      105,          "Google",
      106,            "meet",
      107,            "Zoom",
      108,            "zoom",
      109,     "Google Meet",
      112, "Microsoft Teams",
      113,                NA
  )
df
```

```
## # A tibble: 7 × 2
##   tch_id online_platform
##    <dbl> <chr>          
## 1    105 Google         
## 2    106 meet           
## 3    107 Zoom           
## 4    108 zoom           
## 5    109 Google Meet    
## 6    112 Microsoft Teams
## 7    113 <NA>
```

方法1，使用`stringr::regex()`

```r
df %>%
   filter(
      str_detect(online_platform, regex("google|meet", ignore_case = TRUE))
   )
```

```
## # A tibble: 3 × 2
##   tch_id online_platform
##    <dbl> <chr>          
## 1    105 Google         
## 2    106 meet           
## 3    109 Google Meet
```

方法2，使用`(?i)`

```r
df %>%
   filter(
      str_detect(online_platform, "(?i)google|meet")
   )
```

```
## # A tibble: 3 × 2
##   tch_id online_platform
##    <dbl> <chr>          
## 1    105 Google         
## 2    106 meet           
## 3    109 Google Meet
```


## 解决实际问题



### 确定一个字符向量是否匹配一种模式

实际问题中，想判断是否匹配？可以用到`str_detect()`函数

```r
x <- c("apple", "banana", "pear")
str_detect(x, "e")
```

```
## [1]  TRUE FALSE  TRUE
```

数据框中也是一样

```
## # A tibble: 3 × 1
##   x     
##   <chr> 
## 1 apple 
## 2 banana
## 3 pear
```


```r
d %>% mutate(has_e = str_detect(x, "e"))
```

```
## # A tibble: 3 × 2
##   x      has_e
##   <chr>  <lgl>
## 1 apple  TRUE 
## 2 banana FALSE
## 3 pear   TRUE
```

用于筛选也很方便


```r
d %>% dplyr::filter(str_detect(x, "e"))
```

```
## # A tibble: 2 × 1
##   x    
##   <chr>
## 1 apple
## 2 pear
```




`stringr::words`包含了牛津字典里常用单词

```r
stringr::words %>% head()
```

```
## [1] "a"        "able"     "about"    "absolute" "accept"   "account"
```

我们统计下以`t`开头的单词，有多少个？

```r
# How many common words start with t?
sum(str_detect(words, "^t"))
```

```
## [1] 65
```
我们又一次看到了**强制转换**.


以元音结尾的单词，占比多少？

```r
# proportion of common words end with a vowel?
mean(str_detect(words, "[aeiou]$"))
```

```
## [1] 0.2765306
```


放在数据框里看看, 看看以`x`结尾的单词是哪些？

```r
tibble(
  word = words
) %>%
  dplyr::filter(str_detect(word, "x$"))
```

```
## # A tibble: 4 × 1
##   word 
##   <chr>
## 1 box  
## 2 sex  
## 3 six  
## 4 tax
```




`str_detect()` 有一个功能类似的函数`str_count()`，区别在于，后者不是简单地返回是或否，而是返回字符串中匹配的数量


```r
x <- c("apple", "banana", "pear")
str_count(x, "a")
```

```
## [1] 1 3 1
```



```r
tibble(
  word = words
) %>%
  mutate(
    vowels = str_count(word, "[aeiou]"),
    consonants = str_count(word, "[^aeiou]")
  )
```

```
## # A tibble: 980 × 3
##    word     vowels consonants
##    <chr>     <int>      <int>
##  1 a             1          0
##  2 able          2          2
##  3 about         3          2
##  4 absolute      4          4
##  5 accept        2          4
##  6 account       3          4
##  7 achieve       4          3
##  8 across        2          4
##  9 act           1          2
## 10 active        3          3
## # ℹ 970 more rows
```








### 确定匹配的位置


大家放心，正则表达式不会重叠匹配。比如用`"aba"`去匹配`"abababa"`，肉眼感觉是三次，但正则表达式告诉我们是两次，因为不会重叠匹配


```r
str_count("abababa", "aba")
```

```
## [1] 2
```



```r
str_view_all("abababa", "aba")
```

```
## Warning: `str_view()` was deprecated in stringr 1.5.0.
## ℹ Please use `str_view_all()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## [1] │ <aba>b<aba>
```


这里插播一个正则表达式"\b"，它用于匹配单词字符与非单词字符（比如逗号、空格、句点）之间的位置。比如这里返回字符串中`s`的个数

```r
c("she sells seashells") %>% str_count(pattern = "s")
```

```
## [1] 6
```

这里的`s`后面必须跟一个非字符的符号，比如逗号、空格、句点等，因此是2个

```r
c("she sells seashells") %>% str_count(pattern = "s\\b")
```

```
## [1] 2
```


### 提取匹配的内容


```r
colours <- c(
  "red", "orange", "yellow",
  "green", "blue", "purple"
)
colour_match <- str_c(colours, collapse = "|")
colour_match
```

```
## [1] "red|orange|yellow|green|blue|purple"
```

colour_match 这里是一个字符串，放在pattern参数位置上也是正则表达式了,

这里注意以下两者的区别


```r
str_view("abcd", "ab|cd")
str_view("abc", "a[bc]d")
```




```r
more <- "It is hard to erase blue or red ink."
str_extract(more, pattern = colour_match)
```

```
## [1] "blue"
```



```r
str_extract_all(more, pattern = colour_match)
```

```
## [[1]]
## [1] "blue" "red"
```





```r
more <- sentences[str_count(sentences, colour_match) > 1]
more
```

```
## [1] "It is hard to erase blue or red ink."          
## [2] "The green light in the brown box flickered."   
## [3] "The sky in the west is tinged with orange red."
```
取出sentences中，含有有两种和两种颜色以上的句子。不过，不喜欢这种写法，看着费劲，还是用tidyverse的方法

```r
tibble(sentence = sentences) %>%
  filter(str_count(sentences, colour_match) > 1)
```

```
## # A tibble: 3 × 1
##   sentence                                      
##   <chr>                                         
## 1 It is hard to erase blue or red ink.          
## 2 The green light in the brown box flickered.   
## 3 The sky in the west is tinged with orange red.
```

`str_extract()`提取匹配, 谁先匹配就提取谁


```r
tibble(x = more) %>%
  mutate(color = str_extract(x, colour_match))
```

```
## # A tibble: 3 × 2
##   x                                              color 
##   <chr>                                          <chr> 
## 1 It is hard to erase blue or red ink.           blue  
## 2 The green light in the brown box flickered.    green 
## 3 The sky in the west is tinged with orange red. orange
```


`str_extract_all()`提取全部匹配项


```r
tibble(x = more) %>%
  mutate(color = str_extract_all(x, colour_match))
```

```
## # A tibble: 3 × 2
##   x                                              color    
##   <chr>                                          <list>   
## 1 It is hard to erase blue or red ink.           <chr [2]>
## 2 The green light in the brown box flickered.    <chr [2]>
## 3 The sky in the west is tinged with orange red. <chr [2]>
```


```r
tibble(x = more) %>%
  mutate(color = str_extract_all(x, colour_match)) %>%
  unnest(color)
```

```
## # A tibble: 6 × 2
##   x                                              color 
##   <chr>                                          <chr> 
## 1 It is hard to erase blue or red ink.           blue  
## 2 It is hard to erase blue or red ink.           red   
## 3 The green light in the brown box flickered.    green 
## 4 The green light in the brown box flickered.    red   
## 5 The sky in the west is tinged with orange red. orange
## 6 The sky in the west is tinged with orange red. red
```




### 替换匹配内容


只替换匹配的第一项

```r
x <- c("apple", "pear", "banana")
str_replace(x, "[aeiou]", "-")
```

```
## [1] "-pple"  "p-ar"   "b-nana"
```


替换全部匹配项

```r
str_replace_all(x, "[aeiou]", "-")
```

```
## [1] "-ppl-"  "p--r"   "b-n-n-"
```







### 拆分字符串

这个和`str_c()`是相反的操作


```r
lines <- "I love my country"
lines
```

```
## [1] "I love my country"
```



```r
str_split(lines, " ")
```

```
## [[1]]
## [1] "I"       "love"    "my"      "country"
```



```r
fields <- c("Name: Hadley", "Country: NZ", "Age: 35")
fields %>% str_split(": ", n = 2, simplify = TRUE)
```

```
##      [,1]      [,2]    
## [1,] "Name"    "Hadley"
## [2,] "Country" "NZ"    
## [3,] "Age"     "35"
```








## 进阶部分

带有条件的匹配

### look ahead

想匹配Windows，同时希望Windows右侧是`"95", "98", "NT", "2000"`中的一个

```r
win <- c("Windows2000", "Windows", "Windows3.1")
str_view(win, "Windows(?=95|98|NT|2000)")
```

```
## [1] │ <Windows>2000
```




```r
win <- c("Windows2000", "Windows", "Windows3.1")
str_view(win, "Windows(?!95|98|NT|2000)")
```

```
## [2] │ <Windows>
## [3] │ <Windows>3.1
```



Windows后面的 `()` 是匹配条件，事实上，有四种情形：

- `(?=pattern)`  要求此位置的后面必须匹配表达式pattern
- `(?!pattern)`  要求此位置的后面不能匹配表达式pattern
- `(?<=pattern)` 要求此位置的前面必须匹配表达式pattern
- `(?<!pattern)` 要求此位置的前面不能匹配表达式pattern


<div class="danger">
<p>注意：对于正则表达式引擎来说，它是从文本头部向尾部（从左到右）开始解析的，因此对于文本尾部方向，称为“前”，因为这个时候，正则引擎还没走到那块；而对文本头部方向，则称为“后”，因为正则引擎已经走过了那一块地方。</p>
</div>



### look behind



```r
win <- c("2000Windows", "Windows", "3.1Windows")
str_view(win, "(?<=95|98|NT|2000)Windows")
```

```
## [1] │ 2000<Windows>
```



```r
win <- c("2000Windows", "Windows", "3.1Windows")
str_view(win, "(?<!95|98|NT|2000)Windows")
```

```
## [2] │ <Windows>
## [3] │ 3.1<Windows>
```






## 案例分析

### 案例1

我们希望能提取第二列中的数值，构成新的一列


```r
dt <- tibble(
  x = 1:4,
  y = c("wk 3", "week-1", "7", "w#9")
)
dt
```

```
## # A tibble: 4 × 2
##       x y     
##   <int> <chr> 
## 1     1 wk 3  
## 2     2 week-1
## 3     3 7     
## 4     4 w#9
```







```r
dt %>%
  mutate(
    z = str_extract(y, "[0-9]")
  )
```

```
## # A tibble: 4 × 3
##       x y      z    
##   <int> <chr>  <chr>
## 1     1 wk 3   3    
## 2     2 week-1 1    
## 3     3 7      7    
## 4     4 w#9    9
```







### 案例2

提取第二列中的大写字母


```r
df <- data.frame(
  x = seq_along(1:7),
  y = c("2016123456", "20150513", "AB2016123456", "J2017000987", "B2017000987C", "aksdf", "2014")
)
df
```

```
##   x            y
## 1 1   2016123456
## 2 2     20150513
## 3 3 AB2016123456
## 4 4  J2017000987
## 5 5 B2017000987C
## 6 6        aksdf
## 7 7         2014
```





```r
df %>%
  mutate(
    item = str_extract_all(y, "[A-Z]")
  ) %>%
  tidyr::unnest(item)
```

```
## # A tibble: 5 × 3
##       x y            item 
##   <int> <chr>        <chr>
## 1     3 AB2016123456 A    
## 2     3 AB2016123456 B    
## 3     4 J2017000987  J    
## 4     5 B2017000987C B    
## 5     5 B2017000987C C
```







### 案例3

要求：中英文分开






```r
tb %>%
  tidyr::extract(
    # x, c("en", "cn"), "([:alpha:]+)([^:alpha:]+)",
    x, c("en", "cn"), "([a-zA-Z]+)([^a-zA-Z]+)",
    remove = FALSE
  )
```

```
## # A tibble: 3 × 3
##   x      en    cn   
##   <chr>  <chr> <chr>
## 1 I我    I     我   
## 2 love爱 love  爱   
## 3 you你  you   你
```



### 案例4

要求：提取起始数字


```r
df <- tibble(x = c("1-12week", "1-10week", "5-12week"))
df
```

```
## # A tibble: 3 × 1
##   x       
##   <chr>   
## 1 1-12week
## 2 1-10week
## 3 5-12week
```




```r
df %>% extract(
  x,
  # c("start", "end", "cn"), "([:digit:]+)-([:digit:]+)([^:alpha:]+)",
  c("start", "end", "cn"), "(\\d+)-(\\d+)(\\D+)",
  remove = FALSE
)
```

```
## # A tibble: 3 × 4
##   x        start end   cn   
##   <chr>    <chr> <chr> <chr>
## 1 1-12week 1     12    week 
## 2 1-10week 1     10    week 
## 3 5-12week 5     12    week
```





### 案例5

要求：提取大写字母后的数字


```r
df <- tibble(
  x = c("12W34", "AB2C46", "B217C", "akTs6df", "21WD4")
)
```



```r
df %>%
  mutate(item = str_extract_all(x, "(?<=[A-Z])[0-9]")) %>%
  tidyr::unnest(item)
```

```
## # A tibble: 5 × 2
##   x      item 
##   <chr>  <chr>
## 1 12W34  3    
## 2 AB2C46 2    
## 3 AB2C46 4    
## 4 B217C  2    
## 5 21WD4  4
```

思考题，

- 如何提取大写字母后的连续数字，比如B217C后面的217
- 如何提取提取数字前的大写字母？
- 为什么第一个正则表达式返回结果为"" 


```r
x <- "Roman numerals: MDCCCLXXXVIII"
str_match_all(x, "C?") 
```

```
## [[1]]
##       [,1]
##  [1,] ""  
##  [2,] ""  
##  [3,] ""  
##  [4,] ""  
##  [5,] ""  
##  [6,] ""  
##  [7,] ""  
##  [8,] ""  
##  [9,] ""  
## [10,] ""  
## [11,] ""  
## [12,] ""  
## [13,] ""  
## [14,] ""  
## [15,] ""  
## [16,] ""  
## [17,] ""  
## [18,] ""  
## [19,] "C" 
## [20,] "C" 
## [21,] "C" 
## [22,] ""  
## [23,] ""  
## [24,] ""  
## [25,] ""  
## [26,] ""  
## [27,] ""  
## [28,] ""  
## [29,] ""  
## [30,] ""
```

```r
str_match_all(x, "CC?")
```

```
## [[1]]
##      [,1]
## [1,] "CC"
## [2,] "C"
```
 
 "?"的意思是匹配0次或者1次

### 案例6

提取数字并求和


```r
df <- tibble(
  x = c("1234", "B246", "217C", "2357f", "21WD4")
)
df
```

```
## # A tibble: 5 × 1
##   x    
##   <chr>
## 1 1234 
## 2 B246 
## 3 217C 
## 4 2357f
## 5 21WD4
```



```r
df %>%
  mutate(num = str_match_all(x, "\\d")) %>%
  unnest(num) %>%
  mutate_at(vars(num), as.numeric) %>%
  group_by(x) %>%
  summarise(sum = sum(num))
```

```
## # A tibble: 5 × 2
##   x       sum
##   <chr> <dbl>
## 1 1234     10
## 2 217C     10
## 3 21WD4     7
## 4 2357f    17
## 5 B246     12
```

### 案例7

```r
text <- "Quantum entanglement is a physical phenomenon that occurs when pairs or groups of particles are generated, interact, or share spatial proximity in ways such that the quantum state of each particle cannot be described independently of the state of the others, even when the particles are separated by a large distance."


pairs <-
  tibble::tribble(
    ~item, ~code,
    "Quantum entanglement", "A01",
    "physical phenomenon", "A02",
    "quantum state", "A03",
    "quantum mechanics", "A04"
  ) %>%
  tibble::deframe()



text %>% str_replace_all(pairs)
```

```
## [1] "A01 is a A02 that occurs when pairs or groups of particles are generated, interact, or share spatial proximity in ways such that the A03 of each particle cannot be described independently of the state of the others, even when the particles are separated by a large distance."
```




## 回答提问

回到上课前的提问：如何提取`Sichuan Univ`后面的学院？

```
## # A tibble: 8 × 2
##      No address                      
##   <int> <chr>                        
## 1     1 Sichuan Univ, Coll Chem      
## 2     2 Sichuan Univ, Coll Elect Engn
## 3     3 Sichuan Univ, Dept Phys      
## 4     4 Sichuan Univ, Coll Life Sci  
## 5     6 Sichuan Univ, Food Engn      
## 6     7 Sichuan Univ, Coll Phys      
## 7     8 Sichuan Univ, Sch Business   
## 8     9 Wuhan Univ, Mat Sci
```



```r
d %>%
  dplyr::mutate(
    coll = str_extract(address, "(?<=Sichuan Univ,).*")
  ) %>%
  tidyr::unnest(coll, keep_empty = TRUE)
```

```
## # A tibble: 8 × 3
##      No address                       coll              
##   <int> <chr>                         <chr>             
## 1     1 Sichuan Univ, Coll Chem       " Coll Chem"      
## 2     2 Sichuan Univ, Coll Elect Engn " Coll Elect Engn"
## 3     3 Sichuan Univ, Dept Phys       " Dept Phys"      
## 4     4 Sichuan Univ, Coll Life Sci   " Coll Life Sci"  
## 5     6 Sichuan Univ, Food Engn       " Food Engn"      
## 6     7 Sichuan Univ, Coll Phys       " Coll Phys"      
## 7     8 Sichuan Univ, Sch Business    " Sch Business"   
## 8     9 Wuhan Univ, Mat Sci            <NA>
```


当然还有其他的解决办法

```r
d %>% mutate(
  coll = str_remove_all(address, ".*,")
)
```


```r
d %>% tidyr::separate(
  address,
  into = c("univ", "coll"), sep = ",", remove = FALSE
)
```



```r
d %>%
  tidyr::extract(
    address, c("univ", "coll"), "(Sichuan Univ), (.+)",
    remove = FALSE
  )
```
