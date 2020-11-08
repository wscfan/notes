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
+ **练习：** 列出当前目录包含hello字符串的文件的个数（不区分大小写） 

  ```python
  import os
  
  num = 0
  for file in os.scandir('./'):
  if not file.is_dir():
  if 'hello' in file.name.lower():
  num += 1
  print("包含hello的文件个数：", num)
  ```

  

### 2、遍历、搜索文件及查询文件信息

+ 遍历文件夹

  ```python
  import os
  
  for root, dirs, files in os.walk('./'):
      print(f'发现文件夹：{root})')
      print(f'文件夹列表：{dirs}')
      print(f'文件列表：{files}')
    print('--------------------------')
  ```

+ 是否某字符串开头、结尾

  ```python
  print('abc.txt'.startswith('ab'))
  print('abc.txt'.endswith('.txt'))
  ```

+ 搜索指定的文件

  | 模式   | 意义                    |
  | ------ | ----------------------- |
  | *      | 匹配所有                |
  | ？     | 匹配任何单个字符        |
  | [seq]  | 匹配seq中的任何字符     |
  | [!seq] | 匹配任何不在seq中的字符 |

  ```python
  import glob
  print(glob.glob('*.txt'))
  ```

+ 递归搜索指定文件

  ```python
  import glob
  print(glob.glob('**/*.txt', recursive=True))
  ```

+ 匹配文件名

  ```python
  import fnmatch
  
  print(fnmatch.fnmatch('lesson1.py', 'le*1.py'))
  print(fnmatch.fnmatch('lesson1.py', 'le*[0-9].py'))
  ```

+ 查询文件信息

  ```python
  import os
  
  import time
  for file in os.scandir():
      print(file.name, file.stat().st_size/1024/1024, time.ctime(file.stat().st_mtime))
  ```

+ datetime模块输出时间

  ```python
  import datetime
  
  that_time = datetime.datetime.fromtimestamp(1567764428)
  print(that_time)
  print(that_time.year, that_time.month, that_time.day, that_time.hour, that_time.minute, that_time.second)
  ```

+ **练习：** 递归搜索当前文件夹中今年修改的后缀名为 `.zip` 的所有文件

  ```python
  import os
  import glob
  import datetime
  
  now_year = datetime.datetime.now().year
  print('当前目录以下文件为今年的.zip压缩文件：')
  for file in glob.glob('**/*.zip', recursive=True):
    file_year = datetime.datetime.fromtimestamp(os.stat(file).st_mtime).year
      if file_year == now_year:
          print(file, f'文件大小为：{os.stat(file).st_size/1024}KB')
  ```

### 3、创建临时文件及文件夹

+ 读取文件

  ```python
  f = open('test.txt', 'r', encoding='utf-8')
  text = f.readlines()
  print(text)
  f.close()
  ```

  with ... as ... 写法：

  ```python
  with open('test.txt', 'r', encoding='utf-8') as f:
      text = f.readlines()
      print(text)
  ```

+ 写入文件

  ```python
  with open('test2.txt', 'w', encoding='utf-8') as f:
      f.write('hello world')
  ```

+ 创建临时文件

  ```python
  from tempfile import TemporaryFile
  
  f = TemporaryFile('w+')
  f.write('hello world')
  f.seek(0)
  data = f.readlines()
  print(data)
  f.close()
  ```

+ 创建临时文件夹

  ```python
  from tempfile import TemporaryDirectory
  
  with TemporaryDirectory() as tmp_folder:
      print(f'临时文件夹:{tmp_folder}')
  ```

### 4、批量创建、复制、移动、删除、重命名文件及文件夹







































