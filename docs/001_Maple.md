


# Maple {#Maple}

## 快捷键

- `ctrl+del`删除全行
- `ctrl+K,ctrl+J`在前或后插入一行
- `?plot`看帮助
- `with(plottools)`载入包

## 几个基本的代数处理

<!-- 通常`simplify,combine,expand,convert`进行化简 -->
- 复合函数$f(g(x))$可以写成，`(evalf@arccos)(2/3)`，这就对反余弦进行了数值评价
- `factor,expand,simplify，collect`会进行因式分解，多项式展开，化简，合并同类项。带有附加关系的化简,`simplify(expr, {x*z=1})`

## 产生序列

中括号产生list，它是有序的。大括号产生集合，它是无序的。下面的代码可以产生一系列值。
```
L:= [a1,a2,a3]:
map(f, L)
# [f(a1),f(a2),f(a3)]
```
可以使用`L[i..j]`来选择集合中的元素，也可以使用`op(i..j,L)`来选择多个元素。当然，使用`seq`函数也能产生一系列值。
```
[seq(f(n),n=n1..n2)]
```
## 函数定义

- 简单函数定义：`f:=x->(1+x)^(1/x)`
- 复杂函数定义：
```
f:=proc(x)
(1+x)^(1/x)
end proc:
```

### 参数的宣称

- 如下写法可以按位置给参数赋值
```
f := proc(a::integer := 10, b::expects(integer) := 100.1) a + b end proc:
```

- 也可以按参数名称赋值，但写法略有不同
```
f := proc( { simple::integer := 2 } )
    sprintf("simple=%d",simple)
end proc:
```

## 控制流

- 条件语句：
```
if ...then...
elif...then...
else ... 
end if:
```

## 画图

- 2D图：`plot(sin(x), x=-Pi..2*Pi);`
- 3D图：`plot3d(f(x,y),x=a..b,y=c..d)`
- 等高线图：`plots[contourplot](f(x,y),x=a..b,y=c..d)`
- 多图叠在一张图显示：`plots[display](p1,p2,p3)`

## 矩阵

- 通常要载入包`LinearAlgebra`和`linalg`。
- 产生脚标为$a_{i,j}$的矩阵$A$。
```
A:=array([a[1,1],a[1,2]],[a[2,1], a[2,2]])
A:=Matrix([a[1,1],a[1,2]],[a[2,1], a[2,2]])
A:=Matrix(2,2,symbol = a)
A:=Vector(2, symbol = a)
```

- 矩阵计算
```
with(LinearAlgebra):
evalm(A^2)
A.B # 矩阵相乘
```


## 一句话Tips

- 语句可以清除原来的`f`的定义，
- 多项式降幂排列`sort(x^3*y-y^3*x,[y,x])`
- 乘号记得输入`*`，各种数学运算：
- 求导：`diff(f,x)`; 求二阶导数，`diff(f,x$2)`
- `nops`返回比如向量的元素的数目：
```
u:=[1,4,9]:
nops(u);
```
- 求具体的数值，用`evalf(Pi)`
- 矩阵相乘用`.`
- 它的`surfdata`以点的数值的形式绘制三维曲线，相对比较好看。
- 用`y`替换表达式中的`x`：`subs(x=y, expression)`
