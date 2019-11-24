# 队列容器priority_queue

**头文件：** #include < priority_queue >
**内部数据结构：** 连续或分段连续存储数组（两端
开口的数组）
**特点：** 获取元素效率较高，插入和删除的效率较高
**注意事项：** 禁止在首尾之外的地方访问/插入元素

## 函数详解

### 构造函数
|用法|解释
|---|---
|priority_queue < Type > priq;|新建Type类型的priority_queue
|priority_queue < Type > priq2(priq1);|priq2是priq1的副本
|priority_queue < Type > priq2=priq1;|同上
|priority_queue<int,vector<int>,greater<int> > priq;|从大到小排序，最小值优先
|priority_queue<int,vector<int>,less<int> > priq;|从小到大排序，最大值优先，后两个参数不写默认为这个
由于元素是从顶部弹出的，所以输出大小顺序是反的

#### 自定义优先级
* 从小到大输出
```cpp
priority_queue<int, vector<int>, greater<int> > q;
```
* 从小到大输出
```cpp
struct cmp
{
	bool operator ()(int x, int y)
	{
		return x > y;
	}
};
priority_queue<int, vector<int>, cmp> q;
```
* 按照结构体的x从小到大输出
```cpp
struct node
{
	int x, y;
	friend bool operator < (node a, node b)
	{
		return a.x > b.x;
	}
}no[macn];
priority_queue<node> q;
```
### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
|用法|解释|时间复杂度
|---|---|---
|priq.push(a);|给队尾插入数据|O(1)
|priq.emplace(a);|给队尾插入数据|O(1)
|priq1.swap(v2);|交换priq1和priq2|O(1)
emplace效率比push高一点，因为不触发拷贝构造和转移构造


### 访问元素

|用法|解释|时间复杂度
|---|---|---
|priq.top();|返回队尾元素|O(1)


### 删除元素
|用法|解释|时间复杂度
|---|---|---
|priq.pop();|删除队尾元素|O(1)




### 长度&容量
|用法|解释|时间复杂度
|---|---|---
|priq.size();|获取priq的长度|O(1)
|priq.empty();|为空返回true，不为空返回false|O(1)