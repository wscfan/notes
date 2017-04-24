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
