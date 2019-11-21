# 队列容器deque

**头文件：** #include < deque >
**内部数据结构：** 连续或分段连续存储数组（两端
开口的数组）
**特点：** 获取元素效率较高，插入和删除的效率较高
**注意事项：** 禁止在首尾之外的地方访问/插入元素

## 函数详解

### 构造函数
|用法|解释
|---|---
|deque < Type > deq;|新建Type类型的deque
|deque < Type > deq2(deq1);|deq2是deq1的副本
|deque < Type > deq2=deq1;|同上
|deque < Type > deq(n);|deq包含n个元素
|deque < Type > deq(n,i);|deq包含n个元素，全部初始化为i
|deque < Type > deq{a,b,c,...};|包含初始个数的的元素，每个元素赋予相应初值
|deque < Type > deq={a,b,c,...};|同上
|deque<deque< Type > > deq;|嵌套deque，相当于二维数组


### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
|用法|解释|时间复杂度
|---|---|---
|deq.push_front(a);|给队首插入数据|O(1)
|deq.push_back(a);|给队尾插入数据|O(1)
|deq.insert(it, val);|向迭代器it指向的元素前插入新元素deqal|O(N)
|deq.insert(it, n, x);|向迭代器it指向的元素前插入n个x|O(N)
|deq.insert(it, first, last);|将迭代器指定的序列[first, last)插入到迭代器it指向的元素前|O(N)
|deq.emplace(it,a);|从it位置开始插入元素a|O(N)
|deq.emplace_front(a);|给deq前插入元素a|O(1)
|deq.emplace_back(a);|给deq后插入元素a|O(1)
|deq.assign(deq2.begin(),deq2.end());|deq重新初始化为deq2的值|O(N)
|deq.assign(n,val);|重新初始化n个数，值为val|O(N)
|deq.assign({a1,a2,a3,...});|重新初始化|O(N)
|deq1.swap(deq2);|交换deq1和deq2|O(1)
emplace效率比insert和push_back高一点，因为不触发拷贝构造和转移构造


### 访问元素

|用法|解释|时间复杂度
|---|---|---
|deq[i];|访问第i个元素|O(1)
|deq.at(i);|访问第i个元素，会检查边界是否越界|O(1)
|deq.front();|返回首元素|O(1)
|deq.back();|返回最后一个元素|O(1)
|deq.begin();|返回首元素的**地址**|O(1)
|deq.end();|返回最后一个元素**后一位的地址**|O(1)
|deq.rbegin();|返回最后一个元素**地址**(++取前一个元素)|O(1)
|deq.rend();|返回第一个元素之前的**虚拟地址**(--取后一个元素)|O(1)
|cbegin(),cend(),crbegin(),crend()|返回一个const迭代器|O(1)


### 删除元素
|用法|解释|时间复杂度
|---|---|---
|deq.pop_front();|删除队首元素|O(1)
|deq.pop_back();|删除队尾元素|O(1)
|deq.erase(it);|删除迭代器it指向的元素|O(N)
|deq.erase(first,last);|删除迭代器指定区间 [first,last) 的元素|O(N)
|deq.clear();|清空全部元素|O(N)




### 长度&容量
|用法|解释|时间复杂度
|---|---|---
|deq.size();|获取deq的使用长度|O(1)
|deq.max_size();|返回在当前平台下能给Type类型的deque分配的最大容量|O(1)
|deq.resize(n,val);|修改deq的使用长度为n，并初始化为val(val可略去)|O(N)
|deq.empty();|为空返回true，不为空返回false|O(1)

### 迭代器
|用法|解释|时间复杂度
|---|---|---
|deque<Type>:: iterator it;|定义一个Type类型的迭代器it
|deque<Type>::reverse_iterator it;|定义一个Type类型的反向迭代器it
|	for (auto it=deq.begin();it!=deq.end();it++) cout<<*it<<endl; |用auto遍历
|	for (auto it: deq) cout<<it<<endl; |用auto遍历

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

