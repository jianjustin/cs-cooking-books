# 数学实验 - Matlab基础语法

## Matlab数组

* 数组初始化

  > a = [1,2,3,4];

  > a = 1:1:4;

## Matlab矩阵

* 矩阵初始化

  > a = [1,2,3;4,5,6];

  > a = zeros(m,n);

* 矩阵操作

## Matlab控制流程语句

* 选择流程

  ```
  if 表达式1
     执行语句1
  else if 表达式2
     执行语句2
  else
     执行语句3
  end
  ```

* 循环流程

  ```
  for 循环变量 = 初值：步长：终值
      循环语句；
  end
  ```

  ```
  while 条件表达式
    循环语句体
  end
  ```

  

## 绘图

基础绘图命令包括`plot`，`ezplot`，`fplot`三种 

* plot
* ezplot
* fplot

#### 多图绘制

​     通过`sbuplot`函数将窗口进行分割，再进行绘图

```
subplot(2,2,1);
plot((x,y),title('sin(x)'));
```

#### 三维图形绘制

* 三维曲线绘制`plot3`

  > plot3(x,y,z,'s');

* 三维曲面绘制(`mesh`)

  > mesh(x,y,z);

