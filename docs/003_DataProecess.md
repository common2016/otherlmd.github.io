
# 数据处理 {#DataProcess}
## 一句话Tips
- 因子操作

```r
# 使用字符串有两个缺陷：第一，不在因子水平范围内的不会转化成NA
# 第二，仅按字母排序。
# 因此，通过设定因子水平，可以解决上述两个问题。注意水平和字符串是一样的，
# 只是相当于设定了范围和排序。
	factor(c('Dec','Apr','Jam','Mar'), levels = ('Jan','Feb','Mar','Apr','May'))
# 因子重编码, 把1改成unmarried等
farcats::fct_recode(rawdata$marrige,
           'unmarried'='1','married'='2','cohabitation'='3','divore'='4','wid'='5')
```

- `dbplyr`可以连接到几乎任何数据库。
- `wbstats`下载世界银行数据，很牛逼。Stata里面的`wbopendata`包更牛逼。
- `stationaRy`:一个从NOAA上下载气象数据，如气温，风向等的包。该包就三个函数，一个用来得到站点id，一个用这个id下载数据，还有一个是如果你想得到其他额外的气象数据时可能有用。
- **当你发现你用`save`命令保存一个数据长达数分钟时**，建议你迅速调用`qs`包，可能一分钟不到就帮你快速读入和保存了。但这个包一次只能保存一个变量。
- `tor`: 提供允许用户同时导入多个文件的功能.
- 读入excel中的sheet名：`openxlsx::getSheetNames(file)`
- `XLConect`处理excel最强大的包。但需要JRE（java run enviornment）。

```r
# 可以不改变原有数据，然后把一个数据框精准地写入某个地方
writeWorksheetToFile("XLConnectExample2.xlsx", data = ChickWeight,
 sheet = "chickSheet", startRow = 3, startCol = 4,header = FALSE, clearSheets = FALSE)
```


- 使用`as.Date`来生成日期，必须带有年月日三个要素，使用`format`来输出日期格式，此时可以只输出年和月。如`as.Date('2010/05/01') %>% format(.,format = '%Y%m')`
- `seq.Date()`生成日期序列，包括日、星期、月、年。
- `readstata13`包可以读入更高版本的stata数据格式。
- `zoo::rollapply(x, 30, mean)`就是30天的移动平均求值。

- `select`是一个很牛逼的函数

```r
select(regdata,id, year) # 选择regdata数据框的id和year两列
select(regdata,starts_with('abc')) # 匹配以'abc'开头的列
select(regdata,ends_with('abc')) # 匹配以'abc'结尾的列
select(regdata,contains('abc')) # 匹配包含'abc'的列
select(regdata,matches('abc')) # 正则表达匹配
select(regdata,num_range('x',1:3)) # 匹配x1, x2,x3的列
```
- R语言给数组各维数命名

```r
# Create two vectors of different lengths.
vector1 <- c(5,9,3)
vector2 <- c(10,11,12,13,14,15)
column.names <- c("COL1","COL2","COL3")
row.names <- c("ROW1","ROW2","ROW3")
matrix.names <- c("Matrix1","Matrix2")

# Take these vectors as input to the array.
result <- array(c(vector1,vector2),dim = c(3,3,2),dimnames = list(row.names,column.names,
                                                                  matrix.names))
print(result)
```

-  `pdftools`包的函数可以读PDF文件：

```r
pdf_info(pdf, opw = "", upw = "")

pdf_text(pdf, opw = "", upw = "")

pdf_data(pdf, opw = "", upw = "")

pdf_fonts(pdf, opw = "", upw = "")

pdf_attachments(pdf, opw = "", upw = "")

pdf_toc(pdf, opw = "", upw = "")

pdf_pagesize(pdf, opw = "", upw = "")
```


同时，利用`qpdf`包的`pdf_subset,pdf_combine,pdf_split`可以提取PDF的部分内容，合并PDF文件，把每一页分成一个PDF文件。

## `RJSDMX`包下载世界各大数据库数据

一般工作流：

```r
library(RJSDMX)
# 查看有哪些库可以用
getProviders()
# 库中有哪些子库可以用
getFlows('WITS')
# 该子库调取数据需要哪几个字段
getDimensions('WITS','WBG_WITS,DF_WITS_TradeStats_Tariff,1.0')
# 查看这个指标有几个选项 
getCodes('WITS','WBG_WITS,DF_WITS_TradeStats_Tariff,1.0','INDICATOR')
# 查好了就可以下载
ans <- getTimeSeries('WITS', 'DF_WITS_TradeStats_Tariff/A.CHN.WLD.01-05_Animal.MFN-WGHTD-AVRG')
# 你也可以调用图形窗口查阅命令
sdmxHelp()
```

`IMF2`里面的`IFS`数据库里面有很多季度的宏观数据，如GDP，固定资本形成等

## 前向、后向、线性和样条插值
- `zoo`包
  - `zoo::na.locf`缺省设置可以前向插，即缺失值等于前面的值。当将该函数的`fromLast`参数设为真时，即为后向插。
  - `zoo:na.approx`可以线性插值但不能外推；`na.spline`可以样条插值；

- `imputeTS`包,`imputeTS::na.locf`也可以，不过它只能对数值。它也有后向插值选项。

### `signal`包
它有一个插值函数`interp1`函数，比较好用：

```r
interp1(x, y, xi, method = c("linear", "nearest", "pchip", "cubic", "spline"), 
        extrap = NA, ...)
```
它的参数说明如下

- x,y：vectors giving the coordinates of the points to be interpolated. x is assumed to be strictly monotonic.
- xi：points at which to interpolate.

method	：
one of "linear", "nearest", "pchip", "cubic", "spline".

- 'nearest': return nearest neighbour
- 'linear': linear interpolation from nearest neighbours
- 'pchip': piecewise cubic hermite interpolating polynomial
- 'cubic': cubic interpolation from four nearest neighbours
- 'spline': cubic spline interpolation–smooth first and second derivatives throughout the curve. for method='spline', additional arguments passed to splinefun.
Details

- extrap：
if TRUE or 'extrap', then extrapolate values beyond the endpoints. If extrap is a number, replace values beyond the endpoints with that number (defaults to NA).
