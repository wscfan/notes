# python 办公

## 一、文件处理

### 1、输出目录下所有文件及文件夹

+ 获取当前python程序运行目录

  ```python
  import os
  print(os.getpwd())
  ```

+ 路径连接

  ```python
  import os
  print(os.path.join('pythonTest', 'test1'))
  ```

+ 列出某个文件夹下所有文件和文件夹

  ```python
  import os
  print(os.listdir('D:/mycode/'))
  ```

+ 循环判断指定文件夹中哪些是文件夹

  ```python
  import os
  files = os.listdir()
  for file in files:
      print(file, os.path.isdir(file))
  ```

+ `os.scandir()` 遍历 **(更推荐)** 

  ```python
  import os
  for file in os.scandir('D:/mycode/'):
      print(file.name, file.path, file.is_dir())
  ```

  **练习：**  列出当前目录包含hello字符串的文件的个数（不区分大小写）

  ```python
  import os
  num = 0
  for file in os.scandir('./'):
      if not file.is_dir():
          if 'hello' in file.name.lower():
              num += 1
  print("包含hello的文件个数：", num)
  ```

  

  

  









