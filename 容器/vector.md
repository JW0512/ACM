# 向量容器vector

**头文件：** #include < vector >
**内部数据结构：** 连续存储的数组形式（一端开口的组）
**特点：** 高效率获取元素，插入和删除效率很低
**注意事项：** 插入和删除会使迭代器失效

## 函数详解

### 构造函数
|用法                   |解释                  |
|-----------------------|----------------------|
|vector < Type > v;|新建Type类型的vector
|vector < Type > v2(v1);|v2是v1的副本
|vector < Type > v2=v1;|同上
|vector < Type > v(n);|v包含n个元素
|vector < Type > v(n,i);|v包含n个元素，全部初始化为i
|vector < Type > v{a,b,c,...};|包含初始个数的的元素，每个元素赋予相应初值
|vector < Type > v={a,b,c,...};|同上
|vector<vector< Type > > v;|嵌套vector，相当于二维数组


### 插入数据
不能通过下标的形式直接插入
|用法                   |解释                  |
|-----------------------|----------------------|
|v.push_back(a);|给表尾插入数据
|v.insert(it, val);|向迭代器it指向的元素前插入新元素val
|v.insert(it, n, x);|向迭代器it指向的元素前插入n个x
|v.insert(it, first, last);|将迭代器指定的序列[first, last)插入到迭代器it指向的元素前


### 访问元素
不建议使用v[i]=a;来修改已有数据，因为容易访问越界
|用法                   |解释                  |
|-----------------------|----------------------|
|v[i];|访问第i个元素
|v.front();|返回首元素
|v.back();|返回最后一个元素


### 删除元素
|用法                   |解释                  |
|-----------------------|----------------------|
|v.pop_back();|删除尾端元素
|v.erase(it);|删除迭代器it指向的元素
|v.erase(first,last);|删除迭代器指定区间 [first,last) 的元素
|v.clear();|清空全部元素

### 获取当前长度或者当前容量
|用法                   |解释                  |
|-----------------------|----------------------|
|v.size();|获取v的长度
|v.empty();|为空返回true，不为空返回false

### 迭代器
|用法                   |解释                  |
|-----------------------|----------------------|
|vector<Type>:: iterator it;|定义一个Type类型的迭代器it
|vector<Type>::reverse_iterator it;|定义一个Type类型的反向迭代器it
|	for (auto it=vec.begin();it!=vec.end();it++) cout<<*it<<endl; |用auto遍历
|	for (auto it: vec) cout<<it<<endl; |用auto遍历