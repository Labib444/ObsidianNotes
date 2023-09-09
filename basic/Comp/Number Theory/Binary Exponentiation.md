```c++
int power(int a, int b){
	int res = 1;
	while(b){
		if( b&1 ) res *= a;
		a *= a;
		b >> 1;
	}
	return res;
}
```

### Capping to the last value using Mod

```C++
long long power(long long a, long long b){
	long long res = 1;
	while( b ){
		if( b & 1 ) res *= a, res %= mod;
		a *= a; a %= mod;
		b >> 1;
	}
	return res;
}
```

### (a x b)%c Where a,b,c <= 10^15

Here a^15 * b^15 is not possible. Imagine (a^15-1) x (b^15 - 1) and c = c^15 then we can't calculate such large numbers.

```
int multiply(int a, int b, int c){
	int res = 0;
	while(b){
		if(b & 1) res = res + a, res %= c;
		a += a; a %= c;
		b >> 1;
	}
	return res;
}
```

### Mod

```C#
(a*b) % mod = (a*b) % mod // a < mod and b < mod
			= (a%mod * b%mod) % mod
```

