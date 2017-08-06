# php笔记

[toc]

## 一、Apache、PHP、MySQL安装
### 1、Apache的安装
#### 1.1 安装：
1. 在dos界面进入安装包的bin目录下
2. 执行语句：httpd -k install

#### 1.2 卸载：
1. 在dos界面进入安装包的bin目录下
2. 执行语句：httpd -k uninstall

#### 1.3 Apache常见命令：
1. httpd -k start （启动）
2. httpd -k restart （重启）
3. httpd -k stop （停止）
4. httpd -t （检查配置文件是否出错）
5. httpd -h （显示有哪些指令可以使用）

#### 1.4 查看端口监听：
netstat -an  
netstat -anb | more （可以看到监听程序）

#### 1.5 修改Apache根目录
1. 打开 conf/httpd.conf 文件
2. 查找 Apache24，更改所有路径

#### 1.6 修改端口号：
1. 打开 conf/httpd.conf 文件
2. 修改 Listen 80

#### 1.7 解决Apache启动时出现的错误信息
1. 打开 conf/httpd.conf 文件
2. 查找 ServerName，启动 ServerName localhost:80

#### 1.8 Apache虚拟主机配置
1. 打开 Apache24/conf/extra/httpd-vhosts.conf 文件
2. 在文件末尾添加下面的代码：
```
<VirtualHost *:80>
    DocumentRoot "c:/wamp/Apache24/htdocs/wsweb"
    ServerName wsweb.com
</VirtualHost>
```
3. 重启Apache

### 2、PHP安装
#### 2.1 PHP和Apache整合
1. 打开 Apache24/conf/httpd.conf 文件
2. 在 #LoadModule 下面插入如下代码：
```
LoadModule php5_module "{php路径}/php5.6.16/php5apache2_4.dll"
<FilesMatch \.php$>
	SetHandler application/x-httpd-php
</FilesMatch>
PHPIniDir "{php路径}/php5.6.16/"
```
3. 修改php中的 php.ini-development 文件为 php.ini
4. 打开 php.ini 文件，并在 ;extension=... 后面加入以下代码：
```
extension_dir="{php路径}/php5.6.16/ext"
```

#### 2.2 php命令行模式
在命令行中输入：
```dos
php.exe -f 文件路径
```

### 3、MySQL安装
#### 3.1 查看MySQL是否安装成功
1. 打开 windows 服务，看是否有MySQL
2. netstat -anb | more，看是否有3306端口在监听
3. 是否可以登录到mysql

#### 3.2 MySQL常见命令
1. net start mysql （启动）
2. net stop mysql （停止）

#### 3.3 MySQL和PHP整合
1. 打开 php/php.ini 文件
2. 取消注释 extension=php_mysql.dll
3. 重启Apache

### 4、phpMyAdmin安装
#### 4.1 安装过程
1. 在浏览器中输入 http://localhost/phpMyAdmin/setup/index.php
2. 执行时，提示错误，需要编辑php中的 php.ini 文件，取消注释：extension=php_mbstring.dll，并重启Apache
3. 打开后提示错误，可根据错误编辑php中的 php.ini 文件，取消注释：extension=php_bz2.dll，并重启Apache
4. 根据错误，在phpMyAdmin中新建config文件夹，并使其可写
5. 浏览器中输入 http://localhost/phpMyAdmin/index.php 进入登录界面
6. 出现提示错误，可到php中的 php.ini 文件中取消注释 extension=php_mysqli.dll ，并重启Apache

## 二、PHP
### 1、PHP数据类型
#### 1.1 基本数据类型
##### 1.1.1基本数据类型
1. 整型
2. 浮点型
3. 布尔型
4. 字符串型
##### 1.1.2 复合数据类型
1. 数组
2. 对象
##### 1.1.3 特殊数据类型
1. null
2. 资源

#### 1.2 字符串的定义方法
1. 单引号
2. 双引号
3. heredoc 语法结构
4. nowdoc 语法结构（PHP5.3.0以后）  

**1.2.1 单引号特点**
1. 不会去解析 $
2. 执行效率高

**1.2.2 双引号特点**
1. 会解析 $
2. 执行效率没有单引号高
3.  如果在双引号中要解析特殊字符或者数组，则需要用{}括起来

**1.2.3 heredoc使用**  
一般用于返回大段的HTML代码
```php
$div_info=<<<HTMLCON
<h1>hello,world!</h1>$num
HTMLCON;
```
1. 会解析 $
2. <<<标识符后面的HTMLCON可以自己命名，一般大写
3. <<<HTMLCON 后面不要有其他字符
4. <<<HTMLCON 和后面的定界标识符 HTMLCON 相等
5. 最后的 HTMLCON; 定格写

**1.2.4 nowdoc使用**  
类似单引号的使用，不解析 $  
```php
$div_info=<<<'HTMLCON'
<h1>hello,world!</h1>$num
HTMLCON;
```

#### 1.3 字符串的转义
1. 在双引号中，使用\x40，表示输出16进制的值对应的 ascii 字符
2. \101 表示输出8进制的值对应的 ascii 码字符


#### 1.4 数据类型转换
##### 1.4.1 自动数据类型转换  
当进行运算时，数据类型向高精度自动转换。精度的高低：布尔 < int < float  

##### 1.4.2 强制数据类型转换  
```php
//settype
/*
  可以将 $num 强制转换成：
  "boolean" （或为"bool"，从 PHP4.2.0 起）
   "integer" （或为"int"，从 PHP4.2.0 起）
   "float" （只在 PHP4.2.0 之后可以使用，对于旧版本中使用的 "double" 现已停用）
   "string"
   "array"
   "object"
   "null" （从 PHP4.2.0 起）
*/
$num = 100;
settype($num,"string");
var_dump($num);
```
```php
//使用（类型）变量的方式转换
//$num2 的类型不变
$num2 = 200;
$a = (string)$num2;
var_dump($a);
```
```php
//intval、floatval、stringval ...
//$num3 的类型不变
$num3 = 400;
$b = floatvar($num3);
var_dump($b,$num3);
```
三种方式的区别和使用：
1. 第一种方式会把变量本身修改成新的数据类型，支持的转变数据的类型很多；
2. 第二种方式，支持的转变数据的类型很多，可以转成数组类型；
3. 第三种方式，重点是把变量的值返回，不支持返回数组类型。  

###  2、变量相关函数
#### 2.1 isset()  
isset(变量)，用于检测某个变量是否存在，并且不为 null
```php
$a = 100;
if(isset($a)){
	echo '$a 存在，并且不为 null'
}
```

#### 2.2 unset()  
销毁指定的变量  
- 如果在函数中 unset() 一个全局变量，则只是局部变量被销毁
- 如果想在函数中 unset() 一个全局变量，可以使用 $GLOBALS 数组来实现
```php
$b = 100;
if(isset($b)){
	echo '$b 存在';
}
unset($b);
if(isset($b)){
	echo '$b 存在';
}else{
	echo '$b 不存在';
}
```

#### 2.3 empty()  
检测一个变量是否为空
```php
$c = 'hello';
if(empty($c)){
	echo '$c 为空';
}else{
	echo '$c 不为空';
}
```

#### 2.4 gettype()  
返回某个变量的数据类型
```php
$d = 123;
echo gettype($d);
```

#### 2.5 判断是否是某个数据类型  
`is_int()`、`is_float()`、`is_array()`、...

###  3、数据的传递方式
#### 3.1 值传递
```php
$a = 123;
$b = $a;
echo $b;
$b = 45;
echo '<br>' . $a;
```

#### 3.2 引用传递
```php
$a = 123;
$b = &$a;
echo $b;
$b = 45;
echo '<br>' . $a;
```

#### 3.3 说明  
- int ， float ， boolearn ， string 都是默认值传递
- array 数据类型默认值传递
- 对象的传递方式，也是值传递，传递的是对象标识符
- null 数据类型也是默认值传递
- resource 数据类型也是默认值传递

### 4、变量
#### 4.1 变量命名
1. PHP 变量前必须带上 $ 符号
2. PHP 变量区分大小写，函数不区分大小写
3. PHP 变量可以是关键字，但不推荐

### 5、运算符
#### 5.1 运算符优先级
- **|| 和 or 区别**
	1. 他们的优先级是 || 大于 = 大于 or
	2. 可以给使用 () 改变优先级

#### 5.2 字符串运算符
1. 字符串和数字连接时，使用 `.` 优先转换成字符串
2. 字符串和数字连接时，使用 `+` 优先转换成数字

#### 5.3 类型运算符
- `instance of` 用于确定一个 PHP 变量是否属于某一类的实例
```
class Dog{
}
$dog1=new Dog();
if($dog1 instanceof Dog){
    echo '$dog是Dog对象的实例';
}else{
    echo '$dog不是Dog对象的实例';
}
```

#### 5.4 执行运算符
- 基本说明
PHP 支持一个执行运算符：反引号（\`\`）, PHP 将尝试将反引号中的内容作为外壳命令来执行，并将其输出信息返回。使用反引号运算符的效果与函数 `shell_exec()` 相同。
```
echo '<pre>';
echo `netstat -anb`;
```

#### 5.5 错误控制运算符
PHP 支持一个错误控制运算符：`@`。当将其放在一个 PHP 表达式之前，该表达式可能产生的任何错误信息都被忽略掉。  
`@` 一般可以和 `die` 配合使用，使致命错误不被忽略。
```
$con = @mysql_connect('localhost','root','root') or die(mysql_error());
if($con){
	echo 'ok';
}else{
	echo 'error';
}
```

#### 5.6 位运算符
- `&` 按位与
- `|` 按位或
- `^` 按位异或
- `~` 按位取反
- `<<` 左移
- `>>` 右移
##### 5.6.1 二进制
1. 二进制的最高位是符号位：0表示正数，1表示负数
2. 正数的原码，反码，补码都一样
3. 负数的反码=它的原码符号位不变，其它位取反
4. 负数的补码=它的反码+1
5. 0的反码、补码都是0
6. php没有无符号数，换言之，php中的数都是有符号的
7. 在计算机运算的时候，都是以补码的方式来运算的


### 6、流程控制
#### 6.1 顺序控制
#### 6.2 分支控制
1. 单分支
2. 双分支
3. 多分支

- **单分支控制替代语法：**
```html
<!--模板文件-->
<?php if($age > 18):?>
<h1>这是我的模板文件</h1>
<?php endif;?>
```
```php
// PHP文件
$age=19;
require 'mytpl.html';
```
- **双分支控制替代语法：**
```html
<!--模板文件-->
<?php if($age > 18):?>
<h1>年龄大于18岁</h1>
<?php else:?>
<h2>年龄小于18岁</h2>
<?php endif;?>
```
```php
// PHP文件
$age=16;
require 'mytpl.html';
```

- **多分支分支控制替代语法：**
```html
<!--模板文件-->
<?php if($age > 18):?>
<h1>年龄大于18岁</h1>
<?php elseif($age < 16):?>
<h2>年龄小于16岁</h2>
<?php else:?>
<h3>年龄在16岁到18岁之间</h3>
<?php endif;?>
```
```php
// PHP文件
$age=17;
require 'mytpl.html';
```

### 6、循环控制
1. for 循环
2. while 循环
3. dowhile 循环

- **goto 语句**
```php
goto a;
echo 'aa';
a:
echo 'bb';
```

### 7、常量
1. unset() 中不能使用常量
2. 常量前面没有美元符号（$）
3. 常量用 define() 函数定义，而不能通过赋值语句
4. 常量也可以通过 const 来定义
5. 常量的命名规范是“字母大写加下划线”，比如 `TAX_RATE`

```php
// 常量使用
define('TAX_RATE',0.08);
$salary = 10000;
$tax = $salary * TAX_RATE;
echo 'tax=' . $tax;
```

#### 7.1 预定义常量和魔术常量
- **预定义常量**
- **魔术常量**

|魔术常量|说明|
|----|----|
|\_\_LINE\_\_|显示当前是第几行|
|\_\_DIR\_\_|显示当前文件的路径，不含文件名|
|\_\_FILE\_\_|显示当前文件的路径，含文件名|
|\_\_FUNCTION\_\_|显示当前函数的函数名|

#### 7.2 变量名、函数名、常量名的可变性讨论
```php
$a='Hello World!';
$var='a';
echo $$var;
```
```php
function test(){
	echo 'Hello World!';
}
$fun='test';
$fun();
```
```php
define('PI',3.14);
$my_pi='PI';
echo constant($my_pi);
```

### 8、函数
#### 8.1 文件引入的基本使用
1. require
	- 当 require 的文件不存在时，直接退出
	- 当 require 文件时，不会判断之前是否引用过，而是会重复引入
2. require_once
	- 当 require_once 的文件不存在时，直接退出
	- 当 require_once 文件时，会判断之前是否引用过，不会重复引入，可以防止引入时，出现函数重复定义问题，而且有利于性能提高，推荐使用
3. include
	- 当 include 的文件不存在时，不会退出，会继续执行
	- 当 include 文件时，不会判断之前是否引用过，而是会重复引用
4. include_once
	- 当 include_once 的文件不存在时，不会退出，会继续执行
	- 当 include_once 文件时，会判断之前是否引用过，不会重复引用

#### 8.2 被引入文件中的 return 说明
1. 在我们引入一个文件时，默认情况下返回 1
2. 在引入文件的过程中，当遇到被引入文件的 return 语句时，引入过程将终止，返回主文件，继续执行。 

#### 8.3 全局变量  
函数中使用全局变量时，用 global
```php
$a=10;
function aa(){
	global $a;  // 等价于 $a=&$GLOBALS['a'];
	$a=20;
	echo $a;;
}
aa();
echo $a;
```

#### 8.4 静态变量  
静态变量的特点是，只会被初始化一次。
```php
function test(){
	static $n=0;
	$n++;
	echo $n;
}
test();  // 1
test();  // 2
test();  // 3
```

#### 8.5 函数参数的引用传递
默认情况下，函数参数通过值传递，如果想让函数的参数通过引用传递，可在参数的前面加上 &
```php
$a=10;
function test(&$b){
	$b=20;
}
test($a);
echo $a;  // 20
```

#### 8.6 内部函数
1. 在没有调用外部函数时，内部函数只能在内部使用；
2. 在调用了外部函数后，内部函数也可以在外部使用。

#### 8.7 可变函数
```php
function test($a,$b){
	echo $a+$b;
}
$v='test';
$v(20,50);
```

#### 8.8 获取参数
- `func_num_args()` 返回参数的个数
- `func_get_args()` 返回所有参数的值，并且是数组
- `func_get_arg(数字)` 返回指定的那个参数

#### 8.9 匿名函数
- 匿名函数作为回调函数

#### 8.10 变量作用域
1. 局部作用域
2. 全局作用域
3. 超全局作用域

```php
$a = 12;
function test(){
	var_dump($GLOBALS);
}
test();
```

### 9、数组
- 不能用数组和对象作为键(key)，这样做会导致一个警告：illegal offsettype
- php 数组是可以自动增长的
#### 9.1 数组创建方式
1. 第一种，对数组的每一个元素赋值：
```php
$arr[0]=1;
$arr[1]='Hello World';
```
2. 第二种：
```php
$arr=array('张三','李四','王五');
```
3. 第三种：
```php
$arr=array('no1'=>'张三','no2'=>'李四','no3'=>'王五');
```

#### 9.2 数组的自动增长
```php
$arr=array();
for($i=0; $i<20; $i++){
	$arr[]=$i;
}
var_dump($arr);
```

#### 9.3 数组的常用函数
|函数名|说明|
|-----|----|
|count()|统计数组条数|
|is_array()|判断是否为数组|
|sort()|排序|
|usort()|使用用户自定义的比较函数对数组中的值进行排序|
|array_merge()|合并数组|
|array_reverse()|数组颠倒|
|array_search()|搜索值|
|array_pop()|将数组最后一个单元弹出（出栈）|
|array_push()|将一个或多个单元压入数组的末尾（入栈）|

```php
$arr=array(12,34,45,2,34,4,23,5,3,39,34,344);
sort($arr);
var_dump($arr);
```
```php
usort($arr,function($n1,$n2){
	if(strlen($n1 == strlen($n2))){
		return 0;
	}else{
		return strlen($n1) < strlen($n2) ? 1 : -1;
	}
});
var_dump($arr);
```
```php
$arr = array('a','b','c','d','f');
$arr=array_reverse($arr);
var_dump($arr);
$arr_last = array_pop($arr);
$arr_last = strtoupper($arr_last);
array_push($arr,$arr_last);
var_dump($arr);  // f d c b A
```

#### 9.4 删除数组或数组项
```php
$arr = array('a','b','c','d');
unset($arr);  // 删除数组
unset($arr[2]);  // 删除数组项
```

#### 9.5 数组的空间分配机制
|函数名|说明|
|-----|----|
|memory_get_usage()|返回分配给 PHP 的内存量|

#### 9.6 数组的运算符
```PHP
$arr1 = array(1,6);
$arr2 = array(2,8,9);
$arr3 = $arr1 + $arr2;
print_r($arr3);  // 1 6 9
```

#### 9.7 排序
##### 9.7.1 冒泡排序
```php
// 代码优化之后，速度有很大提升
$arr = array(12,23,34,1,-12,26,56,234,237,9);
function bubble(&$arr){
	$flag = 0;
	$arr_len=count($arr);
	for($i = 0; $i < $arr_len-1; $i++){
		for($j = 0; $j < $arr_len-1-$i; $j++){
			if($arr[$j] > $arr[$j + 1]){
				$temp=$arr[$j];
				$arr[$j]=$arr[$j+1];
				$arr[$j+1]=$temp;
				$flag = 1;
			}
		}
		if($flag == 0){
			break;
		}else{
			$flag = 0;
		}
	}
}
date_default_timezone_set('PRC');
echo '<br>' . date('H:i:s');
bubble($arr);
echo '<br>' . date('H:i:s');
echo '<pre>';
print_r($arr);
```

##### 9.7.2 选择排序
```php
$arr = array(12,34,99,3,45,103,4);
function selectSort(&$arr){
	$arr_len=count($arr);
	for($i = 0; $i < $arr_len; $i++){
		$arr_min = $arr[$i];
		$arr_index = $i;
		for($j = $i; $j < $arr_len; $j++){
			if($arr_min > $arr[$j]){
				$arr_min = $arr[$j];
				$arr_index = $j;
			}
		}
		if($arr_index != $i){
			$arr[$arr_index] = $arr[$i];
			$arr[$i] = $arr_min;
		}
	}
}
selectSort($arr);
echo '<pre>';
print_r($arr);
```

##### 9.7.3 插入排序
```php
$arr = array(12,3,436,43,76,10);
function insertSort(&$arr){
	$arr_len = count($arr);
	for($i = 1; $i < $arr_len; $i++){
		$index = $i - 1;
		$arrItem = $arr[$i];
		while($index >= 0 && $arr[$index] > $arrItem){
			$arr[$index+1] = $arr[$index];
			$index--;
		}
		$arr[$index+1] = $arrItem;
	}
}
insertSort($arr);
echo '<pre>';
print_r($arr);
```

##### 9.7.4 快速排序

##### 9.7.5 四种排序的速度
冒泡排序 < 选择排序 < 插入排序 < 快速排序

#### 9.8 查找
##### 9.8.1 顺序查找
```php
$arr = array(12,3,34,45,111,23,5,67,23);
function search($arr,$item){
	$arr_len = count($arr);
	$flag = 0;
	for($i = 0; $i < $arr_len; $i++){
		if($item == $arr[$i]){
			echo '找到了，索引值为：' . $i . '<br>';
			$flag = 1;
		}
	}
	if($flag == 0){
		echo '很抱歉，没有找到哦。';
	}
}
search($arr,23);
```

##### 9.8.2 二分法查找
二分法查找适用于查找有序的数组。
```php
$arr = array(1,3,6,12,22,34,45,56,67,99,122,155);
function searchNum($left_index,$right_index,$arr,$item){
	if($left_index > $right_index){
		echo '很抱歉，没有找到。';
		return;
	}
	$mid_index = round(($left_index + $right_index) / 2);
	if($arr[$mid_index] == $item){
		echo '找到了，索引值为：' . $mid_index;
	}else if($arr[$mid_index] > $item){
		searchNum($left_index,$mid_index-1,$arr,$item);
	}else if($arr[$mid_index] < $item){
		searchNum($mid_index+1,$right_index,$arr,$item);
	}
}
searchNum(0,count($arr)-1,$arr,288);
```

#### 9.9 二维数组
```php
$arr = array(array(12,23,34),array(123,345,333),100);
$arr_len = count($arr);
for($i = 0; $i < $arr_len; $i++){
	$arr2_len = count($arr[$i]);
	if(is_array($arr[$i])){
		for($j = 0; $j < $arr2_len; $j++){
			echo $arr[$i][$j] . ' ';
		}
	}else{
		echo $arr[$i];
	}
	echo '<br>';
}
```
```php
$arr = array(array(12,23,34),array(123,345,333),100);
foreach($arr as $val){
	if(is_array($val)){
		foreach($val as $v){
			echo $v . ' ';
		}
	}else{
		echo $val . ' ';
	}
	echo '<br>';
}
```

### 10、PHP面向对象
#### 10.1 类的定义
1. 类的定义有一个关键字，就是 class
2. 成员属性是指的类中定义的变量
3. 成员属性前面有修饰符（public），public 就是访问控制符，oop 有三种（public、protected、private）

```php
class Person{
	public $name;
}
$p1 = new Person();
$p1 -> name = '张三';
echo $p1 -> name;
```
#### 10.2 成员函数
```php
class Person{
	public function sum($a,$b){
		return $a + $b;
	}
}
$p = new Person();
$num = $p->sum(10,20);
echo '结果为：' . $num;
```

#### 10.3 构造方法
构造方法是类的一种特殊的方法，它的主要作用是完成对新对象的初始化。
1. 没有返回值
2. 在创建一个类的新对象时，系统会自动地调用该类的构造方法完成对新对象的初始化。
3. 在 PHP4 中，它的构造函数和类名一样。
4. 在 PHP5 中，使用 `__construct` 方式来定义构造函数。
5. 一个类中只能有一个构造函数。

```php
class Person{
	public $name;
	public $age;
	public function __construct($my_name,$my_age){
		$this->name = $my_name;
		$this->age = $my_age;
	}
}
$p1 = new Person('张三',22);
echo $p1->name . ':' . $p1->age . '岁';
```

#### 10.4 析构函数
析构函数会在某个对象的所有引用都被删除或者当对象被显式销毁时执行。析构函数主要作用是去释放对象被分配的相关资源。
```php
function __destruct(){
	// 释放资源操作
}
```

#### 10.5 魔术修饰符
1. public（公有）：类成员可以在任何地方被访问；
2. protected（受保护）：类成员可以被其自身以及其子类和父类访问；
3. private（私有）：类成员只能被其定义所在的类访问。
```php
class Test{
	public $a;
	protected $b;
	private $c;
	public function __construct($aa){
		$this->a = $aa;
	}
	function fn($bb){
		$this->b = $bb;
		return $this->b;
	}
	function fn2($cc){
		$this->c = $cc;
		return $this->c;
	}
}
$test = new Test(12);
echo $test->a . '<br>';
echo $test->fn(23) . '<br>';
echo $test->fn2(34);
```

#### 10.6 魔术方法  
`__get` ，读取不可访问属性的值（如 private/protected/不存在）时，`__get()` 会被调用。  
`__set` ，在给不可访问属性赋值（如 private/protected/不存在）时，`__set()` 会被调用。
```php
class Person{
	public $name;
	protected $age;
	function __construct($name,$age){
		$this->name = $name;
		$this->age = $age;
	}
	function __get($p_age){
		if(isset($this->$p_age)){
			return $this->$p_age;
		}else{
			return '<br>属性不存在。';
		}
	}
}
$p1 = new Person('张三',19);
echo $p1->name . '<br>';
echo $p1->age;
```
```php
class Person{
	public $name;
	protected $age;
	function __construct($name,$age){
		$this->name = $name;
		$this->age = $age;
	}
	function __get($p_age){
		if(isset($this->$p_age)){
			return $this->$p_age;
		}else{
			return '属性不存在。<br>';
		}
	}
	function __set($pro_name,$val){
		if(isset($this->$pro_name)){
			$this->$pro_name = $val;
		}else{
			echo '属性不存在<br>';
		}
	}
}
$p1 = new Person('张三',19);
$p1->name = '李四';
$p1->age = 20;
echo $p1->name . '<br>';
echo $p1->age . '<br>';
```
`__isset` ，当对不可访问属性（如：private/protected/不存在）调用 `isset()` 或 `empty()` 时， `__isset()` 会被调用。  
`__unset` ，当对不可访问属性（如：private/protected/不存在）调用 `unset()` 时， `__unset()` 会被调用。
`__toString()` ，当我们把一个对象当做字符串使用时，会调用 `toString()` 方法。
`__clone()`
`__call()` ，在对象调用一个不可访问的方法时，`__call()` 会被调用。