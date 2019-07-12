# 树【算法导论】

>这节我主要针对数据结构-树做一些理解和扩展

### 简介

在计算机科学中，**树**是一种抽象数据类型或是实现这种抽象数据类型的数据结构，用来模拟具有树状结构性质的数据集合。它是由n（n>0）个有限节点组成一个具有层次关系的集合

###### 树具有哪些细分子类呢？

* 线段树
* 自平衡二叉查找树
* B树
* 二叉树
  * AA树
  * AVL树
  * 红黑树
  * 平衡树
  * 伸展树
  * 二叉查找树
* 堆
  * 二叉堆
  * 左偏树
  * 二项堆
  * 斐波那契堆
* R树
  * R*树
  * R+树
  * Hilbert R树
* 前缀树
  * 哈希树

后面我主要针对B树，AVL树，红黑树，查找树等的特性做一些深入的了解

### 基本操作

- 构造空树

  ```c
  void ClearTree(PTree *T)
  { 
    T->n = 0;
  }
  ```

- 构造树

  ```c
  void CreateTree(PTree *T)
  { 
    LinkQueue q;
    QElemType p,qq;
    int i=1,j,l;
    char c[MAX_TREE_SIZE]; /* 临时存放孩子节点数组 */
    InitQueue(&q); /* 初始化队列 */
    printf("请输入根节点(字符型，空格为空): ");
    scanf("%c%*c",&T->nodes[0].data); /* 根节点序号为0，%*c吃掉回车符 */
    if(T->nodes[0].data!=Nil) /* 非空树 */
    {
      T->nodes[0].parent=-1 ; /* 根节点无父节点 */
      qq.name=T->nodes[0].data;
      qq.num=0;
      EnQueue(&q,qq); /* 入队此节点 */
      while(i<MAX_TREE_SIZE&&!QueueEmpty(q)) /* 数组未满且队不空 */
      {
        DeQueue(&q,&qq); /* 节点加入队列 */
        printf("请按长幼顺序输入节点%c的所有孩子: ",qq.name);
        gets(c);
        l=strlen(c);
        for(j=0;j<l;j++)
        {
          T->nodes[i].data=c[j];
          T->nodes[i].parent=qq.num;
          p.name=c[j];
          p.num=i;
          EnQueue(&q,p); /* 入队此节点 */
          i++;
        }
      }
      if(i>MAX_TREE_SIZE)
      {
        printf("节点数超过数组容量\n");
        exit(OVERFLOW);
      }
      T->n=i;
    }
    else
      T->n=0;
  }
  ```

- 判断树是否为空

  ```c
  Status TreeEmpty(PTree *T)
  { 
    return T->n==0;
  }
  ```

- 获取树的深度

  ```c
  int TreeDepth(PTree *T)
  { 
    int k,m,def,max=0;
    for(k=0;k<T->n;++k)
    {
      def=1; 
      m=T->nodes[k].parent;
      while(m!=-1)
      {
        m=T->nodes[m].parent;
        def++;
      }
      if(max<def)
        max=def;
    }
    return max; 
  }
  ```

- 获取根节点

  ```c
  TElemType Root(PTree *T)
  { 
    int i;
    for(i=0;i<T->n;i++)
      if(T->nodes[i].parent<0)
        return T->nodes[i].data;
    return Nil;
  }
  ```

- 获取第i个节点的值

  ```c
  TElemType Value(PTree *T,int i)
  { 
    if(i<T->n)
      return T->nodes[i].data;
    else
      return Nil;
  }
  ```

- 改变节点的值

  ```c
  Status Assign(PTree *T,TElemType cur_e,TElemType value)
  { 
    int j;
    for(j=0;j<T->n;j++)
    {
      if(T->nodes[j].data==cur_e)
      {
        T->nodes[j].data=value;
        return OK;
      }
    }
    return ERROR;
  }
  ```

- 获取节点的父节点

  ```c
  TElemType Parent(PTree *T,TElemType cur_e)
  { 
    int j;
    for(j=1;j<T->n;j++) 
      if(T->nodes[j].data==cur_e)
        return T->nodes[T->nodes[j].parent].data;
    return Nil;
  }
  ```

- 获取节点的最左孩子节点

  ```c
  TElemType LeftChild(PTree *T,TElemType cur_e)
  { 
    int i,j;
    for(i=0;i<T->n;i++)
      if(T->nodes[i].data==cur_e) 
        break;
    for(j=i+1;j<T->n;j++) 
      if(T->nodes[j].parent==i) 
        return T->nodes[j].data;
    return Nil;
  }
  ```

- 获取节点的右兄弟节点

  ```c
  TElemType RightSibling(PTree *T,TElemType cur_e)
  { 
    int i;
    for(i=0;i<T->n;i++)
      if(T->nodes[i].data==cur_e) 
        break;
    if(T->nodes[i+1].parent==T->nodes[i].parent)
      return T->nodes[i+1].data;
    return Nil;
  }
  ```

- 输出树

  ```c
  void Print(PTree *T)
  { 
    int i;
    printf("节点个数=%d\n",T->n);
    printf(" 节点 父节点\n");
    for(i=0;i<T->n;i++)
    {
      printf("    %c",Value(T,i)); 
      if(T->nodes[i].parent>=0) 
        printf("    %c",Value(T,T->nodes[i].parent)); 
      printf("\n");
    }
  }
  ```

- 向树中插入另一棵树

  ```c
  Status InsertChild(PTree *T,TElemType p,int i,PTree c)
  { /* 初始条件：树T存在，p是T中某个节点，1≤i≤p所指节点的度+1，非空树c与T不相交 */
    /* 操作结果：插入c为T中p节点的第i棵子树 */
    int j,k,l,f=1,n=0; /* 设交换标志f的初值为1，p的孩子数n的初值为0 */
    PTNode t;
    if(!TreeEmpty(T)) /* T不空 */
    {
      for(j=0;j<T->n;j++) /* 在T中找p的序号 */
        if(T->nodes[j].data==p) /* p的序号为j */
          break;
      l=j+1; /* 如果c是p的第1棵子树，则插在j+1处 */
      if(i>1) /* c不是p的第1棵子树 */
      {
        for(k=j+1;k<T->n;k++) /* 从j+1开始找p的前i-1个孩子 */
          if(T->nodes[k].parent==j) /* 当前节点是p的孩子 */
          {
            n++; /* 孩子数加1 */
            if(n==i-1) /* 找到p的第i-1个孩子，其序号为k1 */
              break;
          }
        l=k+1; /* c插在k+1处 */
      } /* p的序号为j，c插在l处 */
      if(l<T->n) /* 插入点l不在最后 */
        for(k=T->n-1;k>=l;k--) /* 依次将序号l以后的节点向后移c.n个位置 */
        {
          T->nodes[k+c.n]=T->nodes[k];
          if(T->nodes[k].parent>=l)
            T->nodes[k+c.n].parent+=c.n;
        }
      for(k=0;k<c.n;k++)
      {
        T->nodes[l+k].data=c.nodes[k].data; /* 依次将树c的所有节点插于此处 */
        T->nodes[l+k].parent=c.nodes[k].parent+l;
      }
      T->nodes[l].parent=j; /* 树c的根节点的父节点为p */
      T->n+=c.n; /* 树T的节点数加c.n个 */
      while(f)
      { /* 从插入点之后，将节点仍按层序排列 */
        f=0; /* 交换标志置0 */
        for(j=l;j<T->n-1;j++)
          if(T->nodes[j].parent>T->nodes[j+1].parent)
          {/* 如果节点j的父节点排在节点j+1的父节点之后（树没有按层序排列），交换两节点*/
            t=T->nodes[j];
            T->nodes[j]=T->nodes[j+1];
            T->nodes[j+1]=t;
            f=1; /* 交换标志置1 */
            for(k=j;k<T->n;k++) /* 改变父节点序号 */
              if(T->nodes[k].parent==j)
                T->nodes[k].parent++; /* 父节点序号改为j+1 */
              else if(T->nodes[k].parent==j+1)
                T->nodes[k].parent--; /* 父节点序号改为j */
          }
      }
      return OK;
    }
    else /* 树T不存在 */
      return ERROR;
  }
  
  ```

- 删除子树

  ```c
  Status deleted[MAX_TREE_SIZE+1]; /* 删除标志数组(全局量) */
  void DeleteChild(PTree *T,TElemType p,int i)
  { /* 初始条件：树T存在，p是T中某个节点，1≤i≤p所指节点的度 */
    /* 操作结果：删除T中节点p的第i棵子树 */
    int j,k,n=0;
    LinkQueue q;
    QElemType pq,qq;
    for(j=0;j<=T->n;j++)
      deleted[j]=0; /* 置初值为0(不删除标记) */
    pq.name='a'; /* 此成员不用 */
    InitQueue(&q); /* 初始化队列 */
    for(j=0;j<T->n;j++)
      if(T->nodes[j].data==p)
        break; /* j为节点p的序号 */
    for(k=j+1;k<T->n;k++)
    {
      if(T->nodes[k].parent==j)
        n++;
      if(n==i)
        break; /* k为p的第i棵子树节点的序号 */
    }
    if(k<T->n) /* p的第i棵子树节点存在 */
    {
      n=0;
      pq.num=k;
      deleted[k]=1; /* 置删除标记 */
      n++;
      EnQueue(&q,pq);
      while(!QueueEmpty(q))
      {
        DeQueue(&q,&qq);
        for(j=qq.num+1;j<T->n;j++)
          if(T->nodes[j].parent==qq.num)
          {
            pq.num=j;
            deleted[j]=1; /* 置删除标记 */
            n++;
            EnQueue(&q,pq);
          }
      }
      for(j=0;j<T->n;j++)
        if(deleted[j]==1)
        {
          for(k=j+1;k<=T->n;k++)
          {
            deleted[k-1]=deleted[k];
            T->nodes[k-1]=T->nodes[k];
            if(T->nodes[k].parent>j)
              T->nodes[k-1].parent--;
          }
          j--;
        }
      T->n-=n; /* n为待删除节点数 */
    }
  }
  ```

- 层序遍历树

  ```c
  void TraverseTree(PTree *T,void(*Visit)(TElemType))
  { /* 初始条件：二叉树T存在,Visit是对节点操作的应用函数 */
    /* 操作结果：层序遍历树T,对每个节点调用函数Visit一次且仅一次 */
    int i;
    for(i=0;i<T->n;i++)
      Visit(T->nodes[i].data);
    printf("\n");
  }
  ```


### 二叉搜索树

### 红黑树

### B树

### 哈夫曼树

