

# canvas学习笔记

1.两件事情： canvas学习、水桶效应画图、tree.js画水桶

### canvas.getContext('2d')

可以得到一个2d渲染上下文对画布进行渲染

### canvas可操作的图形

#### 一、矩形操作

`const ctx = canvas.getContext()` 会返回一个渲染上下文，这个实例会有对应方法

```javascript
ctx.fillRect(x1, y1, x2, y2) 	// 填充矩形
ctx.strokeRect(x1, y1, x2, y2) 	// 描绘矩形边框
ctx.clearRect(x1, y1, x2, y2) 	// 消除矩形区域，让清除部分完成透明
rect(x, y, width, height) // 矩形
```



### 二、绘制路径

图形的基本形状是路径。路径是通过不同颜色和宽度的线段或者曲线相连形成的不同形状点的集合。

绘制路径需要的步骤： 

1. 第一步创建路径其实节点
2. 使用画线命令画出命令
3. 路径封闭
4. 形成路径通过描边填充形成图形

```javascript
ctx.beginPath()		// 开始
ctx.moveTo(x,y) 	// 创建路径其实节点
ctx.stroke(x,y) 	// 通过线条描绘图形轮廓
ctx.fill() 			// 填充路径的你内容区域生成的实心图形
ctx.moveTo(x,y)		// 笔触开始，这个函数不能画出任何东西，也是路径列表的一部分，将笔触移动到指定的坐标，使用biginPath()之后你要设置起点
ctx.arc(x,y, radius,startAngle，endAngle, 方向 )
ctx.arc(75, 75, 25, 0, Math.PI, false); // 用来绘制圆 75，75为圆心 25的半径，其实角度为0，旋转半圆，顺时针
arcTo(x1, y1, x2, y2, radius)
ctx.lintTo(x,y)		// 绘制一条从当前位置到x,y的直线

```

![弧/曲线](https://imgconvert.csdnimg.cn/aHR0cDovL3d3dy53M3NjaG9vbC5jb20uY24vaS9hcmMuZ2lm)



### 贝塞尔曲线

```
quadraticCurveTo(cp1x, cp1y, x, y)  			// 二次贝塞尔曲线
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)  	// 三次贝塞尔曲线
```

​										

### path2D()

用来缓存或记录绘画命令，这样你将能快速地回顾路径。



### 添加样式和颜色

```javascript
fillStyle = color // 填充样式
strokeStyle = color // 描边样式
globalAlpha = number // 透明度
```



### 线相关样式

```
lineWidth = number // 以1.0为最小单位，以点为线为中心向两边拓展
lineCap = butt | square | round // 线端点的样式
lineJoin = round | bevel | miter // 线的连接处样式

```



### 绘制文本

```
fillText('string',x ,y[,maxWidth])
strokeText()
font // 字的大小
```

