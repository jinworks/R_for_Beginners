# 函数应用 {#baseR-functions-adv}

## 灵活的语法

我们对输入的向量每个元素计算平方，可以这样写函数

```r
mysquare <- function(x) {
   y <- x^2
   return(y)  
}
```

R语言里面，完成一件事情往往有很多种方法，比如

方法1：

```r
mysquare <- function(x) {
    return(x^2)
}
```


方法2：

```r
mysquare <- function(x) { return(x^2) }
```



方法3：

```r
mysquare <- function(x) return(x^2)
```


方法4：

```r
mysquare <- function(x) {
    x^2
}
```


方法5：

```r
mysquare <- function(x) x^2
```




## 多个参数


```r
sum_two <- function(num1, num2) {
  sum  <- num1 + num2
  return(sum)
}


sum_two(num1 = 1, num2 = 2)
```

```
## [1] 3
```

```r
sum_two(12, 9)
```

```
## [1] 21
```


练习：说说这个函数的意思

```r
norm_by_y <- function(num1, num2) {
   result  <- (num1 - num2)/num2
   return(result)
}
```



## 条件语句

使用 `if-else` 语句

```r
if(condition) {
   Do something
} else {
   Alternative something
}
```



比如，先判断是否为数值，如果是返回它的平方，如果不是数值，就返回提示语句



```r
square_if <- function(num) {
    if (is.numeric(num)) {
      num^2
    } else {
     "Your input is not numeric."
    }
}


square_if("a")
```

```
## [1] "Your input is not numeric."
```

```r
square_if(3)
```

```
## [1] 9
```




练习：将上面`sum_two()`函数增加数据类型判断语句，让函数更安全。

```r
sum_two("a", "b")
```


多个条件的，就需要`if-else if-else`语句，比如这里判断一个数是正数、负数还是0

```r
check_number <- function(x) {
  if (x < 0) {
    print("Negative number")
  } else if (x > 0) {
    print("Positive number")
  } else {
    print("Zero")
  }
}


x <- 0
check_number(x)
```

```
## [1] "Zero"
```


## 返回多个结果

如果要返回多个统计结果，可以把结果先放在list或者data.frame中，然后再返回。

```r
mystat <- function(x){
   meanval <- mean(x) 
   sdval <- sd(x)
   
   list(sd = sdval, mean = meanval)
}
```


或者

```r
mystat <- function(x){
   meanval <- mean(x) 
   sdval <- sd(x)
   
   data.frame(  
     sd = sdval, 
     mean = meanval
   )
}
```



## 更多

冰淇淋放大器，这个机器有很多档位，可以让冰淇淋放大指定的倍数，默认为10倍


```r
enlarge_icecream <- function(multi = 10) {
  function(icecream) {
    icecream * multi
  }
}
```
注意：不建议这样写嵌套函数，可以写子函数，然后再调用，这样程序的可读性更强，更清晰。 

