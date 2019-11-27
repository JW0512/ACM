# 映射map

**头文件：** #include < map >
**内部数据结构：** 红黑树
**特点：** 
1. 键和值分开（模版有两个参数，前面是键后面是值）
2. 键唯一
3. 元素默认按键的升序排列

**注意事项：** 插入，迭代器不会失效。删除，指向被删除节点迭代器失效。

# 多重映射multimap

**头文件：** #include < map >
**内部数据结构：** 红黑树
**特点：** 
1. 键和值分开
2. 键可以不唯一
3. 元素默认按键的升序排列

**注意事项：** 插入，迭代器不会失效。删除，指向被删除节点迭代器失效。

---

### 构造函数
| 用法 | 解释
| --- | ---
| map < Type1,Type2 > s; | 新建键为Type1，值为Type2的map
| map< Type1,Type2 > s2(s1);|s2是s1的副本
| map < Type1,Type2 > s2(s1.begin(), s1.end()); | 同上
| map < Type1,Type2 > s2 = s1; | 赋值，两者大小不一样也可以
注意：圆括号()只能在定义时使用


按value排序
```cpp
struct cmp {    
    bool operator()(const pair<int,int>& lhs, const pair<int,int>& rhs)const
    {      
         return lhs.second < rhs.second;
    }
};
sort(mp.begin(),mp.end(),cmp());
```

**自定义键值的构造方法**
方法一
```cpp
struct Person {
    string name;
    int age;
}p;
struct cmp
{
    bool operator ()(const Person& p1,const Person& p2) const //注意这里的两个const必须要有
    {
        return (p1.age < p2.age) || (p1.age == p2.age && p1.name.length() < p2.name.length());
    }
};
map<Person, int, cmp> mp;
p.age = 1, p.name = "Jing";
mp[p] = 1;
cout << mp[p] << endl;
```
方法二
```cpp
class Person {
public:
	string name;
	int age;

	Person(string n, int a) {
		name = n;
		age = a;
	}

	bool operator<(const Person& p) const //注意这里的两个const必须要有
	{
		return (age < p.age) || (age == p.age && name.length() < p.name.length());
	}
};
int main() {
	map<Person, int> mp;
	mp[Person("Jing", 20)] = 1;
	mp[Person("Wei", 21)] = 2;
	for (auto i : mp)
	{
		cout << i.first.name << " " << i.first.age << " ";
		cout << i.second << endl;
	}
	return 0;
}
```

### 修改数据
不能通过下标的形式插入新数据，可以通过下标修改已有数据，但易访问越界出错
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| mp.insert(make_pair(val1,val2)); | 插入键为val1，值为val2的元素 | O(logN)
| mp.insert(it, make_pair(val1,val2)); | 向迭代器it指向位置插入数据 | O(1)~O(logN)
| mp2.insert(mp1.begin(),mp2.end()); | 给mp2插入mp1[firsr,last)的数据 | O(NlogN)
| mp.emplace(val1,val2); | 插入键为val1，值为val2的元素 | O(logN)
| mp.emplace_hint(it, val1, val2); | 向迭代器it指向位置插入数据 | O(logN)
| mp1.swap(mp2); | 交换mp1和mp2 | O(1)
emplace效率比insert高一点，因为不触发拷贝构造和转移构造


### 访问元素

| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| mp[key]; |访问键值key对应的元素|O(logN)
| mp.at(key); |访问键值key对应的元素，会检查边界是否越界|O(logN)
| mp.begin(); | 返回首元素的 **地址** | O(1)
| mp.end(); | 返回最后一个元素 **后一位的地址** | O(1)
| mp.rbegin(); | 返回最后一个元素 **地址** (++取前一个元素) | O(1)
| mp.rend(); | 返回第一个元素之前的 **虚拟地址** (--取后一个元素) | O(1)
| cbegin(), cend(), crbegin(), crend() | 返回一个const迭代器 | O(1)
| mp.find(val1); | 返回键val1的 **地址** | O(logN)
| s.lower_bound(key); | 返回大于等于key的第一个数的地址 | O(logN)
| s.upper_bound(key); | 返回大于键val1的第一个数的地址 | O(logN)
| s.equal_range(key); | 返回一对pair地址，第一个是lower_bound后的地址，第二个是upper_bound后的地址 | O(logN)
| s.key_comp(); | 比较键，小于返回1，否则返回0 | O(1)
| s.value_comp(); | 比较值，小于返回1，否则返回0 | O(1)
| s.count(key); | 数s中有几个key，map中只有0和1 | O(1)
multimap没有[]和at的功能，因为它的键值可以重复

**key_comp()的用法**
```cpp
map<string, int> mp, mp2;
mp.emplace("a", 20);
mp.emplace("b", 10);
map<string,int>::iterator it = mp.begin();
map<string,int>::reverse_iterator it2 = mp.rbegin();
map<string, int>::key_compare cmp = mp.key_comp();
cout << (*it).first << " " << (*it2).first << endl;
cout << cmp((*it).first, (*it2).first) << endl;
//begin的键肯定比end的键小，输出1
//value_comp这里改成cmp(*it,*it2)，其他同理
//（但是这里测试后发现好像有点问题，value_comp也是按key比较的,功能和key_value一模一样）
```
**equal_range(val)的用法**
```cpp
pair<map<int>::iterator, map<int>::iterator> it;
it = s1.equal_range(3);
cout << "the lower bound points to: " << *it.first << '\n';
cout << "the upper bound points to: " << *it.second << '\n';
```

### 删除元素
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| mp.erase(key); | 删除key对应元素 | O(logN)
| s.erase(it); | 删除迭代器it指向的元素 | O(1)
| s.erase(first, last); | 删除[first, last)区间的元素 | O(N)
| s.clear(); | 清空全部元素 | O(N)

### 长度&容量
| 用法 | 解释 | 时间复杂度
| --- | --- | ---
| s.size(); | 获取s的使用容量 | O(1)
| s.max_size(); | 返回在当前平台下能给Type类型的map分配的最大容量 | O(1)
| s.empty(); | 为空返回true，不为空返回false | O(1)

### 迭代器
| 用法 | 解释 |
| --- | --- |
| map< Type1,Type2 >::iterator it; | 定义一个Type类型的迭代器it
| map< Type1,Type2 >::reverse_iterator it; | 定义一个Type类型的反向迭代器it
| for (auto it = vec.begin(); it != vec.end(); it++) <br> cout << *it << endl; | 用auto遍历
| for (auto it : vec) <br> cout << it << endl; | 用auto遍历

| 支持的操作 | 解释
| --- | --- |
| operator* | 取内容
| operator-> | 取地址
| operator++ | 自增
| operator-- | 自减
| operator+ | 位置加
| operator- | 位置减
| operator+= | 加等于
| operator-= | 减等于
