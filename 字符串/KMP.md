## KMP

```cpp
const int max_len = 1e6;
int fail[max_len];
void get_fail(string &s)//模式串
{
	int len = s.length();
	fail[0] = 0;
	for (int i = 1, k = 0; i < len; i++)
	{
		while (k > 0 && s[i] != s[k])
			k = fail[k - 1];//失去匹配后，看上个字符失去匹配后应该匹配的下个位置
			if (s[i] == s[k])
				k++;
			fail[i] = k;
	}
}
int KmpMatch(string &T, string &P)//这里是求主串中第一个匹配成功的下标 如果没有返回-1
{
	int n, m;
	n = T.size();
	m = P.size();
	if (!n || !m) return -1;//如果有一个为空的话 返回-1
	for (int i = 0, q = 0; i < n; i++)//q为模式串的下标
	{
		while (q > 0 && P[q] != T[i])
		q = fail[q - 1];
//当失去匹配的时候 模式串的下标跳回直到归零或者匹配
		if (P[q] == T[i])
			q++; //如果两个匹配上了 那模式串下标+1 表示多匹配上了一个字符
  		if (q == m)//如果匹配的个数等于模式串的长度的话 就返回
		{
			return i - m + 1;
		}
	}
	return -1;//匹配失败 返回-1
}
```

