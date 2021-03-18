# lower_bound和upper_bound

lower_bound和upper_bound在algorithm头文件中，greater在functional头文件中

* 函数lower_bound(first,last)在first和last中的前闭后开区 间进行二分查找，返回大于或等于val的第一个元素位 置。如果所有元素都小于val，则返回last的位置。

* 函数upper_bound(first,last)在first和last中的前闭后开区 间进行二分查找，返回大于val的第一个元素位置。如果所有元素都小于val，则返回last的位置。

* Lower_bound(first,last,greater<type>() )在first和last中 的前闭后开区间进行二分查找，返回小于或等于val的第一个元素位置。如果所有元素都小于val，则返回last的位置。

* Lower_bound(first,last,greater<type>() )在first和last中 的前闭后开区间进行二分查找，返回小于val的第一个元素位置。如果所有元素都小于val，则返回last的位置。

例：

num[7]={4,10,11,30,69,70,96,100},插入3，9，111 

int pos=lower_bound(num,num+8,3)-num; 

返回pos=0 

int pos=lower_bound(num,num+8,9)-num; 

返回pos=1 

int pos=lower_bound(num,num+8,111)-num; 

返回pos=8 

(注意函数返回的是指针类型) 