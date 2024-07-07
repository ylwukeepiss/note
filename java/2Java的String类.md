### String类解析
#### 先来一道题
```
    String a = new String("abc");
    上述代码生成了多少个对象？
```

#### 答案是，1个或者2个；
#### 先看一张JMM结构图：

![](../resource/jvm/jvm内存结构图.png)

#### 在JDK8发布后，字符串常量池被挪到了Heap中；
#### new String("abc"); 首先在字符串常量池中检查是否存在 abc，不存在则创建，随后在Heap中创建
####
