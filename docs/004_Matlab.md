

# Matlab {#Matlab}
## 一句话Tips
- 在Editor中各个窗口之间的切换 
  - 快捷键ctrl+pageup和ctrl+pagedown之间可以实现在editor中所打开文件之间的切换
- 在IDE中各个窗口之间的切换(该快捷方式不可更改，Mac中ctrl为cmd) 
  - ctrl+Tab 实现IDE中的各个窗口轮流转换
  - ctrl+PageUp/PageDown在子窗口的各标签切换
  - ctrl+0(或者1、2、3、4)实现转换到控制台(或其他特定窗口 )
  - ctrl+shift+0切换到editor窗口

- ctrl + R/T：注释/取消注释
- 杀死excel进程：system('tskill excel')
- F9运行当前行
- 当使用一个函数，输出结果有两个。但第一个结果不需要，只需要第二个时，第一个结果可用`~`代替。如`[~,area] = xlsread('malm.xlsx',1,'b2:aa31');`
- 当使用一个函数，输入需要跳跃时，可用`[]`代替。如
```matlab
[x, fval] = fmincon(@loglik, para0,[],[],[],[],...
  [0.0001,0.0001,0.0001, -1, 0.00001*ones(1,ncol)],[ones(1,length(para0))],...
  [],[],Y, tau, nrow, ncol);
```

## 带名称和缺省值的参数设置

在Matlab2019以后，函数参数的设置有了更多功能，比如可以验证参数的类型（整数、双精度？）、维数，同时提供了带名称的参数设置。这需要在代码块中加入一个模块。比如`myFunction`函数第一个参数是`X`，这个就像通常的函数一样按位置确定，剩下的参数按名称来匹配参数，那么要把按名称匹配的参数都放在一个结构体中，而且这样的参数也可以赋予默认值，这就和R等函数非常类似了。具体如下，

```matlab
function myRectangle(X,Y,options)
    arguments
       X double
       Y double
       options.LineStyle (1,1) string = "-"  % 括号是维数约定，可以省略，string是类型约定，可以省略
       options.LineWidth (1,1) {mustBeNumeric} = 1 % 花括号是验证函数，可以省略
    end
    % Function code
    ...
end
```

该函数可以如下调用：
```matlab
myRectangle(4,5)
myRectangle(4,5,LineStyle=":",LineWidth=2)
myRectangle(4,5,LineWidth=2,LineStyle=":")
myRectangle(4,5,LineStyle=":")
myRectangle(4,5,LineWidth=2)
```


## 窗口输出到文本

  1.把窗口输出写入指定的文件，之后会一直记录窗口输出信息，直到你关闭；
```matlab
diary('testlog.txt');
diary on;
```
  2.关闭日志记录
```matlab
diary off;
```
  3.删除日志
```matlab
diary off;%先关闭
delete('testlog.txt');
```

## GVAR Toolbox使用报告
### 数据输入
- E-G列中，国家排序会影响后面的估计，结果的呈现都是以该顺序排列的。如果你要用程序内建的加权矩阵，那么G列的国家码需要填充。
- 在各变量数据存放的sheet中，sheet名要按照short name来命名，虽然顺序不必一致，但推荐一致。
- 变量数据的sheet中，第一列按`yyyyMmm`形式标识月度数据，`yyyyQq`的形式标识季度数据。特别注意此时国家的名字都是short name命名，而且顺序和main中的国家顺序精确一致。
- 注意，缺失值一定要用123456789替代，不要留空。

### 模型设定
- 在模型设定中，要注意，空的意味着数据不可行，而不要设置成0.
- 有个时候要注意协整rank不能过大，比如3个变量设成4个rank，会报错。

### 一些坑
1.国内变量那些相应的权重矩阵要填满，否则会报错。如都填好`wmat1`。

2.`GVAR Toolbox`主导单元是按全局变量设置的，因此，如果你要将美国作为主导单元，应该将美国的变量单独提出来作为全局变量。（对否？）
