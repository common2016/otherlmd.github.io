
# Julia {#Julia}

- Julia的一个IDE就是Atom中的Juno，它已经非常好用了。记录几个点：
  - 快捷键配置，是在一个文件中修改的，见https://blog.csdn.net/uniqsa/article/details/91315714。这个给一个我常用的运行和保存配置书写,注意缩进：
  
```
'.platform-darwin atom-text-editor[data-grammar="source julia"]':
  'ctrl-enter': 'julia-client:run-block'
  'shift-ctrl-enter': 'julia-client:run-all'

'body':
  'ctrl-s': 'core:save'
```
  
- 安装包
```julia
import Pkg;
Pkg.add("DataFrames")
# Pkg.rm()是删除包
using DataFrames # 导入包
```

- `Pipe`包提供了强大的管道操作`|>`，与R语言中的`.`不同，它使用`_`作为占位符。
```julia
@pipe data |> addX(_[1],_[2]) |> println
```
- `1:5`返回的是一个迭代器，可以用`collect(1:5)`变成一个数组
- `using RCall, Statistics`可以同时导入多个包
- `mean(skipmissing([1, missing, 3]))`求均值忽略缺失值
- 查看模块中所有方法或变量`names(模块)`。通常是去github上看它的案例用法。
- 返回当前工作空间的变量，可以用`names(Main)`或者`varinfo()`
- 函数名字中带‘！’号，该函数不仅返回值，还会更改参数的值。
- 在Julia表达式中，如果要标志某个字符是变量，也就是它要被它的内涵所替代，前面要加美元符号。比如定义一个宏：
```julia
macro sayhello(name)
  :(println("Hello,",$name)) # 注意括号的使用
end
```
- 变量赋值
```julia
a,b,c = 1,'a',3 
```
- 一些函数可以作用于矩阵，如果在其后加个点意味着它作用于矩阵的元素，如
```julia
x1 = randn(6,4)
x2 = round.(x1)
```
- 创建矩阵的方式，和matlab有许多共通之处。拼接也是类似的。
```julia
A = [1 2 3 4;5 6 7 8 9];
A = randn(10,8) # 10行8列的正态分布矩阵
A = ones（6，5）
```
- `include`类似于R的`source`.
- `DataFrames`包


