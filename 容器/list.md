# 列表容器list

**头文件：** #include < list >
**内部数据结构：** 双向环状链表
**特点：** 获取元素效率很低，插入和删除的效率很高
**注意事项：** 插入，迭代器不会失效。删除，指向被删除节点迭代器失效。

---

### 构造函数
| 用法 | 解释
| --- | ---
| list < Type > l; | 新建Type类型的list
| list < Type > l2(l1); | l2是l1的副本
| list < Type > 12 = 11; | 赋值，两者大小可以不一样
| list < Type > 12 (l1.begin(),l1.end()); | 同上
| list < Type > l(n); | l包含n个元素
| list < Type > l(n, i); | l包含n个元素，全部初始化为i
| list < Type > l{ a,b,c,... }; | 包含初始个数的的元素，每个元素赋予相应初值
| list < Type > l = { a,b,c,... }; | 同上
注意：圆括号()只能在定义时使用

### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| l.push_back(a);<br>l.push_front(a); | 给表尾插入数据 <br> 给表首插入数据| O(1)
| l.insert(it, val); | 向迭代器it指向的元素前插入新元素val | O(N)
| l.insert(it, n, x); | 向迭代器it指向的元素前插入n个x | O(N)
| l.insert(it, first, last); | 将迭代器指定的序列[first, last)插入到迭代器it指向的元素前 | O(N)
| l.emplace(it, a); | 在it位置插入a| O(1)
| l.emplace_back(a);<br>l.emplace_front(a); | 给l后面插入元素a<br>给l前面插入元素a | O(1)
| l.assign(l2.begin(), l2.end()); | l重新初始化为l2的值 | O(N)
| l.assign(n, i); | l包含n个元素，全部初始化为i|O(N)
| l.assign({ a1,a2,a3,... }); | 重新初始化 | O(N)
| l1.swap(l2); | 交换l1和l2 | O(1)
| l1.merge(l2);|合并两个有序链表到l1中|O(N)
| l1.splice(it,l2);|在l1的it前插入l2所有元素|O(1)
| l1.splice(it1,l2,it2);|l1的it1位置插入l2的it2指向的元素|O(1)
| l.splice(l1.begin(),it1,it2,l1.end());|l1的【begin到it2】和【it2到end】交换顺序|O(N)
emplace效率比insert和push_back高一点，因为不触发拷贝构造和转移构造

### 访问元素

| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| l.front(); | 返回首元素 | O(1)
| l.back(); | 返回最后一个元素 | O(1)
| l.begin(); | 返回首元素的 **地址** | O(1)
| l.end(); | 返回最后一个元素 **后一位的地址** | O(1)
| l.rbegin(); | 返回最后一个元素 **地址** (++取前一个元素) | O(1)
| l.rend(); | 返回第一个元素之前的 **虚拟地址** (--取后一个元素) | O(1)
| cbegin(), cend(), crbegin(), crend() | 返回一个const迭代器 | O(1)
|l.sort(cmp);|按照cmp排序|O(N)
sort的用法：
```cpp
bool cmp(int a,int b)//从大到小排序
{
	return a > b;
}
int main()
{
    list<int> l{1,2,3,4};
    l.sort(cmp);
}
```
### 删除元素
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| l.pop_back();<br>l.pop_front(); | 弹出表尾数据 <br> 弹出表首数据| O(1)
| l.erase(it); | 删除迭代器it指向的元素 | O(N)
| l.erase(first, last); | 删除迭代器指定区间[first, last) 的元素 | O(N)
|l.remove(a);|“删除”元素a，其实是放到链表尾部，不减少size|O(N)
|l.remove_if(begin, end, op) |移除区间[begin,end)中每一个“令op(elem)获得true”的元素|O(N)
| l.clear(); | 清空全部元素 | O(N)
| l.unique();|去重，必须对有序链表操作|O(N)

### 长度&容量
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| l.size(); | 获取l的使用长度 | O(1)
| l.max_size(); | 返回在当前平台下能给Type类型的list分配的最大容量 | O(1)
| l.resize(n, val); | 修改l的使用长度为n，并初始化为val(val可略去) | O(N)
| l.empty(); | 为空返回true，不为空返回false | O(1)

### 迭代器
| 用法 | 解释
| --- | ---
| list<Type>::iterator it; | 定义一个Type类型的迭代器it
| list<Type>::reverse_iterator it; | 定义一个Type类型的反向迭代器it
| for (auto it = vec.begin(); it != vec.end(); it++) <br> cout << *it << endl; | 用auto遍历
| for (auto it : vec) <br> cout << it << endl; | 用auto遍历

| 支持的操作 | 解释
| --- | ---
| operator* | 取内容
| operator-> | 取地址
| operator++ | 自增
| operator-- | 自减
| operator+ | 位置加
| operator- | 位置减
| operator+= | 加等于
| operator-= | 减等于