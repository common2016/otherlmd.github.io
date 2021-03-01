
# Stata {#Stata}
## 数据处理
1. 生成观测的脚标，即1:nrow(x): `gen yr = _n // ` 
2. 回归完成，使用`predict`可以得到拟合值，残差等序列。如`predict yres, resid`则得到名为`yres`的残差。
3. `matrix list A`: 显示矩阵
4. `outfile v1 v2 v3 using mydata.csv, comma` 输出逗号分割的csv文件。
5. 
6. 季节调整，外部命令`sax12`
7. 从变量名`income`生成一个新的变量名如`income_y`，则可以如下操作：
```stata
local xvar income
local yvar `xvar'_y
```
此时的`yvar`就是`income_y`。

## 回归相关

- `#`是所有分类交互，`##`是仅仅两个分类交互。
- 获得残差。估计完以后，使用命令：`predict e, r //e是生成的残差变量名，r表明生成残差`。
- 将回归后的系数存入数据集中，然后即可导出。
```
mat b = e(b)'
svmat double b, n(beta)
```

- 滞后、超前、差分变量。`L.x L2.x F.x F2.x D.x D2.x`
- 字符串转因子变量：`encode str_var, g(newvar)`
- 恩格尔格兰杰两步法：`egranger`是外部命令。
- 滚动估计：`_b,_se`意味着保存回归系数和标准误，200的窗口样本，原来数据集的变量合并到新的保存的数据集betas中去。
```stata
rolling _b _se, window(200) saving(betas, replace) keep(date):regress ibmadj spxadj
```
### 描述性统计和回归结果导出

- 一个例子：
```stata
ssc install sum2docx
sum2docx pay stock fund finance income age edu sex hukou marriage bond ///
insurance deposit income depositDum RiskDum InsDum using  pm.docx, replace ///
        stats(N mean(%9.2f) sd min(%9.0g) ///
        median(%9.0g) max(%9.0g))  ///
        title("表 1: 描述性统计")
ssc install reg2docx
probit depositDum pay income age edu sex hukou marriage
est store p1
probit RiskDum pay income age edu sex hukou marriage
est store p2
probit InsDum pay income age edu sex hukou marriage
est store p3
reg2docx p1 p2 p3 using pm.docx, append title("表2: Probit回归结果") scalars(r2_p N)
```
- 另一个例子：输出回归结果到csv文件
```stata
reg y x
est store m1
esttab m1 m2 m3 using rlt.csv, replace //输出回归结果
esttab e(b) using rlt.csv, replace  //输出回归系数
```

## 一些外部命令
- `diff`可以做DID，PSM-DID，分位DID
- `xtnptimevar`：非参时变面板固定效应系数估计。方法是Li, Chen and Gao (2011)。
- `xtfixedcoeftvcu`：Hsiao(2014)的书上变系数中的固定系数估计命令。
- `xtsemipar`: Baltagi and Li (2002)的半参部分线性固定效应面板估计。代码缺陷在于非参数只能是一个变量。
- `xthreg`，Hansen(1999)静态面板阈值回归。

## stata对某个变量随机抽样
**问题** 回归以后，如何对残差进行自助抽样？stata里面对某个变量随机抽样会比较麻烦一些。它有一个函数`sample`, 但是它是对整个数据集抽样。要对某个变量抽样，需要曲线救国。

解决的思路：

1. 首先保存原来的数据集；
2. 然后删掉其他变量，仅保留残差；
3. `sample`残差，然后`merge`刚刚保存的数据集。这就得到了随机抽样后的残差。

代码
```stata
cd E:\17_HuaDong\reserch\GVAR\RCode\GVAR_UsingR2e
use regdata, replace
gen time = _n
tsset time
threshold y, threshvar(thV) regionvars(V*) trim(20) nthresholds(1)
predict double eps, residuals
save GVAR, replace
keep eps
bsample 100
merge 1:1 _n using GVAR
```
## Stata调用R
- 安装rsource包
```stata
ssc install rsource
```
- 直接在do文件中写R代码
```stata
//调用R软件 END_OF_R是设定一个标志，表示用这个表示结束
rsource, terminator(END_OF_R) rpath(D:\Program Files\R-3.5.0\bin\R.exe) ///
roptions(--vanilla)  // 修改路径
rm(list = ls())
library(foreign)
library(tidyverse)
a318 <- read.dta('E:/25_PartTimeJob/1810318/a318.dta') # 修改路径,记住，是反斜杠
a318 <- as.tibble(a318)
library(PSTR)
regdata <- NewPSTR(a318,dep = 'ev',indep = c('cs','as','p','es'),
                   indep_k = c('cs','as','p','es'),tvars = 'go',iT = nrow(a318),
				   im = 1)
pstr <- LinTest(regdata)
print(pstr,'tests')
rlt <- EstPSTR(regdata,im=1,iq=1)
print(rlt,'estimates')
END_OF_R
```
- 在do 文件中调用R代码
```stata
 rsource using nitz2.R, rpath(D:\Program Files\R-3.5.0\bin\R.exe) roptions(--vanilla)
 ```
## Miscellaneous
- `wbopendata`包的使用
```stata
ssc install wbopendata //下载世界银行数据
db wbopendata //一个GUI下载界面
```
- `icio`投入产出分析，koopman et al. (2014)的方法
- stata15有一个阈值非线性估计命令threshold非常好用：通过AIC，SC和HQIC来选择阈值的个数，最小的就是最合适的。
- `egi`：计算EG指数。
- stata中文乱码问题

第一步，先用命令`unicode analyze Data035.dta` 分析数据集的编码

第二步，用命令`unicode encoding set gb18030`设置编码

第三步，用命令`unicode   translate "Data035.dta", transutf8`转换编码

备注：有时某些变量无法转化，可使用`invalid(mark)`选项。
