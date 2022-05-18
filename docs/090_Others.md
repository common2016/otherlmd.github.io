
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
- F4类似于R里面的`ctrl+enter`，如果选中则运行选中行，没选中则运行当前行。运行整个代码使用Ctrl+R。
- GAUSS在函数中，如果没有使用类似`local x y`方式定义的变量，都是全局变量，会在函数中修改函数外的值。
- `#include myfile.src`将`myfile.src`文件中的内容插入到当前脚本中，类似于R语言中的`source()`。但是该命令不能在控制台执行，而只能嵌入在脚本中。要在控制台中达到相同的效果应该使用`run myfile.src`。
- GAUSS也有自己的库，它调用库的命令与R类似，如`library pgraph; `
- `a|b`垂直连接矩阵a和b, `a~b`水平连接矩阵a和b。

### 系统函数

- `cdir(0)`返回当前工作目录
- `chdir E:\17_HuaDong\learn\HM2009\PythonCode\ch9_code_2nd_gauss;`修改当前工作目录

## Zotero

主要看看它的参考文献格式编辑，这个编辑是CSL语言，和html语言在格式上是互通的。这个语言不仅使用Zotero，也是适用EndNote的。先介绍三个工具，

- 这是一个手把手的教程，入门非常合适 https://www.zotero.org/support/dev/citation_styles/style_editing_step-by-step 。
- 这是它的可视化CSL编辑器，可以在里面所见即所得的编辑 https://editor.citationstyles.org/visualEditor/ 。
- 这是关于它的很多细节设置，包括choose这类标签的使用等 https://docs.citationstyles.org/en/stable/specification.html。
- 这上面有很多中文格式，直接下载稍作修改即可使用，包括《经济研究》、《管理世界》等。 https://github.com/redleafnew/Chinese-STD-GB-T-7714-related-csl 。

基本流程就是：
 
- 先找一个和你想要的格式非常类似的已存格式，可在前面第二点的网址中找。
- 找到后，要先改它的样式的Title和ID，否则当你保存了这个样式，去安装的时候，只会覆盖原来的样式，不会产生新的样式。



## NoteExpress样式设置

- 参考文献中文在前，英文在后，首先把文献的字段“语言”凡是英文的改为"English"，然后排序的时候先按语言排即可。
- 对于空格的处理，以`*`号连接，如
```
作者.标题[J].期刊,年份,卷|(期)|:*页码|.
```
- 一般情况上述格式中的标点符合与前面一个字段关联，如`年份`后面的逗号与`年份`同时出现，同时消失。如果想要标点符号和后面的字段关联，则要使用`|`符号。
- 虚拟机安装NoteExpress弹出一个包含乱码的报错窗口，解决办法见 https://www.zhihu.com/question/514427851/answer/2421268390 。
- "无效的类字符串报错"解决办法见 https://jingyan.baidu.com/article/4b52d702755cd0fc5c774bc8.html 。

## FileZilla
这个文件传输文件不是一般地快。

官网下载地址：https://www.filezilla.cn/

几个问题的处理：

- 很多时候直接就是连不上。做两个动作：
  - 改客户端的文件传输为FTP，下面那个框框的选项是明文传输；
  - 服务端的电脑，更改防火墙设置，把21端口开放。[具体见这里](https://blog.csdn.net/InnovationAD/article/details/84944238?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1db1882b-159d-4705-865f-78d855fe1f50&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

- 如果报错：425 Can't open data connection。还是要更改服务端电脑的防火墙设置，[具体见这里](https://blog.csdn.net/weixin_44284051/article/details/106277683)
