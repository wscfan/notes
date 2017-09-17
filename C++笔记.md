# C++笔记

## 1、C++基本语法
### 1.1 `cin`
以空格、tab键、回车键作为分隔符。如果需要输入带空格的字符串，则使用 `cin.getline(s,50)`

### 1.2 常量
定义常量的两种方法：
1. `const 数据类型 常量名 = 值;`
2. 在main函数之前定义 `#define 常量名 值` ，末尾没有分号

```c++
#include <iostream>
using namespace std;
#define WS "Hello World!"
int main(){
	cout<<WS<<endl;
	const double PI = 3.14;
	cout<<PI<<endl;
	return 0;
} 
```

## 2、C++运算
### 2.1 `pow(x,y)`
1. x的y次方。  
2. x,y均为双精度实数。
3. 数学函数包含在头文件cmath中。

```c++
 #include<iostream>
 #include<cmath>
 using namespace std;
 int main(){
 	double money,years,rate,sum;
 	cin>>money>>years>>rate;
 	sum=money*pow((1+rate),years);
 	cout<<sum<<endl;
 }
```