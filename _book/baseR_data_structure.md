# 数据结构 {#baseR-data-structure}

前面介绍了向量，它是R语言中最基础的数据结构

<img src="images/vector_like2.jpg" width="70%" />


我们还会遇到其它数据结构

- 矩阵
- 列表
- 数据框

这些数据结构都可以看作由向量衍生出来的^[数据结构还包括array，但因为它的使用频次不及向量、矩阵、列表和数据框多，所以暂时不介绍。]。



## 矩阵

矩阵可以存储行(row)和列(column)二维的数据。


<img src="images/matrix.jpg" width="50%" />

它实际上是向量的另一种表现形式，也就说它的本质还是向量，一维的向量用二维的方式呈现。


矩阵可以用 `matrix()` 函数创建，第一个位置的参数是用于创建矩阵的向量。比如下面把向量`c(2, 4, 3, 1, 5, 7)` 转换成2行3列的矩阵


```r
m <- matrix(
  c(2, 4, 3, 1, 5, 7),
  nrow = 2, 
  ncol = 3
)

m
```

```
##      [,1] [,2] [,3]
## [1,]    2    3    5
## [2,]    4    1    7
```

<img src="images/create_matrix.jpg" width="100%" />


大家还记得我们的向量是一个竖着的糖葫芦， 那么在转换成矩阵的时候，也是先竖着排，第一列竖着的方法排满后，就排第二列，这是默认的情形。如果想改变这一传统习惯，也可以增加一个语句 `byrow = TRUE`，这条语句让向量先横着排，排完第一行，再排第二行。


```r
matrix(
  c(2, 4, 3, 1, 5, 7),
  nrow = 2, 
  ncol = 3,
  byrow = TRUE
)
```

```
##      [,1] [,2] [,3]
## [1,]    2    4    3
## [2,]    1    5    7
```

### 矩阵的属性

- 类型


```r
class(m)
```

```
## [1] "matrix" "array"
```


- 长度


```r
length(m)
```

```
## [1] 6
```


- 维度

```r
dim(m)
```

```
## [1] 2 3
```




## 列表

如果我们想要装更多的东西，可以想象有一个小火车^[列表与向量一样，也是竖着的，但为了方便大家理解，我这里把它横过来看，就像一个小火车]，小火车的每节车厢是独立的，因此每节车厢装的东西可以不一样。这种结构装载数据的能力很强大，称之为**列表**（list）。我们可以使用`list()`函数创建列表


<img src="images/list_trian.png" width="95%" />



```r
list1 <- list(
  a = c(5, 10),
  b = c("I", "love", "R", "language", "!"),
  c = c(TRUE, TRUE, FALSE, TRUE)
)
list1
```

```
## $a
## [1]  5 10
## 
## $b
## [1] "I"        "love"     "R"        "language" "!"       
## 
## $c
## [1]  TRUE  TRUE FALSE  TRUE
```


<img src="images/create_list.jpg" width="100%" />


<!-- ::: {.rmdnote} -->

<!-- `list()` 函数创建列表 Vs. `c()` 函数创建向量： -->

<!-- - **相同点**：元素之间用逗号分开。 -->
<!-- - **不同点**： -->
<!--   - 向量的元素是单个值；列表的元素可以是更复杂的结构，可以是单个值，也可以是向量、矩阵或者列表。 -->
<!--   - 向量要求每个元素的数据类型必须相同，要么都是数值型，要么都是字符型；而列表的元素允许不同的数据类型。 -->

<!-- ::: -->


`c()` 函数创建向量 对比 `list()` 函数创建列表

<img src="images/c_vs_list.png" width="100%" />


### 列表的属性

- 类型


```r
class(list1)
```

```
## [1] "list"
```


- 长度

```r
length(list1)
```

```
## [1] 3
```



## 数据框
前面说过，列表可以想象成一个小火车，如果每节车厢装的**都是向量而且等长**，那么这种特殊形式的列表就变成了**数据框** (data frame)

<img src="images/dataframe.jpg" width="100%" />



换句话说，**数据框**是一种特殊的列表，我们可以使用 `data.frame()` 函数构建数据框


```r
df <- data.frame(
  name      = c("Alice", "Bob", "Carl", "Dave"),
  age       = c(23, 34, 23, 25),
  marriage  = c(TRUE, FALSE, TRUE, FALSE),
  color     = c("red", "blue", "orange", "purple")
)
df
```

```
##    name age marriage  color
## 1 Alice  23     TRUE    red
## 2   Bob  34    FALSE   blue
## 3  Carl  23     TRUE orange
## 4  Dave  25    FALSE purple
```



<img src="images/create_dataframe.png" width="100%" />

数据框类似于我们经常用的excel表格。由于数据框融合了向量、列表和矩阵的特性，所以在数据科学的统计建模和可视化中运用非常广泛。

### 数据框的属性

- 类型


```r
class(df) 
```

```
## [1] "data.frame"
```


- 维度

```r
nrow(df)
```

```
## [1] 4
```

```r
ncol(df)
```

```
## [1] 4
```


## 小结

R 对象的数据结构(向量、矩阵、列表和数据框)，总结如下

- 向量: 糖葫芦
- 矩阵: 糖葫芦，有多行多列
- 列表: 小火车
- 数据框: excel表格
<!-- - array: 土司面包一样 -->






<img src="images/data_structure1.png" width="90%" />



## 习题

- 为什么说数据框融合了向量、矩阵和列表的特性？
- 创建一个学生信息的data.frame，包含姓名、性别、年龄，成绩等变量。
