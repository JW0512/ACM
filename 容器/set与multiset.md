# set与multiset
## 集合set

**头文件：** #include < set >
**内部数据结构：** 红黑树（平衡检索二叉树）
**特点：** 
1. 键（关键字）和值（数据）相等
2. 键唯一，无重复元素
3. 元素默认按升序排列
4. 插入删除效率高

**注意事项：** 插入，迭代器不会失效。删除，指向被删除节点迭代器失效。




## 多重集合multiset

**头文件：** #include < set >
**内部数据结构：** 红黑树（平衡检索二叉树）
**特点：** 
1. 键和值相等
2. 键可以不唯一
3. 元素默认按升序排列

**注意事项：** 插入，迭代器不会失效。删除，指向被删除节点迭代器失效。


**set与multiset的函数和用法一样，区别只有multiset可以插入重复数据**

---

### 构造函数
| 用法 | 解释
| --- | ---
| set < Type > s; | 新建Type类型的set
| set < Type > s2(s1);|s2是s1的副本
| set < Type > s2 = s1; |赋值,两者大小不一样也可以
| set < Type > s2(s1.begin(),s1.end()); | 同上
|int a[5]={1,2,3,4,5};<br>set < int > s(a,a+5); | 包含初始个数的的元素，每个元素赋予相应初值
注意：圆括号()只能在定义时使用

**自定义类型的构造方法**
```cpp
struct S {
    int x, y;
};
struct cmp {
    bool operator()(const S& a, const S& b) {
        return a.x < b.x || a.x == b.x && a.y < b.y;
    }
};
multiset<S, cmp> a;
```


### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| s.insert(val); | 插入元素val | O(logN)
| s.insert(it, val); | 向迭代器it指向位置插入val|O(1)~O(logN)
| int a[3]={1,2,3};<br>s.insert(a,a+3); | 将序列[first, last)插入到set中 | O(NlogN)
| s.emplace(val); | 插入元素val | O(logN)
| s.emplace_hint(it,val); | 向迭代器it指向位置val|O(1)~O(logN)
| s1.swap(s2); | 交换s1和s2 | O(1)
emplace效率比insert高一点，因为不触发拷贝构造和转移构造


### 访问元素

| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| s.begin(); | 返回首元素的 **地址** | O(1)
| s.end(); | 返回最后一个元素 **后一位的地址** | O(1)
| s.rbegin(); | 返回最后一个元素 **地址** (++取前一个元素) | O(1)
| s.rend(); | 返回第一个元素之前的 **虚拟地址** (--取后一个元素) | O(1)
| cbegin(), cend(), crbegin(), crend() | 返回一个const迭代器 | O(1)
|s.find(val);|返回val的**地址**|O(logN)
|s.lower_bound(val);|返回大于等于val的第一个数的地址|O(logN)
|s.upper_bound(val);|返回大于val的第一个数的地址|O(logN)
|s.equal_range(val);|返回一对pair地址，第一个是lower_bound后的地址，第二个是upper_bound后的地址|O(logN)
|s.key_comp();|比较键，小于返回1，否则返回0|O(1)
|s.value_comp();|比较值，小于返回1，否则返回0|O(1)
|s.count(val);|数s中有几个val，set中只有0和1|O(1)

**key_comp()的用法**
```cpp
int a[5] = { 1,3,4,5,6 };
set<int> s1(a, a + 5);
set<int>::iterator it = s1.begin();
set<int>::reverse_iterator it2 = s1.rbegin();
set<int>::key_compare cmp = s1.key_comp();
cout << *it << " " << *it2 << endl;
cout << cmp(*it, *it2) << endl;//value_comp同理
```
**equal_range(val)的用法**
```cpp
pair<set<int>::iterator, set<int>::iterator> it;
it = s1.equal_range(3);
cout << "the lower bound points to: " << *it.first << '\n';
cout << "the upper bound points to: " << *it.second << '\n';
```

### 删除元素
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| s.erase(val); | 删除元素val | O(logN)
| s.erase(it); | 删除迭代器it指向的元素 | O(1)
| s.erase(first,last); | 删除[first,last)区间的元素 | O(N)
| s.clear(); | 清空全部元素 | O(N)


### 长度&容量
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| s.size(); | 获取s的使用容量 | O(1)
| s.max_size(); | 返回在当前平台下能给Type类型的set分配的最大容量 | O(1)
| s.empty(); | 为空返回true，不为空返回false | O(1)

### 迭代器
| 用法 | 解释 |
| --- | --- |
| set<Type>::iterator it; | 定义一个Type类型的迭代器it
| set<Type>::reverse_iterator it; | 定义一个Type类型的反向迭代器it
| for (auto it = vec.begin(); it != vec.end(); it++)<br>cout << *it << endl; | 用auto遍历
| for (auto it : vec)<br>cout << it << endl; | 用auto遍历

| 支持的操作 | 解释
| --- | --- |
|operator* | 取内容
| operator-> | 取地址
| operator++ | 自增
| operator-- | 自减
| operator+ | 位置加
| operator- | 位置减
| operator+= | 加等于
| operator-= | 减等于

