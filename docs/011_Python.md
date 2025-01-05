
# python {#Python}

## 利用Python进行局域网文件传输

1. 问kimi如何查询你要发送文件的电脑的IPv4网络地址，通常是`192.168.*.*`之类的
2. 在发送文件的电脑终端用`cd`命令先把工作目录定位到你要发送文件的目录下，然后使用
```
python3 -m http.server
```
命令把文件共享出来,并且可以看到服务器的端口，往往是8000.

3. 在接受文件的电脑浏览器输入网址：`http://192.168.*.*:8000`，也就是`http://ip:端口`。就可以看到发送文件电脑共享出来的文件，直接下载它即可。

## VSCode下配置python

最近发现用vscode写python也不错，记录几个要点：

- 在应用拓展搜“python”，通过微软认证那个就是，一般就是第一个。
- 通过`command+shift+P`调出命令`Python: Select interpreter`，如果你以前用的是conda的python，你可以选择这个解释器，目录不出意外应该在`~/opt/anaconda3/bin/python`，这样，你以前安装的包都能调用。

## Spyder下配置python

- Spyder有官网可以直接下载，不需要Anaconda来集成。下载时它会附带把python也给你默认装上使用。
- 你可以通过“偏好”设置来改变python解释器，即不用它的默认解释器。
- F9等价于RStudio的`ctl + Enter`.
- 在Spyder的控制台中安装包，可以使用`conda install <包名>`命令而不能使用`pip`，你可以通过该命令先安装`pip`，然后再用`pip`安装包。不过现在新版本的python都自带`pip`了。

### spyder下安装pip

1. 去pypi官网下载pip的源文件, 后缀名是.whl
2. 把.whl改成.zip，然后解压缩可以看到两个文件，将其复制到sypder安装路径下，如`D:\spyder6\envs\spyder-runtime\Lib\site-packages`。
3. 重启spyder, 然后在控制台使用pip命令即可，脚本里面无法使用。
