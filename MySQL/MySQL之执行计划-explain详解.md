## MySQL执行计划详解

#### mysql执行计划有如下10列：

<table> 
    <th>id</th>
    <th>select_type</th>
    <th>table</th>
    <th>type</th>
    <th>possible_key</th>
    <th>key</th>
    <th>key_len</th>
    <th>ref</th>
    <th>rows</th>
    <th>Extra</th>
</table>

#### 其中最重要的列为：id、type、key、rows、Extra
### id
#### id相同：执行顺序由上到下；
![](../resource/MySQL/MySQL-Explain详解-相同id.png)

#### id不同：如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行；
![](../resource/MySQL/MySQL-Explain详解-不同id.png)

### 参考资料💾
#### <a href="https://blog.csdn.net/wuseyukui/article/details/71512793">MySQL高级 之 explain执行计划详解</a>
