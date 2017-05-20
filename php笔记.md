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