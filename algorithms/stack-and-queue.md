# 栈&队列【算法导论】

这个章节我主要会围绕`队列`和`栈`这两个数据结构讨论，为什么我会将这两个数据结构统一进行讲解呢？因为队列和栈不过是数组的衍生，对数组在操作上做了一部分的限制；队列是通过`先入先出`的方式进行限制，而栈则是通过`先入后出`的方式进行限制

## 队列

首先呢，我先把题目引出来，我使用的是leetcode上关于队列的题目，如果大家有兴趣，可以使用leetcode实现自己的代码（[访问链接-https://leetcode-cn.com/problems/design-circular-queue/](https://leetcode-cn.com/problems/design-circular-queue/)）;当然，如果大家觉得麻烦也可以跟着我去了解队列的一下特性

##### 题目描述

> 设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。
> 循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。
> 你的实现应该支持如下操作：
>
> * MyCircularQueue(k): 构造器，设置队列长度为 k 。
> * Front: 从队首获取元素。如果队列为空，返回 -1 。
> * Rear: 获取队尾元素。如果队列为空，返回 -1 。
> * enQueue(value): 向循环队列插入一个元素。如果成功插入则返回真。
> * deQueue(): 从循环队列中删除一个元素。如果成功删除则返回真。
> * isEmpty(): 检查循环队列是否为空。
> * isFull(): 检查循环队列是否已满。

* **队列包含哪些功能？**
  1. 入队列
  2. 出队列
  3. 队列判空/判满（用于判断是否能够操作队列）

* **入队列**

  ```java
  public boolean enQueue(int value) {
      	if(isFull())return false;//判断队列是否满
      	queue_end++;//获取下一元素位置
      	if(queue_end == queue_size)queue_end = 0;
      	if(0 != queue_data[queue_end])return false;
      	queue_data[queue_end] = value;
      	queue_item++;//用于队列判空/判满
      	return true;
      }
  ```


* **出队列**

  ```java
  public boolean deQueue() {
      	if(isEmpty())return false;//判断队列是否空
      	queue_data[queue_start] = 0;
      	queue_start++;//获取下一元素位置
      	if(queue_start == queue_size)queue_start = 0;
      	queue_item--;//用于队列判空/判满
      	return true;
      }
  ```


> 给大家留一个问题去思考吧（来源于leetcode网站-问题编号为862）

![栈-困难-问题](/Users/jian/blog/source/assets/img/picture/栈-困难-问题.png)



## 栈

栈则是一个先入后出的特殊数组，通过栈顶指针去实现特殊化操作；同样地，我会通过一个题目去大致解释栈的操作

>设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
>
>* push(x) -- 将元素 x 推入栈中。
>* pop() -- 删除栈顶的元素。
>* top() -- 获取栈顶元素。
>* getMin() -- 检索栈中的最小元素。



* 入栈

  ```java
  public void push(int x) {
          if(isFull())resize();
          stack_data[++stack_top] = x;
          this.stack_deep++;
      }
  ```

* 出栈

  ```java
  public void pop() {
      	if(isEmpty())return;
      	stack_data[stack_top--] = 0;
          this.stack_deep--;
      }
  ```

* 获取栈顶元素

  ```java
  public int top() {
      	if(isEmpty())return -1;
  		return stack_data[stack_top];
      }
  ```

* 获取最小元素

  ```java
  public int getMin() {
      	int m = stack_top,n = stack_deep;
          int min = -1,k = 0;
      	while(n>0) {
      		if(k == 0)min = stack_data[m];
      		if(stack_data[m] < min)min = stack_data[m];
      		m--;
      		n--;
      		k++;
      	}
  		return min;
      }
  ```

* 栈扩容

  ```java
  public void resize() {
      	int[] stack_new_data = new int[stack_size*2];
      	for (int i = 0; i < stack_data.length; i++) {
      		stack_new_data[i] = stack_data[i];
  		}
      	this.stack_size = stack_size*2;
      	stack_data = stack_new_data;
      }
  ```




