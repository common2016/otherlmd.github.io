
# 其他小众软件 {#Other}

## PDF文件压缩

github上有个项目，windows下压缩PDF

项目地址：https://github.com/pgodwin/PdfShrinker

- 文件名不要有中文

## Eviews编程
- 单引号`'`意味着评论。
- 一行写不完，用下划线表示下一行接着来`_`，就像matlab里面的`...`。
- 

## GAUSS

- 可以使用学生证申请它的light版，(网址)[https://www.aptech.com/industry-solutions/gauss-in-education/students/]
- F4类似于R里面的`ctrl+enter`，如果选中则运行选中行，没选中则运行当前行。
- GAUSS在函数中，如果没有使用类似`local x y`方式定义的变量，都是全局变量，会在函数中修改函数外的值。
- `include myfile.src`将`myfile.src`文件中的内容插入到当前脚本中。但是该命令不能在控制台执行，而只能嵌入在脚本中。要在控制台中达到相同的效果应该使用`run myfile.src`。

### 系统函数

- `cdir(0)`返回当前工作目录
- `chdir E:\17_HuaDong\learn\HM2009\PythonCode\ch9_code_2nd_gauss;`修改当前工作目录


## NoteExpress样式设置

- 参考文献中文在前，英文在后，首先把文献的字段“语言”凡是英文的改为"English"，然后排序的时候先按语言排即可。
- 对于空格的处理，以`*`号连接，如
```
作者.标题[J].期刊,年份,卷|(期)|:*页码|.
```
- 一般情况上述格式中的标点符合与前面一个字段关联，如`年份`后面的逗号与`年份`同时出现，同时消失。如果想要标点符号和后面的字段关联，则要使用`|`符号。

## FileZilla
这个文件传输文件不是一般地快。

官网下载地址：https://www.filezilla.cn/

几个问题的处理：

- 很多时候直接就是连不上。做两个动作：
  - 改客户端的文件传输为FTP，下面那个框框的选项是明文传输；
  - 服务端的电脑，更改防火墙设置，把21端口开放。[具体见这里](https://blog.csdn.net/InnovationAD/article/details/84944238?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1db1882b-159d-4705-865f-78d855fe1f50&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

- 如果报错：425 Can't open data connection。还是要更改服务端电脑的防火墙设置，[具体见这里](https://blog.csdn.net/weixin_44284051/article/details/106277683)
