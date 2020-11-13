## GCD

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
ll gcd(ll a, ll b)
{
	return b==0?a:gcd(b,a%b);
}
ll x,y;
int main()
{
	cin>>x>>y;
	cout<<gcd(x,y)<<endl;
	return 0;
}
```