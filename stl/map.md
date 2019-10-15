##map的用法
####定义
```cpp
map<int,int> mp;
```
####插入
```cpp
mp.insert(10);
mp[1]=10
```

####迭代器
```cpp
map<int,int>::iterator it=mp.begin();
cout<<it->first<<endl;//或(*it).first
cout<<it->second<<endl;//或(*it).second
```

####遍历
```cpp
法一：
for(auto i: mp)
{
    cout<<(*i).first<<" "<<(*i).second<<endl;
}

法二：
map<int,int>::iterator it=mp.begin();
for(it;it!=mp.end();it++)
{
    cout<<(*it).first<<" "<<(*it).second<<endl;
}
```


####查找键值
```cpp
法一：
auto it = mp.find(2);
if(it!=mp.end())
{
    cout<<(*it).second<<endl;
}

法二：
if(mp.count(2)>0)
{
   cout<<mp[2]<<endl;
}
```

####删除
```cpp
法一：
map<int,int>:: iterator it;
it=mp.find(2);
mp.erase(it);

法二：
int x=mp.erase(2);//1删除成功，0删除失败
```

####清空
```cpp
法一：
mp.clear();

法二：
mp.erase(mp.begin(),mp.begin());//删除一段范围
```