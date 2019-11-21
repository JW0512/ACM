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


### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
|用法                   |解释                  |
|-----------------------|----------------------|
|v.push_back(a);|给表尾插入数据
|v.insert(it, val);|向迭代器it指向的元素前插入新元素val
|v.insert(it, n, x);|向迭代器it指向的元素前插入n个x
|v.insert(it, first, last);|将迭代器指定的序列[first, last)插入到迭代器it指向的元素前
|v.emplace(it,a1,a2,a3,...);|从it位置开始插入a1,a2,a3,...效率比insert高
|v.emplace_back(a);|给v后面插入元素a，效率比push_back高
|v.assign(v2.begin(),v2.end());|v重新初始化为v2的值
|v.assign({a1,a2,a3,...});|重新初始化
|v1.swap(v2);|交换v1和v2（其实是交换了首尾指针）
|v.reverse(first,last);|反转[first,last)区间


### 访问元素

|用法                   |解释                  |
|-----------------------|----------------------|
|v[i];|访问第i个元素
|v.at(i);|访问第i个元素，会检查边界是否越界
|v.front();|返回首元素
|v.back();|返回最后一个元素
|v.data();|返回一个指针，指向vector存储数据的地址
|v.begin();|返回首元素的**地址**
|v.end();|返回最后一个元素**后一位的地址**
|v.rbegin();|返回最后一个元素**地址**(++取前一个元素)
|v.rend();|返回第一个元素之前的**虚拟地址**(--取后一个元素)
|cbegin(),cend(),crbegin(),crend()|返回一个const迭代器


### 删除元素
|用法                   |解释                  |
|-----------------------|----------------------|
|v.pop_back();|删除尾端元素
|v.erase(it);|删除迭代器it指向的元素
|v.erase(first,last);|删除迭代器指定区间 [first,last) 的元素
|v.clear();|清空全部元素




### 长度&容量
|用法                   |解释                  |
|-----------------------|----------------------|
|v.size();|获取v的使用长度
|v.max_size();|返回在当前平台下能给Type类型的vector分配的最大容量
|v.resize(n,val);|修改v的使用长度为n，并初始化为val(val可略去)
|v.capacity();|返回vector的当前容量
|v.empty();|为空返回true，不为空返回false
|v.reserve(n);|添加一段容量n

### 迭代器
|用法                   |解释                  |
|-----------------------|----------------------|
|vector<Type>:: iterator it;|定义一个Type类型的迭代器it
|vector<Type>::reverse_iterator it;|定义一个Type类型的反向迭代器it
|	for (auto it=vec.begin();it!=vec.end();it++) cout<<*it<<endl; |用auto遍历
|	for (auto it: vec) cout<<it<<endl; |用auto遍历

|支持的操作|解释
|---|---|
|operator* |取内容
|operator-> |取地址
|operator++ |自增
|operator-- |自减
|operator+ |位置加
|operator- |位置减
|operator+= |加等于
|operator-= |减等于

