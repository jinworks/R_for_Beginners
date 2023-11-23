
\mainmatter

# (PART) R编程基础 {-}

# R语言介绍及资料推荐 {#baseR-intro-ds}
## R语言的特点
R语言是用于统计分析、图形表示和报告的编程语言:

-   R 是免费的，R 可运行于多种平台之上，包括Windows、UNIX 和 Mac OS X
-   R语言做统计分析，实现了经典的、现代的统计方法，如参数和非参数假设检验、线性回归、广义线性回归、非线性回归、可加模型、树回归、混合模型、方差分析、判别、聚类、时间序列分析等。
-   R 拥有顶尖水准的**制图**功能，ggplot2画图，是颜值担当，非常好看，一直被模仿，从未被超越
-   R 应用广泛，拥有丰富的**库包**，统计科研工作者广泛使用R进行计算和发表算法。R有上万扩展包
- tidyverse来编程，代码可读性强，用的是**人类语言**， 非常好学 
- Rmarkdown 可以生成html、word或者pdf格式的可重复性报告文档，可以方便快捷做幻灯片、海报、论文、书籍、网页。
-   活跃的**社区**

## R能干什么

### 数据科学流程
Hadley Wickham将[数据科学流程](https://r4ds.had.co.nz/)分解成6个环节

<img src="images/data-science-explore.png" width="80%" style="display: block; margin: auto;" />

即数据导入、数据规整、数据处理、可视化、建模以及形成可重复性报告，整个分析和探索过程都在一个程序代码中完成，这种方式对训练我们的数据思维非常有帮助。


### tidyverse家族

R 语言的强大还在于各种宏包。但由于各种包接口不统一，语法不一致，也带来一些困扰。为了解决这个问题，RStudio 公司的[Hadley Wickham](http://hadley.nz) 与其带领的团队推出了`tidyverse`宏包， [tidyverse](https://www.tidyverse.org)将常用的宏包整合在一起，并保持了语法的一致性。
本书将通过一些例子不断地展示`tidyverse`在数据分析和可视化的应用。

<img src="images/tidyverse.png" width="70%" style="display: block; margin: auto;" />

[tidyverse](https://www.tidyverse.org/)套餐，其主要成员包括


| 功能 | 宏包        |
|:------|:-------------|
颜值担当 | ggplot2 |
数据处理王者 | dplyr |
数据转换专家  | tidyr |
数据载入利器 | readr |
循环加速器 | purrr |
强化数据框 | tibble |
字符串处理 | stringr |
因子处理   | forcats |


 
###  R & tidyverse 四大优势

| 序号 | 内容   |  代码演示   |
|:---|:---|:---|
| 1     |  统计分析           |   `<a href="data:text/plain;base64,IyDorqHnrpcKMSArIDUKCgoKIyDnlJ/miJDluo/liJcKMToxMDAKCgoKIyDmiorluo/liJfoo4XliLDlj5jph4/ph4wKeCA8LSAxOjEwMAoKCgojIOaxguWSjApzdW0oeCkKCgoKIyDmsYLlnYflgLwKbWVhbih4KQoKCgojIOaxguaWueW3rgpzZCh4KQoKCiMg55Sf5oiQ5q2j5oCB5YiG5biD55qE6ZqP5py65pWwCnJub3JtKDIwLCBtZWFuID0gMCwgc2QgPSAxKQoKCgojIFTmo4DpqowKdC50ZXN0KGV4dHJhIH4gZ3JvdXAsIGRhdGEgPSBzbGVlcCkKCgojIOaWueW3ruWIhuaekAphb3YobXBnIH4gd3QsIGRhdGEgPSBtdGNhcnMpCgoKCiMg57q/5oCn5qih5Z6LCmxtKG1wZyB+IHd0LCBkYXRhID0gbXRjYXJzKQoK" download="01_stats.R">Download 01_stats.R</a>`{=html} |
| 2     |  可视化ggplot2         |   `<a href="data:text/plain;base64,bGlicmFyeShnZ3Bsb3QyKQoKZ2dwbG90KG1wZywgYWVzKHggPSBkaXNwbCwgeSA9IGh3eSkpICsKICBnZW9tX3BvaW50KGFlcyhjb2xvdXIgPSBjbGFzcykpCg==" download="02_visual.R">Download 02_visual.R</a>`{=html} |
| 3     |  探索性分析     |   `<a href="data:text/plain;base64,bGlicmFyeSh0aWR5dmVyc2UpCgojIOahiOS+i+S4gO+8mumjk+mjjuaVsOaNrumbhgoKc3Rvcm1zICU+JSBjb3VudCh5ZWFyKQoKc3Rvcm1zICU+JQogIGdyb3VwX2J5KHllYXIpICU+JQogIHN1bW1hcml6ZSgKICAgIHdpbmRfbWVhbiA9IG1lYW4od2luZCksCiAgICAgIHdpbmRfc2QgPSBzZCh3aW5kKQogICkKCgoKCgojIOahiOS+i+S6jO+8mlZD5YmC6YeP5ZKM5ZaC6aOf5pa55rOV5a+56LGa6byg54mZ6b2/55qE5b2x5ZON77yfCiMg5Y+M5Zug57Sg5pa55beu5YiG5p6QIChBTk9WQSkKCm15X2RhdGEgPC0gVG9vdGhHcm93dGggJT4lIAogIG11dGF0ZSgKICAgIGFjcm9zcyhjKHN1cHAsIGRvc2UpLCBhc19mYWN0b3IpIAogICAgKQoKCm15X2RhdGEgJT4lIAogIGdncGxvdChhZXMoeCA9IHN1cHAsIHkgPSBsZW4sIGZpbGwgPSBzdXBwKSkgKyAKICBnZW9tX2JveHBsb3QocG9zaXRpb24gPSBwb3NpdGlvbl9kb2RnZSgpKSArCiAgZmFjZXRfd3JhcCh2YXJzKGRvc2UpKQoKCgphb3YobGVuIH4gc3VwcCArIGRvc2UsIGRhdGEgPSBteV9kYXRhKSAKCgphb3YobGVuIH4gc3VwcCArIGRvc2UsIGRhdGEgPSBteV9kYXRhKSAlPiUgCiAgVHVrZXlIU0Qod2hpY2ggPSAiZG9zZSIpICU+JSAKICBicm9vbTo6dGlkeSgpCgoKCgoKCg==" download="03_eda.R">Download 03_eda.R</a>`{=html} |
| 4     |  可重复性报告   |   `<a href="data:text/x-markdown;base64,LS0tCnRpdGxlOiAi6L+Z5piv5LiA5Lu95YWz5LqO5paw5Yag6IK654KO55qE5o6i57Si5oCn5YiG5p6Q5oql5ZGKIgphdXRob3I6ICLnjovlsI/kuowiCmRhdGU6ICJgciBTeXMuRGF0ZSgpYCIKb3V0cHV0OgogIHBkZl9kb2N1bWVudDogCiAgICBsYXRleF9lbmdpbmU6IHhlbGF0ZXgKICAgIGV4dHJhX2RlcGVuZGVuY2llczoKICAgICAgY3RleDogVVRGOAogICAgbnVtYmVyX3NlY3Rpb25zOiB5ZXMKICAgICN0b2M6IHllcwogICAgZGZfcHJpbnQ6IGthYmxlCmNsYXNzb3B0aW9uczogImh5cGVycmVmLCAxMnB0LCBhNHBhcGVyIgotLS0KCgpgYGB7ciBzZXR1cCwgaW5jbHVkZT1GQUxTRX0Ka25pdHI6Om9wdHNfY2h1bmskc2V0KGVjaG8gPSBUUlVFLCAKICAgICAgICAgICAgICAgICAgICAgIG1lc3NhZ2UgPSBGQUxTRSwgCiAgICAgICAgICAgICAgICAgICAgICB3YXJuaW5nID0gRkFMU0UsCiAgICAgICAgICAgICAgICAgICAgICBmaWcuYWxpZ24gPSAiY2VudGVyIgogICAgICAgICAgICAgICAgICAgICAgKQpgYGAKCgojIOW8leiogAoK5paw5Z6L5Yag54q255eF5q+S55ar5oOF5Zyo5aSa5Zu96JST5bu277yM5LiA5Lqb5Zu95a6255qE55eF5L6L56Gu6K+K5pWw6YeP5piO5pi+5aKe5aSa77yM5ZCE5Zu96Ziy55ar5Yqb5bqm57un57ut5Yqg5by644CC5pys56ug6YCa6L+H5YiG5p6Q55ar5oOF5pWw5o2u77yM5LqG6Kej55ar5oOF5Y+R5bGV77yM56Wd5oS/5Lq657G75pep5pel5Lya5oiY6IOc55eF5q+S77yBCgoKIyDlr7zlhaXmlbDmja4KCummluWFiO+8jOaIkeS7rOWKoOi9veWuj+WMhXRpZHl2ZXJzZQoKYGBge3IsIHdhcm5pbmc9RkFMU0UsIG1lc3NhZ2U9RkFMU0V9CmxpYnJhcnkodGlkeXZlcnNlKSAKbGlicmFyeShjb3ZkYXRhKQpgYGAKCgrorrrmlofnmoTmlbDmja7mnaXmupAKW2h0dHBzOi8va2poZWFseS5naXRodWIuaW8vY292ZGF0YS9dKGh0dHBzOi8va2poZWFseS5naXRodWIuaW8vY292ZGF0YS8p77yM5oiR5Lus6YCJ5Y+W5pyA6L+R5pWw5o2u55yL55yLCgoKYGBge3IsIGVjaG8gPSBGQUxTRX0KY292bmF0ICU+JSAKICB0YWlsKDgpIApgYGAKCgoKIyDmlbDmja7lj5jph48KCui/meS4quaVsOaNrumbhuWMheWQqzjkuKrlj5jph4/vvIzlhbfkvZPlkKvkuYnlpoLkuIvvvJoKCnwg5Y+Y6YePICAgICAgCXwg5ZCr5LmJICAgICAgICAgICAgICAgCXwKfC0tLS0tLS0tLS0tCXwtLS0tLS0tLS0tLS0tLS0tLS0tLQl8CnwgZGF0ZSAgICAgIAl8IOaXpeacnyAgICAgICAgICAgICAgIAl8CnwgY25hbWUgICAgIAl8IOWbveWutuWQjSAgICAgICAgICAgICAJfAp8IGlzbzMgICAgICAJfCDlm73lrrbnvJbnoIEgICAgICAgICAgIAl8CnwgY2FzZXMgICAgIAl8IOehruiviueXheS+iyAgICAgICAgICAgCXwKfCBkZWF0aHMgICAgCXwg5q275Lqh55eF5L6LICAgICAgICAgICAJfAp8IHBvcCAgICAgICAJfCAyMDE55bm05Zu95a625Lq65Y+j5pWw6YePIAl8CnwgY3VfY2FzZXMgIAl8IOe0r+enr+ehruiviueXheS+iyAgICAgICAJfAp8IGN1X2RlYXRocyAJfCDntK/np6/mrbvkuqHnl4XkvosgICAgICAgCXwKCgoKIyDmlbDmja7mjqLntKIKCuaJvuWHuioq57Sv56ev56Gu6K+K55eF5L6LKirmnIDlpJrnmoTlh6DkuKrlm73lrrYKCmBgYHtyfQpjb3ZuYXQgJT4lIAogIHVuZ3JvdXAoKSAlPiUgCiAgZmlsdGVyKGRhdGUgPT0gbWF4KGRhdGUpKSAlPiUgCiAgc2xpY2VfbWF4KGN1X2Nhc2VzLCBuID0gOCkKYGBgCgoK5YWo55CDKirnoa7or4rnl4XkvosqKuWSjCoq5q275Lqh55eF5L6LKioKCmBgYHtyfQpjb3ZuYXQgJT4lIAogIHVuZ3JvdXAoKSAlPiUgCiAgc3VtbWFyaXNlKAogICAgdG90YWxfY2FzZXMgPSBzdW0oY2FzZXMpLAogICAgdG90YWxfZGVhdGggPSBzdW0oZGVhdGhzKSwKICApCmBgYAoKCgrnvo7lm73nlqvmg4Xlj5jljJbmg4XlhrUKCmBgYHtyLCBmaWcuc2hvd3RleHQgPSBUUlVFfQplbGVjdF9kZiA8LSB0aWJibGUoCiAgc3RhcnQgPSBjKCIyMDIwLTEwLTAxIiksCiAgZW5kICAgPSBjKCIyMDIwLTExLTAzIikKKSAlPiUKICBtdXRhdGUoYWNyb3NzKGV2ZXJ5dGhpbmcoKSwgYXMuRGF0ZSkpCgpjb3ZuYXQgJT4lCiAgZmlsdGVyKGlzbzMgPT0gIlVTQSIpICU+JQogIGZpbHRlcihjdV9jYXNlcyA+IDApICU+JQogIHVuZ3JvdXAoKSAlPiUKICBnZ3Bsb3QoKSArCiAgZ2VvbV9saW5lKGFlcyh4ID0gZGF0ZSwgeSA9IGNhc2VzKSkgKwogIGdlb21fcmVjdCgKICAgIGRhdGEgPSBlbGVjdF9kZiwKICAgIGFlcyh4bWluID0gc3RhcnQsIHhtYXggPSBlbmQsIHltaW4gPSAtSW5mLCB5bWF4ID0gK0luZiksCiAgICBmaWxsID0gImdyYXkiLCBhbHBoYSA9IDAuNQogICkgKwogIHNjYWxlX3hfZGF0ZShuYW1lID0gTlVMTCwgYnJlYWtzID0gIm1vbnRoIikgKwogIGxhYnMoCiAgICB0aXRsZSA9ICLnvo7lm73mlrDlhqDogrrngo7ntK/np6/noa7or4rnl4XkvosiLAogICAgc3VidGl0bGUgPSAi5pWw5o2u5p2l5rqQaHR0cHM6Ly9ramhlYWx5LmdpdGh1Yi5pby9jb3ZkYXRhLyIKICApIApgYGAKCgo=" download="04_reproducible.Rmd">Download 04_reproducible.Rmd</a>`{=html} |


##  推荐资料
### 教程
- 王敏杰 (2023). 数据科学中的 R 语言，https://bookdown.org/wangminjie/R4DS/, 2nd ed. 讲基本的数据整理、汇总。
- 李东风(2023). R语言教程, https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/index.html
- Hadley Wickham and Garrett Grolemund(2022). R for Data Science，https://r4ds.hadley.nz/, 2nd ed. 讲基本的数据整理、汇总。
- Hadley Wickham(2019). Advanced R, 2nd ed., https://adv-r.hadley.nz/, Chapman & Hall/CRC The R Series. 高级R编程，属于对R高级编程技术的讲解
- Hadley Wickham(2016). ggplot2 Elegant Graphics for Data Analysis, 2nd ed., https://ggplot2-book.org/, Springer. 优雅易用的R作图功能
- Susan Holmes, Wolfgang Huber(2020). Modern Statistics for Modern Biology, https://www.huber.embl.de/msmb/index.html.
R的统计功能在生物学中的应用。
- 汤银才（2008），《R语言与统计分析》，高等教育出版社。

### R 语言社区推荐

R 语言社区非常友好，可以在这里找到你问题的答案（一般直接从浏览器搜索问题就好）。下面的前两个有丰富且漂亮的画图代码（强推）：

- Statistical tools for high-throughput data analysis: <http://sthda.com/english/> (e.g., <http://sthda.com/english/articles/24-ggpubr-publication-ready-plots/76-add-p-values-and-significance-levels-to-ggplots/>)

- The R graph gallery: <https://r-graph-gallery.com/> (e.g., <https://r-graph-gallery.com/piechart-ggplot2.html>)

- R-Bloggers: <https://www.r-bloggers.com/>

- stackoverflow: <https://stackoverflow.com/questions/tagged/r>
  
