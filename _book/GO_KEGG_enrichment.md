
# (PART) GO/KEGG富集分析 {-}

# GO/KEGG功能富集分析 {#GO-KEGG}

## 功能富集分析介绍
功能富集分析对于解释转录组学数据至关重要。转录组鉴定了差异表达基因后，通常会进行GO或KEGG富集分析，识别这些差异基因的功能或参与调控的通路，来说明关键基因表达上调/或下调后可能会导致哪功能或通路被激活/或抑制，进而与表型进行联系。

目前能够进行GO、KEGG富集分析的工具有很多，不同的工具之间在算法、数据库上存在不同，因此结果也可能大相径庭。这里就以R包clusterProfiler的方法为例（该包基于超几何分布的原理），展示如何基于给定的差异基因列表进行GO、KEGG富集分析。


## 使用clusterProfiler做功能富集分析

使用Bioconductor安装clusterProfiler


```r
BiocManager::install('clusterProfiler')
```

详细的用法请参考[Overview of enrichment analysis using clusterProfiler](https://yulab-smu.top/biomedical-knowledge-mining-book/enrichment-overview.html) 以及 [使用R包clusterProfiler进行GO/KEGG富集分析（有参/无参）](https://zhuanlan.zhihu.com/p/561522453).

