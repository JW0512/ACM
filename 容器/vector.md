# 向量容器vector

**头文件：** #include < vector >
**内部数据结构：** 连续存储的数组形式（一端开口的组）
**特点：** 高效率获取元素，插入和删除效率很低
**注意事项：** 插入和删除会使迭代器失效

---

### 构造函数
|用法|解释
|---|---
|vector < Type > v;|新建Type类型的vector
|vector < Type > v2(v1);|v2是v1的副本
|vector < Type > v2=v1;|赋值，两者大小可以不一样
|vector < Type > v(n);|v包含n个元素
|vector < Type > v(n,i);|v包含n个元素，全部初始化为i
|vector < Type > v{a,b,c,...};|包含初始个数的的元素，每个元素赋予相应初值
|vector < Type > v={a,b,c,...};|同上
|vector<vector< Type > > v;|嵌套vector，相当于二维数组
注意：圆括号()只能在定义时使用

### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
|用法|解释|时间复杂度
|---|---|---
|v.push_back(a);|给表尾插入数据|O(1)
|v.insert(it, val);|向迭代器it指向的元素前插入新元素val|O(N)
|v.insert(it, n, x);|向迭代器it指向的元素前插入n个x|O(N)
|v.insert(it, first, last);|将迭代器指定的序列[first, last)插入到迭代器it指向的元素前|O(N)
|v.emplace(it,a1,a2,a3,...);|从it位置开始插入a1,a2,a3,...|O(N)
|v.emplace_back(a);|给v后面插入元素a|O(1)
|v.assign(v2.begin(),v2.end());|v重新初始化为v2的值|O(N)
|v.assign({a1,a2,a3,...});|重新初始化|O(N)
|v1.swap(v2);|交换v1和v2（其实是交换了首尾指针）|O(1)
emplace效率比insert和push_back高一点，因为不触发拷贝构造和转移构造


### 访问元素

|用法|解释|时间复杂度
|---|---|---
|v[i];|访问第i个元素|O(1)
|v.at(i);|访问第i个元素，会检查边界是否越界|O(1)
|v.front();|返回首元素|O(1)
|v.back();|返回最后一个元素|O(1)
|v.data();|返回一个指针，指向vector存储数据的地址|O(1)
|v.begin();|返回首元素的**地址**|O(1)
|v.end();|返回最后一个元素**后一位的地址**|O(1)
|v.rbegin();|返回最后一个元素**地址**(++取前一个元素)|O(1)
|v.rend();|返回第一个元素之前的**虚拟地址**(--取后一个元素)|O(1)
|cbegin(),cend(),crbegin(),crend()|返回一个const迭代器|O(1)


### 删除元素
|用法|解释|时间复杂度
|---|---|---
|v.pop_back();|删除尾端元素|O(1)
|v.erase(it);|删除迭代器it指向的元素|O(N)
|v.erase(first,last);|删除迭代器指定区间 [first,last) 的元素|O(N)
|v.clear();|清空全部元素|O(N)




### 长度&容量
|用法|解释|时间复杂度
|---|---|---
|v.size();|获取v的使用长度|O(1)
|v.max_size();|返回在当前平台下能给Type类型的vector分配的最大容量|O(1)
|v.resize(n,val);|修改v的使用长度为n，并初始化为val(val可略去)|O(N)
|v.capacity();|返回vector的当前容量|O(1)
|v.empty();|为空返回true，不为空返回false|O(1)
|v.reserve(n);|添加一段容量n|O(N)

### 迭代器
|用法|解释
|---|---
|vector<Type>:: iterator it;|定义一个Type类型的迭代器it
|vector<Type>::reverse_iterator it;|定义一个Type类型的反向迭代器it
|	for (auto it=vec.begin();it!=vec.end();it++)<br>cout<<*it<<endl; |用auto遍历
|	for (auto it: vec)<br>cout<<it<<endl; |用auto遍历

|支持的操作|解释
|---|---
|operator* |取内容
|operator-> |取地址
|operator++ |自增
|operator-- |自减
|operator+ |位置加
|operator- |位置减
|operator+= |加等于
|operator-= |减等于

