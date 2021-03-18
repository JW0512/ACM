## AC自动机

统计主串中有多少单词出现过

```cpp
#define MS(x) memset(x,0,sizeof(x));
typedef struct TrieNode//Trie树节点
{
	TrieNode *next[26];
	TrieNode *fail;//fail指针，当前节点失去匹配后所指向的节点
	int End;//标记当前节点是否为一个单词的结束位置，有的题可能有重复单词，所以不用bool用int
}Node;
Node* createNew()
{
	Node *p = new Node;
	for (int i = 0; i < 26; i++)
	{
		p->next[i] = NULL;
	}
	p->End = 0;
	p->fail = NULL;
	return p;
}
void Insert(string ss, Node* root)//简单粗暴的trie树插入
{
	int len = ss.length();
	Node *p = root;
	for (int i = 0; i < len; i++)
	{
		int c = ss[i] - 'a';
		p = p->next[c] ? p->next[c] : p->next[c] = createNew();
//p的next[c]是否存在？存在的话就往下走：不存在的话就新建一个 往下走
	}
	p->End++;//到终点了 以这个节点为终点的单词数量+1
}
void GetFail(Node*root)
{
	queue<Node*>q;
	q.push(root);
	Node* temp;
	while (!q.empty())//一个bfs 注意这里是更新子节点的fail指针
	{
		temp = q.front();
		q.pop();
		for (int i = 0; i < 26; i++)//遍历26个子节点
		{
			if (temp->next[i] != NULL)//如果下个节点不为空 才处理
			{
				q.push(temp->next[i]);//加入队列
				if (temp == root)//根节点的下个任意节点的fail指针都指向根节点
				{
					temp->next[i]->fail = root;
				}
				else
				{
					Node*p = temp->fail;
//比如如果字节点字符是a 但是失去匹配了 就要找要跳回那里去
					//这里先用p找当前节点的fail节点
					while (p != NULL)
					{
						if (p->next[i] != NULL)//如果前个节点的下个节点字符为a的节点不为空 那么就跳回到这个节点去
						{
							temp->next[i]->fail = p->next[i];//更新
							break;//跳出
						}
						p = p->fail;//否则一直回溯 一直到根节点 根节点的fail指针为空 其他都不为空 这时候跳出
					}
					if (p == NULL) temp->next[i]->fail = root;//如果当前节点的fail指针已经为空的话 那fail指针就指向root
				}
			}
		}
	}
}
int Ac_Automata(string ss, Node*root)
{
	int cnt = 0;//统计多少个单词在长串中出现过
	Node *p = root;//当前所在节点
	int len = ss.length();
	for (int i = 0; i < len; i++)
	{
		int c = ss[i] - 'a';//映射
		while (p->next[c] == NULL && p != root) 
p = p->fail;//失去匹配的时候 和kmp里面很像
		p = p->next[c];
		if (!p) p = root;//如果为空的话 回到起点
		Node*temp = p;
		while (temp != root)//需要遍历一下当前节点的每个fail节点 确保没有漏算
		{
			if (temp->End > 0)
			{
				cnt += temp->End;
				temp->End = 0;//因为这题说的是 有多少单词出现过 所以出现过 统计后 就要清空
				//如果说的是 单词一共出现了多少次的话 这里不能清空
			}
			else break;
			temp = temp->fail;
		}
	}
	return cnt;//返回
}
void destroy(Node*root)//遍历删点 一个小dfs
{
	if (root)
	{
		for (int i = 0; i < 26; i++)
		{
			if (root->next[i]) destroy(root->next[i]);
		}
	}
	delete root;
}
int main()
{
	int T;
	while (scanf("%d", &T) != EOF)
	{
		while (T--)
		{
			Node *root = createNew();//新建根节点
			string ss;
			int N;
			scanf("%d", &N);
			for (int i = 0; i < N; i++)
			{
				cin >> ss;
				Insert(ss, root);//在trie树上插入单词
			}
			GetFail(root);//寻找每个节点失去匹配以后要跳转的节点
			cin >> ss;
			cout << Ac_Automata(ss, root) << endl;//跑自动机
			destroy(root);//清空数据
		}
	}
}
```

