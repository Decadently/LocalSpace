## Markdown语法

==（1）标题==

# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

==（2）字体==

**加粗**

*斜体*

***斜体加粗***

~~删除线~~

==高亮==

我是<sup>上标</sup>

我是<sub>下标</sub>

==（3）列表==

-  一二三四五

1. 一二三四五
2. 上山打老虎

==（4）引用==

>一二三四五

> Dorothy followed her through many of the beautiful rooms in her castle.
>
> > The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

==（5）分割线==

-----

==（6）代码==

`我是代码`

```

我是代码框

```

时代[^1]

**时代**     Ctrl+B:加粗
*时代*     Ctrl+I:斜体

[^1]: 年代结构

~~~flow
```flow
st=>start: 第一步
op=>operation: 第二步
cond=>condition: Yes or No?
e=>end

st->op->cond
cond(yes)->e
cond(no)->op
```
~~~

```mermaid
graph LR
   A --> B 
```

```flow
​```flow
start=>start: API请求
cache=>operation: 读取Redis缓存
cached=>condition: 是否有缓存？
sendMq=>operation: 发送MQ，后台服务更新缓存
info=>operation: 读取信息
setCache=>operation: 保存缓存
end=>end: 返回信息

start->cache->cached
cached(yes)->sendMq
cached(no)->info
info->setCache
setCache->end
sendMq->end
```

```flow
​```flow
start=>start: 接收到消息
info=>operation: 读取信息
setCache=>operation: 更新缓存
end=>end: 处理结束

start->info->setCache->end
```

## LaTeX语法

输入$$再回车  
**方程**  
```
$$
x^2 + 2x + 5 + \sqrt x = 0  
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;x^2&space;&plus;&space;2x&space;&plus;&space;5&space;&plus;&space;\sqrt&space;x&space;=&space;0" title="x^2 + 2x + 5 + \sqrt x = 0" />  

**方程组**

```
$$  
\begin{cases}  
x+y+z=10\\  
x+2y+3z=20\\  
x+4y+5z=30  
\end{cases}  
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;\begin{cases}&space;x&plus;y&plus;z=10\\&space;x&plus;2y&plus;3z=20\\&space;x&plus;4y&plus;5z=30&space;\end{cases}" title="\begin{cases} x+y+z=10\\ x+2y+3z=20\\ x+4y+5z=30 \end{cases}" />  

**求和**    

```
$$  
\sum_{i=1}^{n}
 \sum_{i=1}^nx_i
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;\sum_{i=1}^{n}&space;\sum_{i=1}^nx_i" title="\sum_{i=1}^{n} \sum_{i=1}^nx_i" />

**连乘**  

```
$$  
\prod\limits_{i=1}^n
$$  
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;\prod\limits_{i=1}^n" title="\prod\limits_{i=1}^n" />

**分数**  

```
$$  
\frac 1 5,
\frac{x+1}{x^2}
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;\frac&space;1&space;5,&space;\frac{x&plus;1}{x^2}" title="\frac 1 5, \frac{x+1}{x^2}" />

**乘除法**  

```
$$  
5 \times 6 = 30\\
5 \cdot 6 = 30\\
30 \div 6 = 5
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;5&space;\times&space;6&space;=&space;30\\&space;5&space;\cdot&space;6&space;=&space;30\\&space;30&space;\div&space;6&space;=&space;5" title="5 \times 6 = 30\\ 5 \cdot 6 = 30\\ 30 \div 6 = 5" />

**函数**  

```
$$  
y=
\begin{cases}
x^2, & x>0,\\
x^2 +x-8, & x \le 0
\end{cases}
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;y=&space;\begin{cases}&space;x^2,&space;&&space;x>0,\\&space;x^2&space;&plus;x-8,&space;&&space;x&space;\le&space;0&space;\end{cases}" title="y= \begin{cases} x^2, & x>0,\\ x^2 +x-8, & x \le 0 \end{cases}" />

**矩阵**  

```
$$  
\left(
\begin{array}
{ccc}
1 & 2 & 3\\
4 & 5 & 6\\
7 & 8 & 9\\
\end{array}
\right)
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;\left(&space;\begin{array}&space;{ccc}&space;1&space;&&space;2&space;&&space;3\\&space;4&space;&&space;5&space;&&space;6\\&space;7&space;&&space;8&space;&&space;9\\&space;\end{array}&space;\right)" title="\left( \begin{array} {ccc} 1 & 2 & 3\\ 4 & 5 & 6\\ 7 & 8 & 9\\ \end{array} \right)" />

**化学方程式**  

```
$$
\ce{2Mg + O2 ->[燃烧] 2 MgO}
$$
```

### 著名公式

**欧拉等式**    
```
$$
e^{i\pi} + 1 = 0
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;e^{i\pi}&space;&plus;&space;1&space;=&space;0" title="e^{i\pi} + 1 = 0" />  

**质能守恒公式**    

```
$$  
E = mc^2  
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;E&space;=&space;mc^2" title="E = mc^2" />  

**万有引力公式**    

```
$$
F=G \frac {m_{1}m_{2}}{R^{2}}  
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;F=G&space;\frac&space;{m_{1}m_{2}}{R^{2}}" title="F=G \frac {m_{1}m_{2}}{R^{2}}" />  

**麦克斯韦方程组**    

```
$$    
\begin{array}{lll}
\nabla\times E &=& -\;\frac{\partial{B}}{\partial{t}}   
\ \nabla\times H &=& \frac{\partial{D}}{\partial{t}}+J   
\ \nabla\cdot D &=& \rho
\ \nabla\cdot B &=& 0
\ \end{array}    
$$      
```

**薛定谔方程**    
```
$$  
i\hbar\frac{\partial \psi}{\partial t} = \frac{-\hbar^2}{2m} \left(\frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2}+\frac{\partial^2}{\partial z^2} \right) \psi + V \psi  
$$
```
<img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\bg_black&space;i\hbar\frac{\partial&space;\psi}{\partial&space;t}&space;=&space;\frac{-\hbar^2}{2m}&space;\left(\frac{\partial^2}{\partial&space;x^2}&space;&plus;&space;\frac{\partial^2}{\partial&space;y^2}&plus;\frac{\partial^2}{\partial&space;z^2}&space;\right)&space;\psi&space;&plus;&space;V&space;\psi" title="i\hbar\frac{\partial \psi}{\partial t} = \frac{-\hbar^2}{2m} \left(\frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2}+\frac{\partial^2}{\partial z^2} \right) \psi + V \psi" />
