###k进制快速幂优化原理

对于$a^n$运用k进制快速幂，可以将指数n用k进制表示

$n=b_m*k^m+b_{m-1}*k{m-1}+\cdots+b_0*k^0$

$\therefore a^n=\prod_0^m a^{b_j*k^j}$

当k足够大的时候，可以处理很复杂的快速幂

```
ll ksm(ll a, ll b)//普通快速幂（基于二进制）
{
	ll ans = 1;
	while (b)
	{
		if (b & 1) ans = ans * a%mod;
		a = a * a%mod;
		b >>= 1;
	}
	return ans;
}

//base[i][1]   :   a^1

//base[i][j]   :  j*a^i

ll base[6][MAXN];

void init(ll a, ll k)//这里将a的j*(k^m)次方预处理
{
	base[0][1] = a % mod;
	int up = 3;//估计基数的指数
	for (int i = 1; i <= up; i++) base[i][0] = 1;//i相当于k进制位数，即k的指数
	for (int i = 0; i <= up; i++)
	{
		if (i) base[i][1] = base[i - 1][k];
		for (int j = 2; j <= k; j++)
		{
			base[i][j] = base[i][j - 1] * base[i][1] % mod;
		}
	}
}

ll k_ksm(ll a, ll b)//k进制快速幂
{
	int k = 65536;
	init(a, k);
	ll ans = 1;
	int u = 0;
	while (b)
	{
		ans = ans * base[u++][b%k] % mod;//连乘
		b /= k;//k进制分解
	}
	return ans;
}
```

题目运用：2019ICPC南昌网络赛 the Nth item

F(0)=0,F(1)=1
F(n)=3∗F(n−1)+2∗F(n−2),(n≥2) 
We have some queries. For each query N, the answer A is the value F(N) modulo 998244353.

在公式里要求$26219973^n$和$736044383^n$
由于询问次数达到1e7
故须要用k进制快速幂加速
这里用65536进制，直接用快速幂分步计算
$x^n=(x^{k^3}\% m)^{n/k^3}*(x^{k^2}\% m)^{(n\%k^3)/k^2}*(x^k\%m)^{(n\%k^2)/k}*x^{n\%k}$

```
long long inv17 = 524399943;//根号17
long long ans1 = 262199973, ans11 = 133071688, ans12 = 296255346, ans13 = 253151293;
long long ans2 = 736044383, ans21 = 19600266, ans22 = 683624219, ans23 = 530734902;
//都是预处理用的数字(事先用快速幂将底数算出来)
long long f1(long long ans, long long n)//优化快速幂
{
	long long x0 = n / 281474976710656LL;
	long long x1 = x1 = (n % 281474976710656LL) / 4294967296LL;
	long long x2 = (n % 4294967296LL) / 65536LL;
	long long x3 = n % 65536LL;
	long long sum = (((((qsm(ans12, x1, mod) % mod)*qsm(ans11, x2, mod) % mod) % mod)*qsm(ans1, x3, mod) % mod)*qsm(ans13, x0, mod) % mod) % mod;
	return sum;
}
long long f2(long long ans, long long n)
{
	long long x0 = n / 281474976710656LL;
	long long x1 = x1 = (n % 281474976710656LL) / 4294967296LL;
	long long x2 = (n % 4294967296LL) / 65536LL;
	long long x3 = n % 65536LL;
	long long sum = ((((qsm(ans22, x1%mod, mod) % mod)*(qsm(ans21, x2, mod) % mod) % mod)*qsm(ans2, x3, mod) % mod)*qsm(ans23, x0, mod) % mod) % mod;
	return sum;
}
```