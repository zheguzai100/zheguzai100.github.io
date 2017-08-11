\`\`\`\`

数据集获取
----------

    library(dplyr)
    satis_deer3 %>%
      group_by(type) %>%
      select(type,ShiYingXing:allowance) %>%
      summarise_all(funs(mean)) -> satisdt1

ggradar需要的数据格式为:

    knitr::kable(satisdt1[,1:8])

<table>
<thead>
<tr class="header">
<th align="left">type</th>
<th align="right">ShiYingXing</th>
<th align="right">XingNeng</th>
<th align="right">price</th>
<th align="right">BianJieXing</th>
<th align="right">ZhiLiang</th>
<th align="right">DongLi</th>
<th align="right">Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">bought</td>
<td align="right">4.122003</td>
<td align="right">4.109107</td>
<td align="right">4.139528</td>
<td align="right">4.122003</td>
<td align="right">4.109107</td>
<td align="right">4.139528</td>
<td align="right">4.122003</td>
</tr>
<tr class="even">
<td align="left">considered</td>
<td align="right">4.109266</td>
<td align="right">4.108875</td>
<td align="right">4.113998</td>
<td align="right">4.109266</td>
<td align="right">4.108875</td>
<td align="right">4.113998</td>
<td align="right">4.109266</td>
</tr>
<tr class="odd">
<td align="left">no_interest</td>
<td align="right">4.126035</td>
<td align="right">4.125736</td>
<td align="right">4.107293</td>
<td align="right">4.126035</td>
<td align="right">4.125736</td>
<td align="right">4.107293</td>
<td align="right">4.126035</td>
</tr>
</tbody>
</table>

数据归一化\[0,1\]
-----------------

使用ggradar作图很关键的一步就是 画图前需要按照公式:

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
$$ \\frac{x-min{x}}{max{x}-min{x}} $$

进行\[0,1\]范围的归一化处理

*在ggradar的函数代码中有对value范围的判断,超出\[0,1\]会报错*
在实际运用过程中可以通过以下语句进行 规范化:

    library(dplyr)
    satisdt1 %>% 
    mutate_at(vars(-type),funs(scales::rescale)) %>%
    select(1:8) %>%
    knitr::kable()

<table>
<thead>
<tr class="header">
<th align="left">type</th>
<th align="right">ShiYingXing</th>
<th align="right">XingNeng</th>
<th align="right">price</th>
<th align="right">BianJieXing</th>
<th align="right">ZhiLiang</th>
<th align="right">DongLi</th>
<th align="right">Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">bought</td>
<td align="right">0.7595444</td>
<td align="right">0.0137548</td>
<td align="right">1.0000000</td>
<td align="right">0.7595444</td>
<td align="right">0.0137548</td>
<td align="right">1.0000000</td>
<td align="right">0.7595444</td>
</tr>
<tr class="even">
<td align="left">considered</td>
<td align="right">0.0000000</td>
<td align="right">0.0000000</td>
<td align="right">0.2080009</td>
<td align="right">0.0000000</td>
<td align="right">0.0000000</td>
<td align="right">0.2080009</td>
<td align="right">0.0000000</td>
</tr>
<tr class="odd">
<td align="left">no_interest</td>
<td align="right">1.0000000</td>
<td align="right">1.0000000</td>
<td align="right">0.0000000</td>
<td align="right">1.0000000</td>
<td align="right">1.0000000</td>
<td align="right">0.0000000</td>
<td align="right">1.0000000</td>
</tr>
</tbody>
</table>

作图
----

作图时可以根据feather多少,图像展示的清晰度来设定相应的参数已达到最佳效果

    library(dplyr)
    library(ggradar)
    satisdt1 %>% mutate_at(vars(-type),funs(scales::rescale))%>%
      ggradar(axis.label.size = 4, # 变量名文本大小
              font.radar='Microsoft YaHei UI', #字体设置
              group.line.width = 0.8, # 数据点间连线的粗细
              group.point.size = 2, # 数据点的大小
              grid.label.size = 5, # (0%,50%,100%)文本大小 
              legend.text.size =7, # legend 文本大小
              plot.title = 'Satis Analysis')+ # 标题
      theme(legend.position = 'bottom') # 改变legend位置,可以为'left/right/top/bottom'

![](gitblog_files/figure-markdown_strict/radarmap-1.png)
