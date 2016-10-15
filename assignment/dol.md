# DOL实例分析&编程

### 实验任务

***

1. 修改example2，让3个square模块变成2个（__修改xml的iterator__）
2. 修改example1，使其输出3次方数（__修改square.c__）

### 实验步骤

***

#####任务一

1. 方法一：修改初始定义的迭代次数变量“N”

   * example2.xml中定义了“N” 为3（如下图），表明有3个square模块

     `<variable value="3" name="N"/>`

   * 将“N”的定义改为2，控制整个迭代过程的次数

     `<variable value="2" name="N"/>`

2. 方法二：修改具体迭代过程的次数参数

   example2.xml中的迭代部分包括__square进程定义__、__通道个数定义__、__连接square的通道定义__（连接square的INPUT端口/OUTPUT端口）

   * 修改square进程定义的次数

     variable从0~2变化，迭代创建了3个square进程，因此将迭代次数减少1，即variable下限减少1

     `<iterator variable="i" range="N">`

     将range="N"  改为range="N-1"  

     `<iterator variable="i" range="N-1">`

   * 修改通道个数定义的次数

     variable从0~3变化，迭代创建了4个通道，因此将迭代次数减少1，即variable下限减少1

     `<iterator variable="i" range="N+1">`


     将range="N+1"  改为range="N"  

     `<iterator variable="i" range="N">`

   * 修改连接square的通道定义的次数

     * 中间连接到square的INPUT端口/OUTPUT端口

       variable从0~2变化，迭代创建了3个连接，因此将迭代次数减少1，即variable下限减少1  

        `<iterator variable="i" range="N">`
           
        将range="N"  改为range="N-1"  

        ` <iterator variable="i" range="N-1">`

     * 最后一个通道连接到消费者consumer的INPUT端口

       `<append function="N"/>`

        最后一个通道function为“N-1”而不再是“N”

        `<append function="N-1"/>`

     ​


#####任务二

通过修改__square.c__中的计算部分达到输出3次方的效果

`i = i*i;`

将原本的计算2次方改为计算3次方

`i = i*i*i;`

PS：由于此模块计算的不再是平方，因此修改计算模块名为“cube”（只需更改模块的“name”及更新此模块引用时的名字）



### 实验结果

***

1. example2运行结果及dot图

   ![1](http://p1.bqimg.com/567571/00f1781ade617e7c.png)

   ![3](http://p1.bpimg.com/567571/8d7c9440e4829bae.png)

2. example1运行结果及dot图

   ![2](http://p1.bpimg.com/567571/b21c6ca12fa587d8.png)

   ![4](http://i1.piimg.com/567571/548d287049fe14ac.png)




### 实验感想

***

本次实验较为简单，特别是建立在实验文档及课上TA对代码分析十分详细的基础上。实验过程中唯一遇到的问题是，在任务一修改example2.xml的时候没有更改最后的通道的function，导致代码中试图用一个不存在的端口连接消费者的输入端口。在发现这个问题之前，我将该example编译时一直报错Hds文件夹不存在，即编译时无法生成该example的Hds文件夹。最后我将代码修改正确后这个问题就不再出现这个报错。

