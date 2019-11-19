# 向量容器vector

**头文件：** #include < vector >
**内部数据结构：** 连续存储的数组形式（一端开口的组）
**特点：** 高效率获取元素，插入和删除效率很低
**注意事项：** 插入和删除会使迭代器失效

## 函数详解

### 构造函数
```cpp
vector <Type> v;//新建Type类型的vector
vetor <Type> v2(v1);//v2是v1的副本
vector <Type> v(n);//v包含n个元素
vector <Type> v(n,i);//v包含n个元素，全部初始化为i
```
