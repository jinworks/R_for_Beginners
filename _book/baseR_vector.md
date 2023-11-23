# 向量 {#baseR-vectors}

向量是R语言最基础的数据类型。

我们把6这个数放入盒子（对象） `x`，


```r
x <- 6
```

现在，我们想多装一些数据（有顺序、好取出），比如`3,4,5,6,7`。为了方便管理，那我们就希望这些数有一定的顺序，并且按照一定的结构组织在一起，我能想到的最好的结构就是---我们小时候吃的冰糖葫芦，中间用一根木棒把水果串起来，有先后顺序，而且当做一个整体方便取出。

<img src="images/vector_like11.jpg" width="30%" />

对应到R语言里，我们可以用 `c()` 函数实现类似**结构**，一个水果对应一个数值


<img src="images/vector_like.jpg" width="100%" />

对应到R语言里，我们可以用 `c()` 函数实现类似**结构**

```r
x <- c(3, 4, 5, 6, 7)
x
```

```
## [1] 3 4 5 6 7
```

我们观察到`c()`函数构造向量的几个要求

- 这里的`c`就是 combine 或 concatenate 的意思
- 它要求元素之间用**英文的逗号**分隔
- 且元素的数据类型是统一的，比如这里都是数值

这样，`c()` 函数把一组数据聚合到了一起，就构成了一个**向量**。


### 聚合成新向量

`c()` 函数还可以把两个向量聚合成一个新的向量。

```r
low      <- c(1, 2, 3)
high     <- c(4, 5, 6)
sequence <- c(low, high)
sequence
```

```
## [1] 1 2 3 4 5 6
```


### 命名向量(named vector)

相比与向量`c(5, 6, 7, 8)`， 每个元素可以有自己的名字

```r
x <- c('a' = 5, 'b' = 6, 'c' = 7, 'd' = 8)
x
```

```
## a b c d 
## 5 6 7 8
```

或者

```r
x <- c(5, 6, 7, 8)
names(x) <- c('a', 'b', 'c', 'd')
x
```

```
## a b c d 
## 5 6 7 8
```

## 数值型向量

向量的元素都是数值类型，因此也叫数值型向量。数值型的向量，有 integer 和 double 两种：


```r
x <- c(1L, 5L, 2L, 3L)    # 整数型
x <- c(1.5, -0.5, 2, 3)   # 双精度类型，常用写法
```

如果向量元素很多，用手工一个个去输入，那就成了体力活，不现实。**在特定情况下**，有几种偷懒方法:

- `seq()` 函数可以生成等差数列，`from` 参数指定数列的起始值，`to` 参数指定数列的终止值，`by` 参数指定数值的间距：


```r
s1 <- seq(from = 0, to = 10, by = 0.5)
s1
```

```
##  [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
## [16]  7.5  8.0  8.5  9.0  9.5 10.0
```



- `rep()` 是 repeat（重复）的意思，可以用于产生重复出现的数字序列：`x` 用于重复的向量，`times` 参数可以指定要生成的个数，`each` 参数可以指定每个元素重复的次数


```r
s2 <- rep(x = c(0, 1), times = 3)
s2
```

```
## [1] 0 1 0 1 0 1
```

```r
s3 <- rep(x = c(0, 1), each = 3)
s3
```

```
## [1] 0 0 0 1 1 1
```



- `m:n`，如果单纯是要生成数值间距为1的数列，用 `m:n` 更快捷，它产生从 m 到 n 的间距为1的数列

```r
s4 <- 0:10  # Colon operator (with by = 1):
s4
```

```
##  [1]  0  1  2  3  4  5  6  7  8  9 10
```




```r
s5 <- 10:1
s5
```

```
##  [1] 10  9  8  7  6  5  4  3  2  1
```

## 字符串型向量

字符串（String）数据类型，实际上就是文本类型，必须用单引号或者是双引号包含，例如：



```r
x <- c("a", "b", "c")    
x <- c('Alice', 'Bob', 'Charlie', 'Dave')    
x <- c("hello", "baby", "I love you!") 
```

需要注意的是，`x1`是字符串型向量，`x2`是数值型向量

```r
x1 <- c("1", "2", "3")
x2 <- c(1, 2, 3)
```



## 逻辑型向量

逻辑型常称为布尔型（Boolean）， 它的常量值只有 `TRUE` 和 `FALSE`。注意 `TRUE` 和 `FALSE` 是在R语言中的保留词汇，所谓保留词汇，就是专用词，类似圆周率$\pi$(`pi`)，特指某个含义，请勿将保留词汇用作变量名。


```r
x <- c(TRUE, TRUE, FALSE, FALSE)
x <- c(T, T, F, F)            # Equivalent, but not recommended
```

`TRUE`和`FALSE`必须都大写，不能写成下面这些形式

```r
x <- c(True, False)          
x <- c(true, false)          
```


注意以下两者不要混淆，`x1`是逻辑型向量，`x2`是字符串型向量

```r
x1 <- c(TRUE, FALSE)             # logical
x2 <- c("TRUE", "FALSE")         # character
```

因为`TRUE`和`FALSE`是专用词，不加引号，如果加了引号，就是字符串型向量。


## 因子型向量

因子型可以看作是字符串向量的增强版，它是带有层级（Levels）的字符串向量。比如这里四个季节的名称，他们构成一个向量


```r
four_seasons <- c("spring", "summer", "autumn", "winter")
four_seasons
```

```
## [1] "spring" "summer" "autumn" "winter"
```

我们使用 `factor()` 函数可以将这里的字符串向量转换成因子型向量


```r
four_seasons_factor <- factor(four_seasons)
four_seasons_factor
```

```
## [1] spring summer autumn winter
## Levels: autumn spring summer winter
```

查看因子型向量的时候，也会输出了层级信息，默认的情况，它是按照字符串首字母的顺序排序，当然也可以指定顺序，这个时候，我们就需要指定层级信息，比如指定我对四个季节喜欢的顺序为c("summer", "winter", "spring", "autumn")


```r
four_seasons <- c("spring", "summer", "autumn", "winter")
four_seasons_factor <- factor(four_seasons, 
                              levels = c("summer", "winter", "spring", "autumn")
                              )
four_seasons_factor
```

```
## [1] spring summer autumn winter
## Levels: summer winter spring autumn
```





## 小结

<img src="images/create_vectors.png" width="100%" />






## 习题

- 请说出fun3的结果

```r
fun <- c("programming", "in", "R") 
fun2 <- c("Have", "fun")
fun3 <- c(fun2, fun)
fun3
```

- 数据类型必须一致是构建向量的基本要求，如果数值型、字符串型和逻辑型写在一起，用`c()`函数构成向量，猜猜会发生什么？


```r
x <- c(1, "USA", TRUE)
x
```

- 形容温度的文字

```r
temperatures <- c("warm", "hot", "cold")
```

要求转换成因子类型向量，并按照温度从高到低排序


